# Parâmetros (PAR) — Plano técnico

## Pacote
`packages/parametros`: `manifesto.ts` (o próprio catálogo de PAR), `shared/valor.ts`, `functions/` e `web/`.

## Coleções (prefixo `par_`)
```
par_catalogo/{chave}             gerado dos manifestos no build: { modulo, descricao, tipo, unidade, min, max, padrao }
par_versoes/{chave}_{AAAAMM}     { chave, valor, vigenteDesde: AAAAMM, motivo, por, em }
par_vigentes/atual               documento único: { [chave]: [{desde, valor}, ...], versao }   (leitura barata: 1 doc)
par_agenda/{AAAAMM}              { mesEspecial: bool, descricao, por, em }
```
Os termos ficam em `nuc_termos` (Núcleo). A tela de Parâmetros chama o contrato do Núcleo para publicá-los.

## Leitura (PAR-007)
- Cliente: `onSnapshot(par_vigentes/atual)` → cache em memória. É uma única leitura no início e atualização ao vivo.
- Functions: cache por instância + verificação de `versao` a cada execução (1 leitura).
- `valor(chave, mes)` = a última entrada com `desde ≤ mes`; se não houver, o `padrao` do catálogo.

## Comandos
`alterarParametro(chave, valor, desde, motivo)` e `marcarMesEspecial(mes, especial, descricao)`. Ambos via `comando()` do Núcleo, que valida faixa e vigência (≥ mês aberto mais antigo, consultando `atv.mesAbertoMaisAntigo()`), grava a versão, atualiza `par_vigentes` e audita.

O cálculo do mês aberto mais antigo vem do contrato de ATV. Para PAR não depender de ATV (regra do MAPA), o contrato é registrado pelo Núcleo como "provedor de fechamento", e PAR consulta o Núcleo.

## Testes
- Unidade: `valor()` com várias vigências; validação de faixa.
- Rules: só SEC e master gravam (via callable); leitura de `par_vigentes` para qualquer usuário autenticado.
- E2E: alterar um prazo e ver o painel do SG atualizar sem recarregar.
