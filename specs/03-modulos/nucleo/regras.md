# Regras — Núcleo (NUC)

Os valores marcados com `⚙` vêm de Parâmetros (PAR-001). O ID antigo aparece entre parênteses.

- **NUC-001** _(RN-002)_ Ativação do aparelho por **QR code individual**:
  - Depois de cadastrar o publicador, o secretário ou o SG gera um QR de ativação de uso único, com validade ⚙ em horas.
  - O publicador lê o QR, aceita o termo de consentimento e o aparelho fica vinculado ao cadastro dele. O nome é exibido e guardado no aparelho.
  - Trocou de aparelho: um novo QR é gerado, e o vínculo anterior é revogado.
- **NUC-002** _(RN-002a)_ Um envio pelo link do grupo **sem ativação** é aceito como "não identificado" e precisa de conciliação pelo SG (ATV-011). Isso serve para casos excepcionais.
- **NUC-003** _(RN-035)_ **Papéis acumuláveis:** uma pessoa é sempre publicador e pode acumular os papéis de SG de um grupo (inclusive um grupo diferente do seu), secretário e membro da CS. Exemplo: o secretário também é SG do Grupo 2. Cada ação registra na auditoria **com qual papel** foi feita. As telas oferecem a troca de papel (Publicador / SG do Grupo X / Secretaria / CS).
- **NUC-004** _(RN-080)_ Consentimento LGPD com versão do termo e data/hora, e possibilidade de revogação.
- **NUC-005** _(RN-081)_ **Expurgo:** prazos de retenção ⚙ por tipo de dado, com o parâmetro ⚙`expurgo_habilitado` = **false** por padrão. Nada é expurgado ou anonimizado até o secretário habilitar explicitamente (ação auditada). Ao habilitar, o sistema mostra uma prévia do que seria afetado.
- **NUC-006** _(RN-082)_ Auditoria de toda alteração: quem, quando, antes e depois, e motivo.
- **NUC-007** **Usuário único por pessoa:** cada pessoa tem um usuário (`nuc_usuarios`), ligado a um `pubId`. Os métodos de entrada (aparelho ativado por QR, Google, link por e-mail) são formas de entrar **no mesmo usuário**. Todos os poderes vêm dos perfis e das concessões desse usuário (ADR-003 §0).
- **NUC-008** **Perfis geram papéis:** os perfis vigentes do Cadastro (PUB, SG, SEC, SS, CCA, CCP, CL…) geram papéis automaticamente pelas derivações dos manifestos (ADR-003 §1–2). A CS = CCA + SEC + SS.
- **NUC-009** **Concessão manual** de papel pelo administrador, pelo CCA ou pelo SEC, com motivo obrigatório e validade opcional. A remoção no vencimento é automática (ADR-003 §6).
- **NUC-010** **Administrador do sistema (master):** conta técnica separada do login pessoal, com acesso total, MFA obrigatório, sessão de ⚙30 min sem uso, aviso "Modo administrador" e auditoria destacada. Só funciona com o **termo de confidencialidade** vigente aceito (ADR-003 §7).
- **NUC-011** **Desconexão em tempo real:** perder qualquer poder desconecta o usuário imediatamente, no app e no servidor. Ganhar poder ou trocar o papel ativo não desconecta (ADR-003 §8).
- **NUC-012** **MFA por papel:** um papel marcado `exigeMFA` (SEC e master no MVP) só vale numa sessão com segundo fator.
- **NUC-013** **Preferências do usuário:** paleta, fonte grande, alto contraste e modo escuro ficam salvos no perfil do usuário e valem em todos os aparelhos dele (UX). Antes do login, valem as preferências do próprio aparelho.
