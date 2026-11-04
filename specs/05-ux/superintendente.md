# UX — Superintendente de Grupo

Decisões de 04/11/2026. Uso em celular e computador (responsivo). Só mostra o grupo supervisionado hoje (CAD-002).

## UX-SG-001 Painel do mês
- Cabeçalho: grupo, mês de referência, estado (Aberto / Fechado / Liberado até dd/mm) e **contagem regressiva até o dia 15** ("faltam 4 dias para o fechamento").
- Indicadores clicáveis, cada um filtra o grid:
  - ✓ Enviaram (n/total)
  - ✗ Faltam (lista com nomes)
  - 🔗 Aguardando conciliação
  - ⏳ Aguardando aprovação (em atraso, retroativo, reenvio, offline no prazo)
- Botão **"Fechar o mês do grupo"**. Antes de fechar, mostra quem falta e as pendências.

## UX-SG-002 Aprovar ou recusar em um toque
- Em cada pendência: **Aprovar** e **Recusar**.
- A recusa abre um campo de **motivo obrigatório**.
- O reenvio mostra o antes e o depois lado a lado.
- O envio "offline no prazo" mostra as duas horas: a do aparelho e a do servidor.
- A autoaprovação (o próprio relatório do SG) tem um aviso visual (ATV-018).

## UX-SG-003 Grid de registros
- **Escopo:** todos os registros do grupo, de qualquer mês ou ano de serviço, inclusive de quem já saiu do grupo (CAD-002).
- **Filtros:** mês ou intervalo, ano de serviço, publicador, estado (aceito, aguardando, recusado, não relatou), tipo de envio (prazo, atraso, retroativo, reenvio, em nome de), categoria (publicador, auxiliar, regular…), status (regular, irregular, inativo).
- **Colunas:** publicador, mês, participou, estudos, horas, crédito, observações, tipo, estado, recebido em, decidido por/em.
- **Celular:** o grid vira uma lista de cartões, com os filtros num painel deslizante.
- Ordenação por coluna e busca por nome.

## UX-SG-004 Inserir dados direto no grid
- **Tocar ou clicar numa linha** abre o editor daquele publicador e mês, logo acima do grid.
- O editor tem: o que registrar (observação, saúde, mudou-se, removido, relatório em nome do publicador), a data e a descrição. A justificativa é obrigatória no "em nome de".
- O que é salvo **aparece na hora no grid**, na coluna **Anotações**, como etiquetas com o tipo e a data.
- **Teclado:** <kbd>↑</kbd>/<kbd>↓</kbd> navegam pelos nomes, <kbd>Home</kbd>/<kbd>End</kbd> vão ao primeiro e ao último, <kbd>Enter</kbd> abre o editor, <kbd>Esc</kbd> fecha e devolve o foco à linha.
- Com o grupo **fechado**, o editor abre só com o aviso "Mês fechado — peça liberação ao secretário". A exceção é aprovar ou recusar (ATV-017).
