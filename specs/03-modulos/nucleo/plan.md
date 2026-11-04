# Núcleo (NUC) — Plano técnico

## Pacote
`packages/nucleo` (TypeScript): `shared/` (tipos, `pode`, catálogo de papéis), `functions/` (callables, gatilhos, job), `web/` (componentes React: login, seletor de papel, QR, auditoria, aparência, `FolhaPdf`).

## Coleções (prefixo `nuc_`)
```
nuc_usuarios/{uid}         { pubId?, tipo: pessoa|master, emails: [..], preferencias: {paleta, fonteGrande, contraste, escuro},
                             mfa: bool, criadoEm }
nuc_sessoes/{uid}          { versao, motivoUltimaMudanca, em }           (lido pelo app em tempo real e pelas rules)
nuc_aparelhos/{deviceId}   { uid, ativadoEm, revogadoEm?, userAgent }
nuc_ativacoes/{hashToken}  { pubId, geradoPor, expiraEm, usadoEm? }      (só Functions; o token em claro nunca é guardado)
nuc_atribuicoes/{id}       { uid, papel, escopo?, desde, ate?, origem: derivada|manual, motivo?, concedidoPor? }
nuc_papeis/{papel}         catálogo gerado dos manifestos
nuc_termos/{tipo_versao}   { tipo: consentimento|confidencialidade, versao, texto, vigenteDesde }
nuc_aceites/{id}           { uid, termo, versao, aceitoEm, revogadoEm? }
nuc_auditoria/{id}         { quem, papelUsado, master: bool, quando, modulo, acao, alvo, antes, depois, motivo }
```

## Autenticação (Firebase Auth + Identity Platform)
| Quem | Método | Detalhe |
|---|---|---|
| Publicador | QR → `ativarAparelho(token)` → **custom token** | Persistência `indexedDBLocal`; o deviceId é gerado no aparelho |
| Equipe | Google ou link por e-mail | O `beforeSignIn` (blocking function) recusa e-mails que não estão no Cadastro |
| SEC | + TOTP | Identity Platform MFA |
| Master | Conta separada + TOTP | `beforeSignIn` exige MFA e o aceite vigente do termo; sessão curta controlada pelo app e pelo `auth_time` |

## Claims e sessão (ADR-003 §4 e §8)
- A função `recalcularAcesso(uid)` lê os perfis vigentes (`cad.perfisVigentes`) e as concessões, aplica as derivações dos manifestos e grava `nuc_atribuicoes` derivadas e os claims `{p, g, v, r, mfa}`.
- Se algum papel saiu: `nuc_sessoes.versao++` + `revokeRefreshTokens`. Se só entrou: atualiza os claims; o app faz `getIdToken(true)`.
- Gatilhos: mudança em `cad_designacoes`/`cad_publicadores.grupoId`, em `nuc_atribuicoes` manuais e em `nuc_aparelhos.revogadoEm`; e o job diário (vigências).
- **Rules:** a função gerada `sessaoValida()` = `request.auth.token.v == get(/nuc_sessoes/$(uid)).data.versao`. Todo `allow` usa `sessaoValida() && temPapel(...)`.
- **App:** `onSnapshot(nuc_sessoes/uid)`; se `versao` ≠ `token.v` e a mudança foi de perda → `signOut()` + mensagem.

## Comandos (Cloud Functions callable)
`gerarQrAtivacao`, `ativarAparelho`, `revogarAparelho`, `concederPapel`, `revogarPapel`, `aceitarTermo`, `revogarConsentimento`, `salvarPreferencias`, `alternarExpurgo`.
Todos usam o envelope `comando()`:
1. Valida o token com `checkRevoked`.
2. Confere `sessaoValida`.
3. Executa `pode()`.
4. Roda o handler numa transação.
5. Grava a auditoria na mesma transação.

## Geração a partir dos manifestos (build)
`scripts/gerar-acesso.ts` lê `packages/*/manifesto.ts` e gera: `firestore.rules` (blocos `temPapel`), `nuc_papeis` (seed) e os tipos `Permissao`. O CI falha se os arquivos gerados estiverem desatualizados.

## Job diário (00:05 BRT)
`agendar()` registra tarefas por módulo. O Núcleo executa, na ordem: vigências de atribuições → recálculo de acesso → tarefas dos módulos → expurgo (se habilitado). Cada tarefa é idempotente e registrada.

## Front-end
- `<Sessao>`: provedor de contexto (usuário, papéis, papel ativo, preferências) e listener de sessão.
- `<SeletorPapel>`, `<TelaQr>`, `<Ativacao>`, `<TermoAceite>`, `<Concessoes>`, `<Auditoria>`, `<Aparencia>`, `<FaixaMaster>`.
- Tema: tokens CSS por paleta (`data-palette`), seguindo o protótipo.

## Testes
- **Unidade (Vitest):** `pode`, derivações, decisão "perdeu ou ganhou poder", validade do QR.
- **Rules (Emulator):** cada papel com e sem permissão, escopo errado, versão de sessão antiga, master sem aceite.
- **E2E (Playwright):** ativação por QR no celular emulado; revogação com o app aberto (logout em ≤ 5 s); troca de papel sem logout.
- **Acessibilidade:** axe-core nas telas do Núcleo.
