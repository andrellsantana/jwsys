# ADR-003 — Papéis e permissões extensíveis

**Status:** proposto (04/11/2026).

## Contexto
- Hoje existem os papéis publicador, SG, secretário e CS. A CS é composta por coordenador, secretário e superintendente de serviço (CAD-005).
- Os módulos futuros (REU-designações, PAS, CTA, LIM, TER, SCT) trazem papéis novos: servo de contas, servo de territórios, coordenador de discursos, anciãos (pastoreio) etc.
- Uma pessoa acumula vários papéis (NUC-003), com escopo (ex.: SG **do Grupo 2**) e vigência (datas).
- O Firestore Security Rules não consulta tabelas de permissão sem custo de leitura, e os custom claims têm limite de **1.000 bytes**.

## Decisão

### 1. Três conceitos separados
| Conceito | O que é | Exemplo |
|---|---|---|
| **Designação** | Fato da congregação, registrado no Cadastro, com vigência | "Paulo Reis é superintendente de serviço desde 01/09/2026" |
| **Papel** | Conjunto de permissões declarado por um módulo | `atv.sg`, `atv.cs`, `cta.servo` |
| **Permissão** | Ação atômica `<módulo>.<recurso>.<ação>` | `atv.relatorio.aprovar`, `atv.peticao.aprovar` |

As designações geram os papéis automaticamente, por regras de derivação:
- coordenador, secretário ou SS → `atv.cs`;
- SG do grupo X → `atv.sg@X`;
- ancião → `pas.anciao` (futuro).

O secretário também pode atribuir um papel manualmente, com vigência e motivo.

### 2. Cada módulo declara um manifesto
```ts
// packages/atividades/manifesto.ts
export const manifesto = defineModulo({
  id: 'atv',
  permissoes: ['relatorio.enviar', 'relatorio.aprovar', 'relatorio.ler', 'grupo.ler', 'peticao.aprovar', 'mes.reabrir', ...],
  papeis: {
    publicador: { permissoes: ['relatorio.enviar', 'relatorio.lerProprio'], escopo: 'pessoa' },
    sg:         { permissoes: ['relatorio.aprovar', 'relatorio.ler', 'grupo.ler', 'anotacao.criar'], escopo: 'grupo' },
    secretario: { permissoes: ['*'], escopo: 'congregacao', exigeMFA: true },
    cs:         { permissoes: ['peticao.aprovar', 'peticao.reativar', 'apuracao.ler'], escopo: 'congregacao' },
  },
  derivacoes: [
    { deDesignacao: ['coordenador', 'secretario', 'superintendente_servico'], papel: 'cs' },
    { deDesignacao: 'superintendente_grupo', papel: 'sg', escopoDe: 'grupoId' },
  ],
});
```
- O Núcleo junta os manifestos no build e **gera**: (a) o catálogo `nuc_papeis` exibido ao secretário; (b) as funções de checagem das Security Rules; (c) o helper `pode(usuario, 'atv.relatorio.aprovar', {grupoId})` usado nas Cloud Functions.
- Um módulo novo entra só adicionando o seu manifesto. Nenhum código existente muda (regra do MAPA).

### 3. Armazenamento
```
cad_designacoes/{id}    { pubId, designacao, escopo?, desde, ate?, registradoPor }     (Cadastro)
nuc_atribuicoes/{id}    { pubId, papel: "atv.sg", escopo: "g2", desde, ate?, origem: derivada|manual, motivo? }
nuc_papeis/{papel}      catálogo gerado dos manifestos (somente leitura)
```

### 4. Claims compactos no token
`{ p: "<pubId>", g: "<grupoPub>", r: ["atv.sg@g2", "atv.sec", "atv.cs"], mfa: ["atv.sec"] }`
- Só os **papéis vigentes**, não as permissões. Cada papel fica com menos de 15 bytes, e o total cabe folgado no limite.
- As Security Rules geradas traduzem papel → permissão em tempo de compilação, sem leituras extras.
- **Mudança de designação ou atribuição** → a função recalcula os claims e chama `revokeRefreshTokens`. O acesso antigo cai em minutos (o SG que muda de grupo perde o anterior, CAD-002).
- **Vigência:** o job diário ativa e expira atribuições com `desde`/`ate` e reemite os claims.

### 5. Regras transversais
- **MFA por papel:** um papel com `exigeMFA` só vale se o login tiver segundo fator (secretário; futuros PAS e CTA).
- **Troca de papel na interface:** a UI mostra só os papéis presentes no token. A ação grava `papelUsado` na auditoria (NUC-006).
- **Separação de funções (opcional por módulo):** o manifesto pode exigir que duas pessoas diferentes participem (ex.: contas, conferência por 2). A autoaprovação do SG é permitida explicitamente (ATV-018).
- **Dados sensíveis por módulo:** PAS e CTA ficam em coleções próprias, sem `permissoes: ['*']` herdadas. Nem o secretário as vê sem o papel específico.
- **Negação por padrão:** sem papel = sem acesso. O publicador sem aparelho ativado só cria `atv_enviosNaoIdentificados` (NUC-002).

## Consequências
- O ADR-002 passa a usar `r[]` no lugar de `sgDe/secretario/cs`. O Cadastro ganha a coleção `cad_designacoes` e a tela "Designações" (substitui CAD-005 como mecanismo; a regra continua).
- Testes: para cada manifesto, testes gerados das Security Rules no Emulator (papel com e sem permissão, escopo certo e errado, vigência expirada).

## Alternativas descartadas
- **Lista fixa de papéis no código:** quebra a extensibilidade dos módulos futuros.
- **Permissões lidas do banco nas rules (`get()`):** custo de leitura em todo acesso e mais latência.
- **Todas as permissões no token:** estoura o limite de 1.000 bytes com os módulos futuros.
