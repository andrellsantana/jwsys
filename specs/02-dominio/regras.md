# Catálogo de Regras de Negócio

Os valores marcados com `⚙` vêm da tabela de **Parâmetros** (RN-090).

## Envio de relatórios (S-4)
- **RN-001** Cada grupo tem um link próprio para o PWA do S-4.
- **RN-002** Ativação do aparelho por **QR code individual**:
  - Depois de cadastrar o publicador, o secretário ou o SG gera um QR de ativação de uso único, com validade ⚙ em horas.
  - O publicador lê o QR, aceita o termo de consentimento e o aparelho fica vinculado ao cadastro dele. O nome é exibido e guardado no aparelho.
  - Trocou de aparelho: um novo QR é gerado, e o vínculo anterior é revogado.
- **RN-002a** Um envio pelo link do grupo **sem ativação** é aceito como "não identificado" e precisa de conciliação pelo SG (RN-010). Isso serve para casos excepcionais.
- **RN-003** O envio feito até o dia ⚙6 (inclusive) do mês seguinte, de um mês ainda sem relatório, é **aceito** automaticamente.
- **RN-003a** **Bloqueio do dia ⚙7**, às 00:00:
  - o envio normal do publicador é encerrado;
  - do dia 7 ao dia 15, o SG analisa e aprova os relatórios recebidos;
  - todo envio feito a partir do dia 7 é retroativo e segue o RN-004.
  (Interpretação a confirmar em Q-10.)
- **RN-004** Reenvio e envio retroativo (após o dia ⚙6 ou de qualquer mês passado) ficam **aguardando aprovação** do SG.
- **RN-005** A recusa exige um motivo, que o publicador vê.
- **RN-006** Todo envio registra a data e a hora da transmissão, o tipo (envio, reenvio ou retroativo), o aparelho e o autor.
- **RN-007** O envio em nome de outro exige que o remetente se identifique e escreva uma justificativa.
- **RN-008** O publicador vê no celular a lista dos próprios envios e o status de cada um: aceito, aguardando ou recusado.
- **RN-009** O rascunho fica salvo no aparelho. Sem conexão, o envio entra em uma fila que é transmitida quando a conexão voltar.
- **RN-010** O SG concilia o nome do relatório com o publicador cadastrado. A conciliação vincula o aparelho ao publicador para os envios futuros.
- **RN-011** O campo Horas só aparece para pioneiro auxiliar, regular ou especial. O Crédito de horas só aparece para pioneiro regular.

## Superintendente de grupo
- **RN-020** O SG pode acrescentar observações e registrar saúde, mudança e remoção, cada uma com data. O secretário também pode ajustar esses três últimos.
- **RN-021** O SG não altera os números enviados pelo publicador.
- **RN-022** O fechamento do grupo é manual até o dia ⚙15. No dia ⚙16, às 00:00, fecha automaticamente. Quem não enviou fica como "não relatou".
- **RN-023** O fechamento do grupo restringe somente o SG. Para editar depois disso, o SG pede uma liberação temporária ao secretário, que define a data do novo bloqueio.

## Secretário e congregação
- **RN-030** O fechamento da congregação acontece até o dia ⚙20 e trava o mês para todos.
- **RN-031** O secretário pode reabrir o mês da congregação **somente para si**, com motivo e registro na auditoria. Isso **não** reabre os grupos. Para reabrir um grupo específico, vale a liberação temporária do RN-023.
- **RN-032** Relatório tardio é o retroativo de um mês já fechado na congregação, aprovado pelo SG.
  - Ele vai para a fila do secretário.
  - Se o secretário aceitar: registra a data do aceite, vale para o S-21 e para o status (regular/irregular) do mês original, e entra no **próximo** relatório da congregação.
  - Depois que o mês de envio se encerrou, o relatório da congregação daquele mês fechado **nunca é alterado**.
- **RN-033** O secretário move publicadores de grupo (com histórico por data de vigência) e define o SG e os ajudantes.
- **RN-034** Só o secretário lança a assistência (S-3), reunião por reunião. A média mensal é calculada, sem separar presencial de online.

## Pioneiros
- **RN-040** A petição de pioneiro auxiliar segue o **S-205b**:
  - meses solicitados, ou o quadrículo "continuamente até segundo aviso";
  - data;
  - nome em letra de forma;
  - assinatura, substituída pelo aceite eletrônico do publicador no app;
  - os 3 critérios avaliados pela CS (bom exemplo/reputação; não repreendido nem readmitido nos últimos 12 meses e sem restrições; SG consultado);
  - a aprovação, que só a CS dá, com a rubrica de cada membro substituída pelo registro de aprovação de cada membro com data.

  O sistema gera o PDF preenchido para guardar nos arquivos da congregação.
