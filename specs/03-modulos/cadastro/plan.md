# Cadastro (CAD) — Plano técnico

## Pacote
`packages/cadastro`: `manifesto.ts` (permissões `cad.*`, perfis declarados, derivações de papéis), `shared/` (tipos, validações CAD-008/011), `functions/` e `web/`.

## Coleções (prefixo `cad_`)
```
cad_publicadores/{pubId}        { nomeCompleto, nomeBusca, nascimento?, batismo?, sexo, esperanca,
                                  endereco: {tipoLogradouro, logradouro, numero, complemento, comunidade, bairro, cidade, uf, cep},
                                  contato: {telefone, email}, emergencia: {nome, telefone, email},
                                  grupoAtual, situacao: {tipo, desde, obs}, consentimentoRevogado: bool, criadoEm }
  /grupos/{AAAAMM}              { grupoId }          (histórico de grupo por vigência)
  /situacoes/{id}               { tipo, desde, obs, por }
  /saude/{id}                   { descricao, desde, ate?, por }
cad_grupos/{grupoId}            { nome, ativo, ordem }
cad_designacoes/{id}            { pubId, perfil, escopo?, desde, ate?, por, motivo? }
```
Os ajudantes são uma designação `AJUDANTE` com escopo de grupo, sem papel associado (Q-01).

## Regras de segurança
- Leitura de `cad_publicadores`: SEC e master (todos os campos); o próprio publicador (Q-30). O **SG não lê `cad_publicadores` diretamente**: usa a função `listarPublicadoresDoGrupo` (abaixo).
- Escrita: só via callables (`cadastrarPublicador`, `editarPublicador`, `registrarDesignacao`, `encerrarDesignacao`, `moverGrupo`, `registrarSituacao`, `gerirGrupo`).

## Leitura do SG por função no backend (decisão de 04/11/2026)
- A callable `listarPublicadoresDoGrupo()` (e `obterPublicador(pubId)`) roda no `comando()` do Núcleo:
  1. confere o papel `atv.sg@grupo`;
  2. consulta os membros do grupo no mês;
  3. busca no Firestore com **projeção de campos** (`select(...)` do Admin SDK), trazendo do banco só os campos permitidos (Q-28);
  4. devolve a resposta enxuta.
- Os campos não permitidos **nem saem do banco**. Não existe cópia duplicada para manter.
- **Custos e efeitos:** 1 chamada de função + 1 leitura por publicador. Isso fica dentro da cota grátis. A resposta não é em tempo real nem fica offline (aceitável para dados cadastrais, que mudam pouco). O app guarda a última resposta em memória durante a sessão.
- **Os relatórios do grupo continuam lidos direto do Firestore**, em tempo real, porque o SG pode ver o documento inteiro (CAD-002).

## Gatilhos
- `cad_designacoes` alterada → `nuc.recalcularAcesso(uid)`.
- `grupos/{mes}` alterado → atualiza `grupoAtual` e o recálculo de acesso.
- `situacao` alterada → evento para ATV (contagens e status).

## Validações (shared, usadas no app e nas Functions)
`validarPerfil(publicador, perfil, escopo, designacoesVigentes)` → erros do CAD-008. `possivelDuplicado(nomeBusca, nascimento)`.

## Testes
- Unidade: validações de perfil (sexo, batismo, ancião, titular único, SG único por grupo).
- Rules: SG não lê `cad_publicadores`; publicador vê só o próprio cadastro.
- Functions: `listarPublicadoresDoGrupo` devolve só os campos permitidos e só do grupo do SG (testes com campos proibidos ausentes).
- E2E: mover em lote 3 publicadores e conferir a visibilidade dos dois SGs.
