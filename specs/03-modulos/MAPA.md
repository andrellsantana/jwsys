# Mapa de módulos do JWSys

O JWSys é uma **plataforma modular** para a congregação. Cada módulo tem dados, regras, telas e testes próprios, e conversa com os outros só por **contratos** (funções e eventos publicados), nunca lendo diretamente as coleções de outro módulo.

```
                ┌──────────────────────── NÚCLEO (plataforma) ────────────────────────┐
                │ Identidade & Acesso · Papéis · Auditoria · LGPD · Jobs · PDF · (Notif.)│
                └───────────────▲───────────────▲───────────────▲───────────────▲──────┘
                                │               │               │               │
                        ┌───────┴──────┐ ┌──────┴───────┐ ┌─────┴──────┐ ┌──────┴──────┐
                        │  PARÂMETROS  │ │   CADASTRO   │ │  REUNIÕES  │ │ ATIVIDADES  │
                        │   (PAR)      │◄┤   (CAD)      │◄┤   (REU)    │ │ PUBLICADORES│
                        └──────────────┘ └──────────────┘ └─────┬──────┘ │   (ATV)     │
                               ▲  ▲               ▲             │média  └──────┬──────┘
                               │  └───────────────┼─────────────┴──────────────┘
                               └──────────────────┴── todos consultam PAR e CAD
                                                      (futuros módulos plugam aqui)
```

## Núcleo (NUC): transversal, não é um módulo de negócio
| Funcionalidade | Regras |
|---|---|
| Ativação por QR, login da equipe, MFA do secretário | RN-002, RN-002a |
| Papéis acumuláveis e troca de papel | RN-035 |
| Auditoria (com o papel usado e a autoaprovação) | RN-082, RN-037 |
| LGPD: consentimento e execução do expurgo | RN-080, RN-081 |
| Agendador diário (despacha os jobs de cada módulo) | ADR-001 |
| Motor de PDF (preencher sem achatar) e formulário sobreposto ao PDF | RN-070, RN-092 |
| Notificações *(2ª etapa)* | ADR-001 |

## Parâmetros do Sistema (PAR)
| Funcionalidade | Regras |
|---|---|
| Parâmetros versionados por vigência, carregados na inicialização | RN-090 |
| Prazos: 6, 7, 15, 16, 20, tolerância offline de 24h | RN-003, RN-003a, RN-009a, RN-022, RN-030 |
| Metas: 15/30 (auxiliar), 50/600/560 (regular), limite de 55 | RN-041, RN-050–057 |
| Flags: `consolidar_especiais`, `expurgo_habilitado` | RN-056, RN-081 |
| Retenção por tipo de dado, validade do QR, tamanho do anexo | RN-081, RN-002, RN-046 |
| Agenda / calendário (meses especiais) | RN-044 |
| Versões do termo de consentimento | RN-080 |

## Cadastro (CAD)
| Funcionalidade | Regras |
|---|---|
| Publicador: dados do S-21, idade, endereço, contatos, emergência | §Cadastro |
| Privilégios (ancião/SM), esperança, designação de pioneiro regular/especial/missionário, enfermo | RN-055, RN-056 |
| Grupos (quantidade variável), SG, ajudantes (só registro) | RN-033, Q-01 |
| Mudança de grupo com vigência; visibilidade pelo grupo atual | RN-033, RN-036 |
| Situação cadastral (mudou-se/transferência, removido, falecido) e saúde, com datas | RN-020, RN-065 |
| Membros da Comissão de Serviço | RN-040 |

## Reuniões (REU)
| Funcionalidade | Regras |
|---|---|
| Assistência S-3 por reunião (meio e fim de semana), lançada pelo secretário | RN-034 |
| Médias mensais → contrato consumido pelo relatório da congregação (ATV) | RN-071 |
| *Futuro:* programação, designações etc. | — |

## Atividades de Publicadores (ATV): o núcleo do MVP
| Submódulo | Funcionalidade | Regras |
|---|---|---|
| **Relatório S-4** | PWA idêntico ao original, offline, em nome de, classificação (prazo/atraso/retroativo/reenvio), status | RN-001–011, RN-092 |
| **Revisão do grupo** | Conciliação, aprovação/recusa, autoaprovação, observações | RN-010, RN-020–024, RN-037 |
| **Fechamentos** | Dia 7, grupo (15/16), liberação temporária, congregação (20), reabertura até o fim do mês, fila de tardios | RN-003a, RN-022–032 |
| **Pioneiros auxiliares** | S-205b idêntico, aprovação da CS, petição em papel com anexo, designação por mês, cancelamento automático/reativação | RN-040–046 |
| **Pioneiros regulares** | Crédito de horas, alertas de média necessária, meta proporcional, apuração anual em faixas | RN-050–057 |
| **Status do publicador** | Regular/irregular/inativo e histórico mensal | RN-060–065 |
| **Documentos** | S-21 editável, relatório da congregação, importação do AS 2026 a partir de S-21 em PDF | RN-070, RN-071, RN-091 |
| **Análise** | Painéis por status e situação dos pioneiros regulares | §Funcionalidades |

## Dependências permitidas
- Todos → NUC e PAR.
- ATV → CAD (publicador, grupo, designações) e REU (média de assistência).
- REU → CAD (somente se precisar, no futuro).
- **Proibido:** CAD ou PAR dependerem de ATV/REU. Um módulo novo nunca altera um existente: ele consome contratos ou publica os próprios.

## Organização proposta
- `specs/03-modulos/{nucleo,parametros,cadastro,reunioes,atividades/<submodulo>}`.
- Código em monorepo: `packages/<modulo>`.
- Coleções com prefixo do módulo: `cad_publicadores`, `atv_relatorios`, `reu_assistencia`, `par_parametros`…
- Regras renumeradas com o prefixo do módulo: `CAD-001`, `ATV-001`, `REU-001`, `PAR-001`, `NUC-001`. As antigas `RN-xxx` ficam como alias, para não perder a rastreabilidade.
