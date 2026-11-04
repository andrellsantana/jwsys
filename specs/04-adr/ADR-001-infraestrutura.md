# ADR-001 — Infraestrutura e stack

**Status:** proposto, aguardando aprovação.

## Contexto
- 1 congregação, cerca de 100 publicadores.
- Dados sensíveis, que precisam ficar armazenados no Brasil.
- 1 mantenedor.
- Orçamento: meta de R$ 0, teto de R$ 25/mês.

## Volume estimado
- Cerca de 100 relatórios por mês, ou 1.200 por ano.
- Menos de 50 MB de banco em 10 anos.
- Tráfego desprezível.

O problema não é capacidade: é **custo fixo mínimo** e **região no Brasil**.

## Opções avaliadas (FinOps)

| Opção | Região BR | Custo | Riscos |
|---|---|---|---|
| **A. Supabase Free** (Postgres + Auth + RLS + pg_cron + Edge Functions) | sa-east-1 (São Paulo) | R$ 0 | O projeto **pausa** após 7 dias sem uso (mitigado com um ping diário via GitHub Actions). Não tem backup gerenciado no plano free (mitigado com `pg_dump` criptografado diário). Limite de 500 MB. O plano Pro custa US$ 25 (cerca de R$ 140) e estoura o teto. |
| **B. Oracle Cloud Always Free** (VM ARM, Postgres próprio) | sa-saopaulo-1 | R$ 0 | Atualizações, segurança e backup ficam todos com você. Há risco de recuperação de VMs ociosas. |
| **C. GCP** (Cloud Run + Cloud SQL) | southamerica-east1 | Cloud SQL ≥ R$ 50/mês | Estoura o teto. |
| **D. GCP Firestore + Cloud Run** | southamerica-east1 | Cerca de R$ 0 | NoSQL piora relatórios e consultas analíticas, e não tem RLS relacional. |

## Decisão proposta
**Opção A**, com o código escrito de forma portável (Postgres puro + migrations SQL), para permitir migrar para a opção B sem reescrever o sistema.

- **Frontend:** PWA em React + Vite + TypeScript, servida pelo Cloudflare Pages (grátis). O host só serve o código estático: nenhum dado pessoal fica lá.
- **Backend:** Postgres (regras em funções SQL e RLS) + Supabase Edge Functions (TypeScript/Deno) para geração do PDF (`pdf-lib`) e notificações.
- **Jobs:** `pg_cron` para fechamento automático, recálculo de status, expurgo e fim de liberações temporárias.
- **Autenticação:**
  - **Publicador:** login anônimo por aparelho + link do grupo. O vínculo ao cadastro é feito na conciliação pelo SG (RN-010).
  - **SG, secretário e CS:** link mágico por e-mail ou login Google. MFA (TOTP) obrigatório para o secretário.
- **Backups:** `pg_dump` diário, criptografado com `age`, guardado em armazenamento no Brasil (OCI Object Storage São Paulo, free 20 GB). O local final ainda está em aberto.
- **Testes:** Vitest (unidade), pgTAP (RLS e regras SQL), Playwright (E2E).
- **CI/CD:** GitHub Actions.

## Notificações — ADIADO (fora do MVP, segunda etapa)
Decisão: o MVP não terá notificações. A proposta abaixo fica registrada para ser retomada na segunda etapa.

| Canal | Custo | Uso |
|---|---|---|
| **Web Push (PWA)** | Grátis | Lembrete do prazo do dia 6 e mudança de status do relatório. No iPhone, exige o app instalado na tela inicial (iOS 16.4+). |
| **E-mail** (Resend/Brevo, free) | Grátis | SG e secretário: fechamentos, pendências, relatórios tardios. |
| **WhatsApp "clique para enviar"** (`wa.me`) | Grátis | O SG dispara manualmente o lembrete para quem não enviou. A API oficial do WhatsApp é paga e foi descartada. |

## Consequências
- Custo de R$ 0/mês, com R$ 25 de folga para um domínio próprio (cerca de R$ 40/ano para `.com.br`).
- Dependência do Supabase, mitigada pela portabilidade.
