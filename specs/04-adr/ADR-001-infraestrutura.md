# ADR-001 — Infraestrutura e stack

**Status:** ACEITO (04/11/2026). Decisão: **GCP + Firebase**, região `southamerica-east1` (São Paulo).

## Contexto
- 1 congregação, cerca de 100 publicadores.
- Dados sensíveis, que precisam ficar armazenados no Brasil.
- 1 mantenedor.
- Orçamento: meta de R$ 0, teto de R$ 25/mês.
- Volume: cerca de 1.200 relatórios por ano, menos de 50 MB em 10 anos.

## Alternativas avaliadas
| Opção | Custo | Motivo da não escolha |
|---|---|---|
| Supabase Free (sa-east-1) | R$ 0 | Pausa por inatividade e não tem backup no plano free. Fica como plano B. |
| AWS (RDS ou DynamoDB) | > R$ 100 (RDS) ou R$ 0 (DynamoDB) | RDS estoura o teto. DynamoDB exige mais código para a segurança. |
| GCP Cloud SQL | R$ 60–80 | Estoura o teto. |
| **GCP + Firebase** | **R$ 0–1** | **Escolhida.** |

## Decisão
| Camada | Tecnologia |
|---|---|
| PWA | React + Vite + TypeScript, servida pelo **Firebase Hosting** |
| Offline / fila de envio (RN-009) | Persistência offline do SDK do Firestore (IndexedDB) |
| Banco | **Cloud Firestore** em modo nativo, `southamerica-east1` |
| Segurança | **Firestore Security Rules**: isolamento por grupo, imutabilidade de envios e travas de fechamento. Testadas com o Emulator Suite. |
| Backend | **Cloud Functions for Firebase (2ª geração, roda em Cloud Run)** em TypeScript: ativação por QR, aprovações, fechamentos, totais pré-calculados, PDFs do S-21 e do S-205b preenchidos sem achatar (`pdf-lib`), importação de S-21 |
| Jobs | Uma única função agendada diária (Cloud Scheduler, 1 de 3 tarefas grátis) que despacha: fechamento do dia 16, fim de liberações temporárias, recálculo de status e expurgo |
| Auth do publicador | O QR de uso único (RN-002) chama uma função que emite um *custom token*. O aparelho fica logado de forma persistente, sem senha. |
| Auth da equipe (SG, secretário, CS) | Login Google ou link por e-mail. **MFA TOTP** para o secretário (Identity Platform, dentro da cota grátis). |
| Backup | Exportação agendada do Firestore para um bucket no Cloud Storage em `southamerica-east1`, com regra de ciclo de vida (custo de centavos) |
| Testes | Vitest (unidade), Firebase Emulator Suite (regras e funções), Playwright (E2E) |
| CI/CD | GitHub Actions + Workload Identity Federation (sem chave de serviço guardada) |

## Consequências para a modelagem (NoSQL)
- **Totais pré-calculados:** documentos `totais/{mes}/{grupo}` e `totais/{mes}/congregacao`, atualizados por gatilhos. Os painéis leem só esses documentos, para economizar leituras.
- **Envios como coleção append-only:** `relatorios/{pubId_mes}/envios/{id}`. A versão vigente é apontada pelo documento pai.
- A apuração dos pioneiros e o cálculo de status rodam nas funções, nunca no cliente.
- A modelagem detalhada vai no ADR-002.

## FinOps — proteções obrigatórias
1. **Alertas de orçamento** em R$ 1, R$ 5, R$ 10 e R$ 20.
2. **Desligamento automático da cobrança** em R$ 30 (decisão do usuário), com a função oficial `stopBilling` assinando o Pub/Sub do orçamento.
3. Funções e Cloud Run com `maxInstances` = 2 e `minInstances` = 0.
4. Política de limpeza no Artifact Registry: manter as 2 últimas imagens.
5. App Check ativado para Firestore e Functions.
6. Painéis só leem os totais pré-calculados.
7. Domínio próprio só na entrada em produção. Até lá, usar `*.web.app`.

## Notificações — ADIADO (fora do MVP, segunda etapa)
A proposta fica registrada para ser retomada:
- **FCM Web Push:** grátis, para o publicador.
- **E-mail** pela extensão Trigger Email, para a equipe.
- **Botão `wa.me`** para o SG cobrar quem não enviou.
