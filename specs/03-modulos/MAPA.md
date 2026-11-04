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
| Ativação por QR, login da equipe, MFA do secretário | NUC-001, NUC-002 |
| Papéis acumuláveis e troca de papel | NUC-003 |
| Auditoria (com o papel usado e a autoaprovação) | NUC-006, ATV-018 |
| LGPD: consentimento e execução do expurgo | NUC-004, NUC-005 |
| Agendador diário (despacha os jobs de cada módulo) | ADR-001 |
| Motor de PDF (preencher sem achatar) e formulário sobreposto ao PDF | ATV-041, ATV-013 |
| Notificações *(2ª etapa)* | ADR-001 |

## Parâmetros do Sistema (PAR)
| Funcionalidade | Regras |
|---|---|
| Parâmetros versionados por vigência, carregados na inicialização | PAR-001 |
| Prazos: 6, 7, 15, 16, 20, tolerância offline de 24h | ATV-002, ATV-003, ATV-010, ATV-015, ATV-019 |
| Metas: 15/30 (auxiliar), 50/600/560 (regular), limite de 55 | ATV-023, ATV-028…ATV-035 |
| Flags: `consolidar_especiais`, `expurgo_habilitado` | ATV-034, NUC-005 |
| Retenção por tipo de dado, validade do QR, tamanho do anexo | NUC-005, NUC-001, ATV-027 |
| Agenda / calendário (meses especiais) | PAR-002 |
| Versões do termo de consentimento | NUC-004 |

## Cadastro (CAD)
| Funcionalidade | Regras |
|---|---|
| Publicador: dados do S-21, idade, endereço, contatos, emergência | §Cadastro |
| Privilégios (ancião/SM), esperança, designação de pioneiro regular/especial/missionário, enfermo | ATV-033, ATV-034 |
| Grupos (quantidade variável), SG, ajudantes (só registro) | CAD-001, Q-01 |
| Mudança de grupo com vigência; visibilidade pelo grupo atual | CAD-001, CAD-002 |
| Situação cadastral (mudou-se/transferência, removido, falecido) e saúde, com datas | CAD-003, CAD-004 |
| Membros da Comissão de Serviço | ATV-022 |

## Reuniões (REU)
| Funcionalidade | Regras |
|---|---|
| Assistência S-3 por reunião (meio e fim de semana), lançada pelo secretário | REU-001 |
| Médias mensais → contrato consumido pelo relatório da congregação (ATV) | ATV-042 |
| *Futuro:* designações das reuniões do meio e do fim de semana, discursos públicos | — |

## Atividades de Publicadores (ATV): o núcleo do MVP
| Submódulo | Funcionalidade | Regras |
|---|---|---|
| **Relatório S-4** | PWA idêntico ao original, offline, em nome de, classificação (prazo/atraso/retroativo/reenvio), status | ATV-001…ATV-013 |
| **Revisão do grupo** | Conciliação, aprovação/recusa, autoaprovação, observações | ATV-011, CAD-003, ATV-014…ATV-018 |
| **Fechamentos** | Dia 7, grupo (15/16), liberação temporária, congregação (20), reabertura até o fim do mês, fila de tardios | ATV-003, ATV-015…ATV-021 |
| **Pioneiros auxiliares** | S-205b idêntico, aprovação da CS, petição em papel com anexo, designação por mês, cancelamento automático/reativação | ATV-022…ATV-027 |
| **Pioneiros regulares** | Crédito de horas, alertas de média necessária, meta proporcional, apuração anual em faixas | ATV-028…ATV-035 |
| **Status do publicador** | Regular/irregular/inativo e histórico mensal | ATV-036…ATV-040, CAD-004 |
| **Documentos** | S-21 editável, relatório da congregação, importação do AS 2026 a partir de S-21 em PDF | ATV-041, ATV-042, ATV-043 |
| **Análise** | Painéis por status e situação dos pioneiros regulares | §Funcionalidades |

## Dependências permitidas
- Todos → NUC e PAR.
- ATV → CAD (publicador, grupo, designações) e REU (média de assistência).
- REU → CAD (somente se precisar, no futuro).
- **Proibido:** CAD ou PAR dependerem de ATV/REU. Um módulo novo nunca altera um existente: ele consome contratos ou publica os próprios.

## Módulos futuros (fora do MVP)
| Módulo | Sigla | Depende de | Observação |
|---|---|---|---|
| Reuniões: designações e discursos públicos | REU | CAD, PAR | Amplia o REU; provavelmente traz papéis novos (ex.: superintendente da reunião, coordenador de discursos). |
| Pastoreio | PAS | CAD, (ATV: status) | Dados muito sensíveis. Acesso restrito aos anciãos, com regras próprias. |
| Contas | CTA | CAD, PAR | Financeiro. Papel novo (servo de contas), auditoria reforçada. |
| Limpeza | LIM | CAD | Escalas por grupo. |
| Territórios | TER | CAD | Mapas e designações de território. Papel novo (servo de territórios). |
| Sistema Coordenado de Transporte | SCT | CAD | Caronas e transporte. Dados de endereço. |

Consequência para o Núcleo: os papéis são **um registro extensível** (cada módulo declara os seus), e não uma lista fixa. Isso entra no ADR-003.

## Organização (aplicada)
- Regras por módulo em `specs/03-modulos/<modulo>/regras.md`, com os prefixos `NUC-`, `PAR-`, `CAD-`, `REU-` e `ATV-`. O índice dos IDs antigos (`RN-xxx`) fica em `specs/02-dominio/regras.md`.
- Código em monorepo: `packages/<modulo>`.
- Coleções do Firestore com prefixo do módulo (`nuc_`, `par_`, `cad_`, `reu_`, `atv_`), conforme o ADR-002.
