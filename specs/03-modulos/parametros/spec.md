# Parâmetros do Sistema (PAR) — Especificação

**Status:** em revisão (04/11/2026). **Regras:** PAR-001…PAR-007.

## Objetivo
Guardar, num só lugar, todos os valores que as regras usam (prazos, metas, limites, opções), com histórico por vigência, e a **agenda** dos meses especiais.

## Catálogo inicial (valores padrão)
| Módulo | Chave | Padrão | Uso |
|---|---|---|---|
| ATV | `prazo_envio_dia` | 6 | Último dia do envio no prazo (ATV-002) |
| ATV | `bloqueio_dia` | 7 | Bloqueio do envio normal (ATV-003) |
| ATV | `fechamento_grupo_dia` | 16 | Fechamento automático do grupo, às 00:00 (ATV-015) |
| ATV | `fechamento_cong_dia` | 20 | Prazo do fechamento da congregação (ATV-019) |
| ATV | `tolerancia_offline_horas` | 24 | Envio offline no prazo (ATV-010) |
| ATV | `meta_aux_mes_especial` | 15 h | Pioneiro auxiliar em mês especial (ATV-023) |
| ATV | `meta_aux_mes_normal` | 30 h | Pioneiro auxiliar nos demais meses (ATV-023) |
| ATV | `meta_regular_anual` | 600 h | Pioneiro regular (ATV-028) |
| ATV | `referencia_regular_mensal` | 50 h | Referência mensal (ATV-028) |
| ATV | `limite_analise_regular` | 560 h | Faixas da apuração (ATV-032) |
| ATV | `limite_campo_mais_credito` | 55 h | Teto de campo + crédito no mês (ATV-030) |
| ATV | `meses_janela_status` | 6 | Regular/irregular/inativo (ATV-036…ATV-040) |
| ATV | `consolidar_especiais` | não | Especiais e missionários no relatório (ATV-034) |
| ATV | `anexo_max_mb` | 5 MB | Digitalização da petição (ATV-027) |
| ATV | `assinatura_confianca_min` | 0,80 | Verificação automática do S-205b (ATV-027a) |
| NUC | `qr_validade_horas` | 72 h | QR de ativação (NUC-001) |
| NUC | `master_sessao_min` | 30 min | Sessão do master sem uso (NUC-010) |
| NUC | `expurgo_habilitado` | não | Expurgo (NUC-005) |
| NUC | `retencao_<tipo>` | sem limite | Retenção por tipo de dado (NUC-005) |

Valores de QR (72 h) e de confiança da assinatura (0,80) confirmados pelo usuário em 04/11/2026.

---

## Histórias e critérios de aceite

### H-PAR-01 Consultar os parâmetros
> Como **SEC ou master**, quero ver todos os valores vigentes e de onde vêm.
- **CA-1** A lista é agrupada por módulo e mostra a descrição, o valor vigente, a unidade, o desde quando vale e quem alterou.
- **CA-2** Cada parâmetro mostra o histórico de versões.
- **CA-3** Outros usuários não veem a tela (os valores são lidos pelo sistema, não exibidos).

### H-PAR-02 Alterar um parâmetro
> Como **SEC ou master**, quero mudar um valor a partir de um mês.
- **CA-1** A alteração pede o novo valor, o mês de início da vigência e o motivo (todos obrigatórios).
- **CA-2** O valor fora do mínimo/máximo do catálogo é recusado com a faixa permitida.
- **CA-3** Uma vigência anterior ao mês aberto mais antigo é recusada: "Meses já fechados não podem ser alterados."
- **CA-4** Antes de salvar, a tela mostra antes → depois e a vigência para confirmar.
- **CA-5** Depois de salvar, as telas abertas e as funções passam a usar o novo valor sem reinício.
- **CA-6** A alteração fica na auditoria.

### H-PAR-03 Agenda de meses especiais
> Como **SEC**, quero marcar os meses especiais conforme forem anunciados.
- **CA-1** Calendário do ano de serviço (set–ago), com cada mês marcável como "especial" e um campo de descrição.
- **CA-2** Um mês fechado na congregação não pode ser alterado (PAR-006).
- **CA-3** Marcar ou desmarcar um mês recalcula as metas dos pioneiros auxiliares daquele mês ainda aberto.
- **CA-4** A alteração fica na auditoria.

### H-PAR-04 Termos (LGPD e confidencialidade)
> Como **SEC ou master**, quero publicar uma nova versão de um termo.
- **CA-1** Editor do texto com pré-visualização e número de versão automático.
- **CA-2** Publicar uma versão nova exige novo aceite no próximo acesso de cada usuário (NUC-004, NUC-010).
- **CA-3** As versões antigas continuam disponíveis para consulta.
- **CA-4** Só o master publica o termo de confidencialidade.

### H-PAR-05 Leitura pelos módulos
> Como **módulo**, quero ler o valor vigente para uma data.
- **CA-1** `par.valor(chave, mes)` devolve o valor vigente naquele mês.
- **CA-2** `par.mesEspecial(mes)` devolve sim ou não.
- **CA-3** Uma chave inexistente gera erro no build (tipos gerados do catálogo).

## Contratos publicados
`par.valor(chave, mes?)`, `par.mesEspecial(mes)`, `par.onAlteracao(callback)` e os tipos `ChaveParametro`.
