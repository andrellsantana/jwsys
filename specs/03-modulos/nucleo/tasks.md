# Núcleo (NUC) — Tarefas

Cada tarefa é pequena (≤ 1 dia), testável e ligada a regras e histórias. ☐ a fazer.

## Fundação do repositório
- ☐ **T-NUC-01** Monorepo (pnpm workspaces) com `packages/{nucleo,parametros,cadastro,atividades,reunioes}`, TypeScript estrito, ESLint, Prettier. *(ADR-001)*
- ☐ **T-NUC-02** Projeto Firebase em `southamerica-east1`, Emulator Suite local e ambientes `dev` e `prod`. *(ADR-001)*
- ☐ **T-NUC-03** CI no GitHub Actions: lint, testes, Emulator, axe e deploy com Workload Identity Federation. *(ADR-001)*
- ☐ **T-NUC-04** Proteções FinOps: alertas de orçamento, função `stopBilling` em R$ 30, `maxInstances`, limpeza do Artifact Registry e App Check. *(ADR-001)*

## Acesso
- ☐ **T-NUC-05** `defineModulo` + tipos `Permissao`/`Papel` + `pode()` com testes. *(NUC-008, ADR-003)*
- ☐ **T-NUC-06** `scripts/gerar-acesso.ts`: gera as rules e o catálogo a partir dos manifestos, com verificação no CI. *(ADR-003)*
- ☐ **T-NUC-07** Envelope `comando()` com checagem de revogação, sessão, permissão e auditoria transacional. *(NUC-006, NUC-011)*
- ☐ **T-NUC-08** `recalcularAcesso(uid)` com derivações e decisão de perda ou ganho. *(NUC-008, NUC-011)*
- ☐ **T-NUC-09** `nuc_sessoes`, `sessaoValida()` nas rules e listener no app com logout. *(H-NUC-06)*
- ☐ **T-NUC-10** Gatilhos de recálculo (designações, grupo, concessões, aparelhos). *(NUC-008)*

## Entrada
- ☐ **T-NUC-11** `gerarQrAtivacao` (hash do token, validade, invalida o anterior) e `<TelaQr>`. *(H-NUC-02)*
- ☐ **T-NUC-12** `ativarAparelho` → custom token, revogação do aparelho anterior, `<Ativacao>`. *(H-NUC-01)*
- ☐ **T-NUC-13** Login da equipe (Google e link por e-mail) + `beforeSignIn` com o cadastro. *(H-NUC-03)*
- ☐ **T-NUC-14** MFA TOTP do SEC e papéis `exigeMFA`. *(NUC-012)*
- ☐ **T-NUC-15** Conta master: `beforeSignIn`, termo de confidencialidade, sessão de 30 min, `<FaixaMaster>`. *(H-NUC-07)*

## Interface comum
- ☐ **T-NUC-16** `<Sessao>` e `<SeletorPapel>` sem logout na troca. *(H-NUC-04)*
- ☐ **T-NUC-17** `<Concessoes>` + `concederPapel`/`revogarPapel` com validade. *(H-NUC-05)*
- ☐ **T-NUC-18** Termos versionados, `<TermoAceite>`, Ajuda › Privacidade e revogação. *(H-NUC-08)*
- ☐ **T-NUC-19** `<Auditoria>` com filtros e destaques; rules que proíbem update e delete. *(H-NUC-09)*
- ☐ **T-NUC-20** `<Aparencia>` + `salvarPreferencias`, tema por paleta. *(H-NUC-11)*

## Jobs e PDF
- ☐ **T-NUC-21** Job diário com `agendar()`, vigências e execução idempotente. *(ADR-002)*
- ☐ **T-NUC-22** Expurgo: parâmetro desligado, prévia e execução registrada. *(H-NUC-10)*
- ☐ **T-NUC-23** Motor de PDF: `preencherPdf` sem achatar + `<FolhaPdf>` (SVG do PDF + campos sobrepostos pelos `/Rect`), com teste de comparação visual. *(ATV-013, ATV-041)*

## Definição de pronto (vale para cada tarefa)
- Os critérios de aceite das histórias ligadas viram testes, e os testes passam.
- Rules testadas no Emulator para o caso permitido e o negado.
- Sem violações do axe nas telas tocadas.
- Auditoria gravada para todo comando novo.
