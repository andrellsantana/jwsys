# ADR-002 — Modelo de dados (Firestore) e segurança

**Status:** ACEITO (04/11/2026).

## Princípios
0. **Prefixo do módulo** em toda coleção (`nuc_`, `par_`, `cad_`, `reu_`, `atv_`). Cada módulo só grava nas próprias coleções e lê as de outros via contrato (MAPA.md).
1. **O cliente quase não escreve.** As únicas gravações diretas são a criação de envios do S-4, para que a fila offline do SDK funcione (ATV-009). Todo o resto (aprovar, conciliar, fechar, cadastrar) passa por **Cloud Functions callable**, que validam as regras e gravam a auditoria.
2. **Append-only:** envios, decisões, atv_fechamentos e nuc_auditoria nunca são alterados nem apagados pelo cliente.
3. **Papéis acumuláveis em custom claims** (NUC-003): `{ pubId, grupoPub, sgDe: grupoId | null, secretario: bool, cs: bool }`. O grupo em que a pessoa **publica** (`grupoPub`) é independente do grupo que ela **supervisiona** (`sgDe`). Exemplo: o secretário que é SG do Grupo 2 → `{ secretario: true, sgDe: "g2" }`. A nuc_auditoria grava `papelUsado`.
4. **Chaves de mês** no formato `AAAAMM` (ex.: `202610`). Ano de serviço = `AS2027`.
5. **Dinheiro em leituras:** os painéis leem só `atv_totais/*` (ADR-001).

## Coleções

```
par_parametros/{vigenciaAAAAMM}          PAR-001: prazos (6,7,15,16,20), metas (15,30,50,55,560,600), flags, retenção
par_agenda/{AAAAMM}                      { mesEspecial: bool }                                       PAR-002

cad_grupos/{grupoId}                     { nome, sgPubId, ajudantes: [pubId], ativo }
cad_publicadores/{pubId}                 dados S-21 + cadastro (endereço, contatos, emergência),
                                     { grupoId, sexo, esperanca, privilegio, pioneiro: {tipo, desde, enfermo},
                                       situacao: {tipo: ativo|mudou|removido|falecido, data},
                                       status: {valor: regular|irregular|inativo, calculadoEm} }
  /historicoGrupo/{id}               { grupoId, vigenteDesde }                                    CAD-001
  /consentimentos/{id}               { versaoTermo, aceitoEm, revogadoEm? }                       NUC-004
  /statusHistorico/{AAAAMM}        { status, situacao }  legível pelo SG atual do publicador (CAD-002)
  /saude/{id}                        { descricao, desde, ate?, registradoPor }                    CAD-003

nuc_aparelhos/{uid}                      { pubId, ativadoEm, revogadoEm? }                            NUC-001
nuc_ativacoes/{hashToken}                { pubId, expiraEm, usadoEm? }    (só via Functions)

atv_relatorios/{pubId}_{AAAAMM}          documento "vigente" do mês, mantido pelas Functions:
                                     { pubId, grupoId, mes, estado: aceito|aguardando|recusado|nao_relatou,
                                       categoria: publicador|auxiliar|regular|especial|missionario,
                                       participou, estudos, horas, horasAuxNaoComputadas,
                                       horasAtividadeAprovada, creditoConsiderado, observacoes,
                                       envioVigenteId, origem: app|importado_s21|lancamento,
                                       contaNoRelatorioDe: AAAAMM, pendenteProcessamento: bool }
  /envios/{envioId}                  APPEND-ONLY, criado pelo publicador:
                                     { dados S-4, uid, emNomeDe?, justificativa?,
                                       criadoNoAparelhoEm, recebidoEm (serverTimestamp),
                                       tipo: prazo|atraso|retroativo|reenvio   (definido por trigger),
                                       decisao?: { por, em, resultado, motivo } (só Functions) }

atv_enviosNaoIdentificados/{id}          envio pelo link do grupo sem ativação (NUC-002): { grupoId, nomeDigitado, dados, ... }

atv_fechamentos/{AAAAMM}                 { congregacao: {estado, fechadoEm, reaberturas: [{em, motivo}] , imutavelApos},
                                       grupos: { [grupoId]: {estado, fechadoEm, automatico, liberacaoAte?} } }

atv_totais/{AAAAMM}_{grupoId|cong}       contagens pré-calculadas (ATV-042, painéis)

atv_peticoesAux/{id}                     S-205b: { pubId, meses: [AAAAMM] | continuoDesde, data, nome,
                                       aceiteEletronico | papel: {anexoPath},
                                       criterios: {exemplo, semRestricao, sgConsultado},
                                       aprovacoes: [{membroPubId, em}], estado: pendente|aprovada|recusada|cancelada,
                                       cancelamento?: {em, automatico, reativadaEm?} }          ATV-022/ATV-025/ATV-026/ATV-027
atv_designacoes/{AAAAMM}                 { [pubId]: categoria }  materializado pelas Functions → libera Horas no S-4

reu_assistencia/{AAAA-MM-DD}_{tipo}      { tipo: meio|fim, total }                                   REU-001
atv_apuracaoPioneiros/{AS}_{pubId}       { meta, limite, campo, credito, mesesComoPioneiro, faixa }  ATV-032/ATV-035
nuc_auditoria/{id}                       { quem, papelUsado, quando, acao, alvo, antes, depois, motivo }         NUC-006
```

