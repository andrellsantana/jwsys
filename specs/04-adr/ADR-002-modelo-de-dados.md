# ADR-002 — Modelo de dados (Firestore) e segurança

**Status:** proposto.

## Princípios
1. **O cliente quase não escreve.** As únicas gravações diretas são a criação de envios do S-4, para que a fila offline do SDK funcione (RN-009). Todo o resto (aprovar, conciliar, fechar, cadastrar) passa por **Cloud Functions callable**, que validam as regras e gravam a auditoria.
2. **Append-only:** envios, decisões, fechamentos e auditoria nunca são alterados nem apagados pelo cliente.
3. **Papéis em custom claims** do token: `{ papel: "publicador" | "sg" | "secretario" | "cs", pubId, grupoId }`. Uma pessoa pode ter vários papéis (`papeis: [...]`).
4. **Chaves de mês** no formato `AAAAMM` (ex.: `202610`). Ano de serviço = `AS2027`.
5. **Dinheiro em leituras:** os painéis leem só `totais/*` (ADR-001).

## Coleções

```
parametros/{vigenciaAAAAMM}          RN-090: prazos (6,7,15,16,20), metas (15,30,50,55,560,600), flags, retenção
agenda/{AAAAMM}                      { mesEspecial: bool }                                       RN-044

grupos/{grupoId}                     { nome, sgPubId, ajudantes: [pubId], ativo }
publicadores/{pubId}                 dados S-21 + cadastro (endereço, contatos, emergência),
                                     { grupoId, sexo, esperanca, privilegio, pioneiro: {tipo, desde, enfermo},
                                       situacao: {tipo: ativo|mudou|removido|falecido, data},
                                       status: {valor: regular|irregular|inativo, calculadoEm} }
  /historicoGrupo/{id}               { grupoId, vigenteDesde }                                    RN-033
  /consentimentos/{id}               { versaoTermo, aceitoEm, revogadoEm? }                       RN-080
  /saude/{id}                        { descricao, desde, ate?, registradoPor }                    RN-020

aparelhos/{uid}                      { pubId, ativadoEm, revogadoEm? }                            RN-002
ativacoes/{hashToken}                { pubId, expiraEm, usadoEm? }    (só via Functions)

relatorios/{pubId}_{AAAAMM}          documento "vigente" do mês, mantido pelas Functions:
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

enviosNaoIdentificados/{id}          envio pelo link do grupo sem ativação (RN-002a): { grupoId, nomeDigitado, dados, ... }

fechamentos/{AAAAMM}                 { congregacao: {estado, fechadoEm, reaberturas: [{em, motivo}] , imutavelApos},
                                       grupos: { [grupoId]: {estado, fechadoEm, automatico, liberacaoAte?} } }

totais/{AAAAMM}_{grupoId|cong}       contagens pré-calculadas (RN-071, painéis)

peticoesAux/{id}                     S-205b: { pubId, meses: [AAAAMM] | continuoDesde, data, nome,
                                       aceiteEletronico | papel: {anexoPath},
                                       criterios: {exemplo, semRestricao, sgConsultado},
                                       aprovacoes: [{membroPubId, em}], estado: pendente|aprovada|recusada|cancelada,
                                       cancelamento?: {em, automatico, reativadaEm?} }          RN-040/043/045/046
designacoes/{AAAAMM}                 { [pubId]: categoria }  materializado pelas Functions → libera Horas no S-4

assistencia/{AAAA-MM-DD}_{tipo}      { tipo: meio|fim, total }                                   RN-034
apuracaoPioneiros/{AS}_{pubId}       { meta, limite, campo, credito, mesesComoPioneiro, faixa }  RN-054/057
auditoria/{id}                       { quem, quando, acao, alvo, antes, depois, motivo }         RN-082
```

## Gatilhos e jobs
| Evento | Ação |
|---|---|
| `envios` onCreate | Classifica o `tipo` pelo `recebidoEm` (ver Q-15); prazo + aparelho ativado → aceito; senão → aguardando; atualiza o `relatorios` vigente e os `totais`. |
| Callable `decidirEnvio` (SG) | Aceita ou recusa com motivo. Se o grupo já fechou → `pendenteProcessamento` (RN-024). |
| Callable `conciliar` (SG) | Liga um `enviosNaoIdentificados` a um `pubId` (e opcionalmente o aparelho). |
| Job diário (00:05 BRT) | Dia 7: bloqueio. Dia 16: fecha os grupos. Fim das liberações temporárias. Fim de mês: torna imutável o relatório da congregação de M−1. Recálculo de status (RN-060–065). Cancelamento de auxiliar contínuo (RN-043). Processa a fila de tardios. Expurgo (RN-081). |
| `peticoesAux` alterada | Recalcula `designacoes/*`. |

## Security Rules (resumo)
- **Publicador:** lê `publicadores/{seuPubId}`, `relatorios/{seuPubId}_*` com seus `envios`, `designacoes/{mes}` (só a própria chave) e `parametros`. **Cria** `envios` sob o próprio relatório, com campos em whitelist, `uid == request.auth.uid` e `recebidoEm == request.time`. Não pode atualizar nem apagar nada.
- **Anônimo pelo link do grupo:** só cria `enviosNaoIdentificados` com `grupoId` válido. Não lê nada.
- **SG:** lê publicadores e relatórios onde `grupoId == token.grupoId` (e o próprio relatório como publicador). Não escreve nada direto: tudo passa por callables.
- **Secretário:** lê tudo. Escreve só via callables. MFA obrigatório (`token.firebase.sign_in_second_factor`).
- **CS:** lê `peticoesAux` e `apuracaoPioneiros`. Aprova via callable.
- `auditoria`, `fechamentos` e `totais`: escrita só por Functions (Admin SDK).
- Todo acesso exige **App Check**.

## Riscos e mitigações
- **Mudança de grupo no meio do ano:** `relatorios.grupoId` é gravado com o grupo **do mês**, para que o histórico do SG anterior não vaze nem suma.
- **Relógio do aparelho:** a classificação usa o `recebidoEm` do servidor. O `criadoNoAparelhoEm` fica só como informação (Q-15).
- **Custos:** índices compostos só onde houver consulta real. Nenhuma consulta de coleção inteira nos painéis.
