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
- Leitura de `cad_publicadores`: SEC e master (todos os campos); SG do grupo (campos de Q-28, servidos por uma **visão** `cad_visaoGrupo/{grupoId}_{pubId}` mantida por gatilho, porque as rules não filtram campos); o próprio publicador (Q-30).
- Escrita: só via callables (`cadastrarPublicador`, `editarPublicador`, `registrarDesignacao`, `encerrarDesignacao`, `moverGrupo`, `registrarSituacao`, `gerirGrupo`).

## Gatilhos
- `cad_designacoes` alterada → `nuc.recalcularAcesso(uid)`.
- `grupos/{mes}` alterado → atualiza `grupoAtual`, a visão do SG e o recálculo de acesso.
- `situacao` alterada → evento para ATV (contagens e status).

## Validações (shared, usadas no app e nas Functions)
`validarPerfil(publicador, perfil, escopo, designacoesVigentes)` → erros do CAD-008. `possivelDuplicado(nomeBusca, nascimento)`.

## Testes
- Unidade: validações de perfil (sexo, batismo, ancião, titular único, SG único por grupo).
- Rules: SG vê só a visão do próprio grupo; publicador vê só o próprio cadastro.
- E2E: mover em lote 3 publicadores e conferir a visibilidade dos dois SGs.
