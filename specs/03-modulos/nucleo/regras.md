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