## Gatilhos e jobs
| Evento | Ação |
|---|---|
| `envios` onCreate | Classifica o `tipo` pelo `recebidoEm`, com tolerância offline (ATV-010); prazo + aparelho ativado → aceito; senão → aguardando; atualiza o `atv_relatorios` vigente e os `atv_totais`. |
| Callable `decidirEnvio` (SG) | Aceita ou recusa com motivo. Se o grupo já fechou → `pendenteProcessamento` (ATV-017). |
| Callable `conciliar` (SG) | Liga um `atv_enviosNaoIdentificados` a um `pubId` (e opcionalmente o aparelho). |
| Job diário (00:05 BRT) | Dia 7: bloqueio. Dia 16: fecha os grupos. Fim das liberações temporárias. Fim de mês: torna imutável o relatório da congregação de M−1. Recálculo de status (ATV-036…ATV-040, CAD-004). Cancelamento de auxiliar contínuo (ATV-025). Processa a fila de tardios. Expurgo, **só se `expurgo_habilitado` = true** (padrão false, NUC-005). |
| `atv_peticoesAux` alterada | Recalcula `atv_designacoes/*`. |

## Security Rules (resumo)
- **Publicador:** lê `cad_publicadores/{seuPubId}`, `atv_relatorios/{seuPubId}_*` com seus `envios`, `atv_designacoes/{mes}` (só a própria chave) e `par_parametros`. **Cria** `envios` sob o próprio relatório, com campos em whitelist, `uid == request.auth.uid` e `recebidoEm == request.time`. Não pode atualizar nem apagar nada.
- **Anônimo pelo link do grupo:** só cria `atv_enviosNaoIdentificados` com `grupoId` válido. Não lê nada.
- **SG:** lê cad_publicadores e relatórios onde `grupoId == token.sgDe` (e o próprio relatório como publicador). Dos cad_publicadores atuais do grupo, lê também `statusHistorico/*` completo, sem acesso aos relatórios de outros grupos. Decisões sobre os próprios envios são marcadas `autoaprovacao` (ATV-018). Não escreve nada direto: tudo passa por callables.
- **Secretário:** lê tudo. Escreve só via callables. MFA obrigatório (`token.firebase.sign_in_second_factor`).
- **CS:** lê `atv_peticoesAux` e `atv_apuracaoPioneiros`. Aprova via callable.
- `nuc_auditoria`, `atv_fechamentos` e `atv_totais`: escrita só por Functions (Admin SDK).
- Todo acesso exige **App Check**.

## Riscos e mitigações
- **Visibilidade por grupo, não por pessoa (CAD-002):** `relatorios.grupoId` é gravado com o grupo **do mês**. A regra é `resource.data.grupoId == token.sgDe`: o SG atual vê **todo o histórico do grupo** que supervisiona hoje, inclusive de quem já saiu dele, e perde o acesso ao grupo anterior no mesmo instante em que é trocado (os claims são revogados e reemitidos).
- **Relógio do aparelho:** a classificação usa o `recebidoEm` do servidor. O `criadoNoAparelhoEm` só ativa a tolerância de 24h com a decisão do SG (ATV-010).
- **Custos:** índices compostos só onde houver consulta real. Nenhuma consulta de coleção inteira nos painéis.
