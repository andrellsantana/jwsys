# UX — Secretário

Proposta de 04/11/2026. Uso principal no computador, com versão no celular para consulta e aprovações rápidas. MFA obrigatório (NUC).

## Navegação (menu lateral, por módulo)
```
Painel · Pendências · Atividades ▸ (Relatórios | Fechamentos | Pioneiros | Status | Documentos)
Cadastro ▸ (Publicadores | Grupos) · Reuniões ▸ (Assistência S-3) · Parâmetros ▸ (Valores | Agenda | Termos LGPD | Expurgo) · Auditoria
```
O seletor de papel no topo alterna para "SG do Grupo X" e "Publicador" (NUC-003). Os módulos futuros entram como novos itens do menu.

## UX-SEC-001 Painel da congregação
- **Linha do tempo do mês:** dia 6 (prazo) → dia 7 (bloqueio) → dia 15/16 (grupos) → dia 20 (congregação) → fim do mês (imutável), com o dia atual destacado.
- **Um cartão por grupo:** enviados/total, pendências, estado (aberto, fechado, liberado até…), SG.
- **Totais da congregação**, a partir de `atv_totais`: relatórios, estudos e horas por categoria.
- Atalho **"Fechar o mês da congregação"**, habilitado só com todos os grupos fechados, ou com confirmação explícita se faltar algum (ver Q-18).

## UX-SEC-002 Pendências (filas)
Abas com contador:
- Tardios a aceitar (ATV-021)
- Atrasos aprovados após o fechamento (ATV-017)
- Pedidos de liberação dos SGs (ATV-016)
- Petições de auxiliar aguardando a CS
- Envios não identificados há mais de N dias
- Alertas de pioneiros regulares

Cada item tem as ações dele (aceitar, recusar com motivo, liberar até uma data).

## UX-SEC-003 Atividades
- **Relatórios:** o mesmo grid do SG, com o filtro "Grupo" e sem restrição de grupo.
- **Fechamentos:** calendário por mês com o estado de cada grupo e da congregação; reabrir com motivo (até o fim do mês).
- **Pioneiros:** auxiliares (petições e designações por mês) e regulares (acumulado, meta proporcional, média necessária, faixa prevista); visão do fim do ano com os filtros "continua / análise / cancelamento sugerido".
- **Status:** distribuição de regular, irregular e inativo, com o histórico por publicador.
- **Documentos:** relatório da congregação (tela para copiar), S-21 individual ou em lote (ZIP de PDFs editáveis), S-205b, importação de S-21 (assistente: escolher pasta → prévia/conciliação → confirmar).

## UX-SEC-004 Cadastro
- Lista de publicadores com busca e filtros (grupo, privilégio, pioneiro, situação, status).
- Ficha com abas: Dados S-21 · Endereço e contatos · Privilégios e pioneiro · Grupo (histórico) · Saúde e situação · Aparelhos e QR (gerar ou revogar) · Consentimentos.
- Grupos: criar ou desativar, definir SG e ajudantes, mover publicadores (arrastar ou ação em lote, com data de vigência).

## UX-SEC-005 Reuniões
- Lançamento do S-3 em formato de calendário: um campo por reunião (meio e fim de semana), com a média mensal calculada ao vivo.

## UX-SEC-006 Parâmetros
- Valores versionados por vigência, com a descrição e o valor padrão de cada um.
- Agenda dos meses especiais.
- Versões do termo LGPD.
- Expurgo: desligado por padrão; ao ligar, mostra uma prévia (NUC-005).

## UX-SEC-007 Auditoria
- Linha do tempo pesquisável: quem, papel, ação, alvo, antes/depois, motivo.