- **RN-041** A meta do auxiliar é ⚙15h em mês especial e ⚙30h nos demais meses.
- **RN-042** Um auxiliar abaixo da meta conta como publicador naquele mês. As horas dele ficam guardadas em campo separado e não entram nas totalizações.
- **RN-043** Um auxiliar por tempo indeterminado abaixo da meta tem a petição **cancelada automaticamente**. A CS pode reativá-la.
- **RN-044** Os meses especiais são marcados pelo secretário em uma agenda, à medida que forem anunciados.
- **RN-045** **Designação de pioneiro auxiliar:** o publicador é pioneiro auxiliar no mês M se, e somente se, existir uma petição **aprovada pela CS** que cubra M, seja pelos meses listados no S-205b ou por "continuamente" a partir do mês inicial, enquanto não for cancelada. Esse vínculo é o que libera o campo Horas no S-4 e o que define a categoria nas totalizações.
- **RN-046** O secretário pode lançar manualmente uma petição em papel já assinada e aprovada. Ele marca "petição física arquivada" e informa a data e os membros da CS que aprovaram. O sistema registra o lançamento na auditoria.
- **RN-050** O pioneiro regular tem meta anual de ⚙600h (referência de ⚙50h por mês).
- **RN-051** O sistema alerta durante o ano mostrando a **média mensal necessária** = (600 − horas de campo acumuladas) ÷ meses restantes. O cálculo usa sempre 600h.
- **RN-052** O crédito de horas só vale para atividade aprovada. Limite: horas de campo + crédito ≤ ⚙55h no mês. Não é transferível entre meses. O sistema guarda separadamente as **horas em atividade aprovada (total)** e o **crédito considerado**.
- **RN-053** O crédito nunca entra no relatório da congregação (Betel). No S-21, Horas = só campo, e Observações = "Atividade aprovada: Xh; crédito considerado: Yh".
- **RN-054** Apuração do fim do ano em faixas, com M = meta anual ajustada (RN-057) e L = limite de análise ajustado:
  - campo ≥ M → **continua**;
  - L ≤ campo < M → **continua**, sem análise;
  - campo < L, mas campo + crédito ≥ L → **análise da CS**, com indicação explícita de que só chega ao limite por causa do crédito;
  - campo + crédito < L → **cancelamento sugerido**.
- **RN-057** **Meta proporcional:** quem passou a ser pioneiro regular no meio do ano, ou deixou de ser, tem M = ⚙600 × (meses como pioneiro ÷ 12) e L = ⚙560 × (meses ÷ 12). Os meses como pioneiro enfermo são excluídos da contagem. A média necessária do RN-051 usa M.
- **RN-055** O pioneiro enfermo não tem exigência de horas e fica fora dos alertas.
- **RN-056** Pioneiros especiais e missionários são habilitados pelo secretário. Os relatórios deles são consolidados em separado. O parâmetro ⚙`consolidar_especiais` tem padrão `false`.

## Status do publicador
- **RN-060** Considera somente meses **fechados** na congregação.
- **RN-061** Inativo = ⚙6 meses consecutivos sem relatar.
- **RN-062** Ativo-irregular = deixou de relatar pelo menos 1 dos últimos ⚙6 meses, sem chegar a ser inativo.
- **RN-063** Ativo-regular = relatou todos os últimos 6 meses. Um publicador novo, com menos de 6 meses, é regular.
- **RN-064** Um inativo que volta a relatar fica ativo de novo.
- **RN-065** As situações cadastrais (mudou-se com transferência do S-21, removido, falecido) tiram o publicador das contagens a partir da data registrada.

## Documentos
- **RN-070** O S-21 é gerado preenchendo o PDF oficial (`S-21_T.pdf`), com layout **idêntico**, 1 por publicador por ano de serviço. O PDF **continua editável**: os campos AcroForm são mantidos, sem achatar o formulário.
- **RN-071** O relatório da congregação é uma tela pronta para copiar, com: ativos; média de assistência do fim de semana; publicadores (relatórios e estudos); pioneiros auxiliares e regulares (relatórios, horas e estudos).

## Transversais
- **RN-080** Consentimento LGPD com versão do termo e data/hora, e possibilidade de revogação.
- **RN-081** Retenção de dados ⚙parametrizável, com expurgo e anonimização agendados.
- **RN-082** Auditoria de toda alteração: quem, quando, antes e depois, e motivo.
- **RN-090** Parâmetros versionados por vigência e editáveis só pelo secretário.
- **RN-091** **Importação do AS 2026 a partir dos S-21 em PDF:**
  - Lê todos os PDFs de uma pasta e extrai os campos AcroForm: cabeçalho e os 12 meses (participou, estudos, auxiliar, horas, observações).
  - Mostra uma prévia com a conciliação de cada cartão com o cadastro e a lista de divergências antes de gravar.
  - Os registros importados passam a fazer parte do sistema, como meses **fechados** com origem "importado S-21". Eles valem para status, apuração e painéis.
- **RN-092** Os **formulários online do S-4 e do S-205b reproduzem os modelos do repositório**: mesmos títulos, rótulos, textos, ordem e disposição dos campos. Os campos extras do sistema (Crédito de horas, Grupo, envio em nome de) ficam visualmente separados do formulário oficial.
