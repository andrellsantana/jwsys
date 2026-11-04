# UX — Publicador (PWA do S-4)

Decisões de 04/11/2026. Público: celular, muitos idosos e leigos.

## UX-PUB-001 Tela inicial com 3 itens
- **Enviar relatório do mês:** já abre o mês correto, normalmente o mês anterior.
- **Meus relatórios:** lista por mês com status (✓ Aceito · ⏳ Aguardando aprovação · ✗ Recusado, com o motivo).
- **Ajuda:** passo a passo com figuras e contato do SG.
- O nome do publicador ativado aparece no topo ("Olá, Maria").

## UX-PUB-002 Entrada numérica: botões + / − e teclado numérico
- Os campos sobre a folha continuam idênticos ao original (ATV-013).
- Tocar em Estudos ou Horas abre um **painel inferior** (fora da folha) com o valor em fonte grande, **botões + e − grandes** (mínimo 56 px) e acesso ao **teclado numérico** (`inputmode="numeric"`).
- O valor aparece na folha em tempo real.
- O "Participou no ministério" é uma caixa marcada com toque, com área de toque ampliada.

## UX-PUB-003 Acessibilidade opcional (tudo desativado por padrão)
- **Fonte grande:** amplia a interface. A folha é escalada e permite zoom com pinça.
- **Alto contraste:** na interface (botões, textos e painéis).
- **Modo escuro:** só na interface. A folha do formulário continua branca, como o papel.
- As preferências ficam salvas no aparelho (`localStorage`) e podem ser ligadas em Ajuda › Aparência.
- Base mínima sempre ativa: WCAG 2.2 AA (contraste, foco visível, rótulos para leitores de tela).

## UX-PUB-004 Confirmação de envio
- Depois do envio: uma tela cheia com **"Recebido ✓"**, mês de referência, data e hora do recebimento pelo servidor e o status inicial.
- Envio offline: **"Guardado — será enviado quando houver internet"** (ícone de relógio). Quando sincronizar, muda para "Recebido ✓".
- Botão único: "Voltar ao início".
