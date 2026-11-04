# Núcleo (NUC) — Especificação

**Status:** em revisão (04/11/2026).
**Regras:** NUC-001…NUC-013. **ADRs:** 001, 002, 003. **UX:** `05-ux/*`.

## Objetivo
Dar a todos os módulos, de forma única: **quem é o usuário**, **o que ele pode fazer**, **como entra e sai**, **o que ele consentiu** e **o registro de tudo o que foi feito**.

## Fora do escopo do Núcleo
- Cadastro de publicadores, grupos e perfis (é do Cadastro; o Núcleo só **lê** os perfis vigentes pelo contrato `cad.perfisVigentes(pubId)`).
- Parâmetros (é do PAR; o Núcleo só lê `par.valor(chave, data)`).
- Notificações (2ª etapa).

---

## Histórias e critérios de aceite

### H-NUC-01 Ativar o celular do publicador com QR (NUC-001, NUC-004)
> Como **publicador**, quero ler um QR e já ficar com o app pronto, sem senha.

- **CA-1** Dado um QR válido e não usado, quando o publicador o lê, então o app mostra o termo de consentimento LGPD vigente.
- **CA-2** Quando aceita o termo, então o aparelho fica ligado ao usuário dele, o consentimento é gravado com a versão e a data/hora, e a tela inicial mostra "Olá, <nome>".
- **CA-3** Dado um QR já usado ou vencido (⚙ `qr_validade_horas`), quando é lido, então aparece "Este código não vale mais. Peça um novo ao seu superintendente."
- **CA-4** Quando ele recusa o termo, então nada é gravado e o aparelho não fica ativado.
- **CA-5** Quando um novo QR é gerado para a mesma pessoa e usado, então o aparelho anterior é revogado e desconectado (NUC-011).
- **CA-6** O app continua logado depois de fechado e reaberto, sem pedir nada.

### H-NUC-02 Gerar QR de ativação (NUC-001)
> Como **SG ou SEC**, quero gerar o QR de um publicador para entregar no salão ou por mensagem.

- **CA-1** O SG só gera QR para publicadores do grupo que supervisiona. O SEC gera para qualquer um.
- **CA-2** O QR aparece na tela, com a opção de mostrar em tela cheia, a validade e o nome do publicador.
- **CA-3** Gerar um novo QR invalida o anterior ainda não usado.
- **CA-4** Cada geração fica na auditoria.

### H-NUC-03 Entrada da equipe (NUC-007, NUC-012)
> Como **SG, SEC, SS, CCA…**, quero entrar com minha conta Google ou um link por e-mail e ver tudo o que me foi concedido.

- **CA-1** O login usa o e-mail cadastrado no Cadastro. Um e-mail desconhecido recebe "Este e-mail não está cadastrado. Fale com o secretário." sem revelar se existe.
- **CA-2** Depois de entrar, o usuário tem os papéis de todos os seus perfis vigentes e concessões.
- **CA-3** Um papel `exigeMFA` (SEC) só fica ativo depois do segundo fator (TOTP). Sem ele, a pessoa entra só com os outros papéis e vê o aviso "Ative a verificação em duas etapas para acessar a Secretaria".
- **CA-4** A mesma pessoa pode entrar no celular pelo QR e no computador pelo Google: é o **mesmo usuário**.

### H-NUC-04 Trocar o papel ativo (NUC-003)
> Como **usuário com vários papéis**, quero alternar entre eles sem sair.

- **CA-1** O seletor mostra só os papéis vigentes do usuário (ex.: Publicador · SG do Grupo 2 · Secretaria).
- **CA-2** A troca muda o menu e as telas, **sem desconectar** (NUC-011).
- **CA-3** Toda ação gravada leva o `papelUsado`.
- **CA-4** O último papel usado é lembrado no aparelho.

### H-NUC-05 Conceder e revogar papéis manualmente (NUC-009)
> Como **master, CCA ou SEC**, quero dar a alguém um papel temporário.

- **CA-1** Só esses três veem a tela "Concessões".
- **CA-2** A concessão exige o papel, o escopo (se houver), o motivo e, opcionalmente, a validade.
- **CA-3** O usuário que recebe o papel o vê na hora, sem desconexão, com o aviso "Você recebeu novas permissões".
- **CA-4** Na revogação ou no vencimento, o usuário é desconectado na hora (NUC-011).
- **CA-5** Ninguém concede a si mesmo um papel que não tem, exceto o master.

### H-NUC-06 Desconexão em tempo real (NUC-011)
> Como **sistema**, quero que quem perdeu um poder não continue usando-o nem por um segundo.

- **CA-1** Dado um usuário com o app aberto, quando perde um papel, então em até ⚙5 s o app sai e mostra "Suas permissões mudaram. Entre novamente."
- **CA-2** Dado um token antigo (app fechado ou adulterado), quando ele tenta ler ou gravar, então o servidor recusa.
- **CA-3** Trocar de grupo como SG conta como perda de poder (perde o grupo anterior).
- **CA-4** Ganhar um papel ou trocar o papel ativo não desconecta.

### H-NUC-07 Administrador master (NUC-010)
> Como **administrador do sistema**, quero acesso total, com segurança máxima.

- **CA-1** A conta master é diferente da conta pessoal, mesmo sendo a mesma pessoa.
- **CA-2** O primeiro acesso, e todo acesso depois de uma nova versão do termo, exige aceitar o termo de confidencialidade. Sem o aceite, nada fica disponível.
- **CA-3** MFA obrigatório. A sessão expira após ⚙30 min sem uso.
- **CA-4** A faixa "Modo administrador" fica sempre visível.
- **CA-5** Toda ação do master aparece destacada na auditoria.

### H-NUC-08 Consentimento LGPD (NUC-004)
> Como **titular dos dados**, quero saber o que aceitei e poder revogar.

- **CA-1** Em Ajuda › Privacidade, o usuário vê o termo aceito (versão e data) e o botão "Revogar consentimento".
- **CA-2** A revogação pede confirmação na tela, desativa o aparelho, avisa o SEC e fica registrada. Os registros já feitos continuam (base: legítimo interesse/obrigação da congregação; ver Q-26).
- **CA-3** Uma nova versão do termo exige novo aceite no próximo acesso.

### H-NUC-09 Auditoria (NUC-006)
> Como **SEC ou master**, quero ver quem fez o quê, quando e com qual papel.

- **CA-1** Todo comando grava: quem, `papelUsado`, quando, ação, alvo, antes, depois e motivo.
- **CA-2** Ninguém altera nem apaga registros de auditoria (nem o master).
- **CA-3** A tela tem filtros por pessoa, ação, módulo e período, e destaca o master e as autoaprovações.

### H-NUC-10 Expurgo controlado (NUC-005)
> Como **SEC**, quero que nada seja apagado sem a minha decisão.

- **CA-1** Com `expurgo_habilitado = false` (padrão), o job não apaga nada.
- **CA-2** Ao ligar, o SEC vê uma prévia (quantidade por tipo de dado) e confirma na tela.
- **CA-3** Cada execução registra o que foi expurgado ou anonimizado (só as contagens).

### H-NUC-11 Preferências de aparência (NUC-013)
- **CA-1** A paleta, a fonte grande, o alto contraste e o modo escuro escolhidos ficam no usuário e aparecem iguais em outro aparelho dele.
- **CA-2** Todas começam desativadas, e a paleta padrão é Azul sereno.

---

## Contratos publicados pelo Núcleo (para os outros módulos)
| Contrato | Uso |
|---|---|
| `defineModulo(manifesto)` | Cada módulo declara as permissões, os papéis e as derivações dele (ADR-003). |
| `pode(ctx, permissao, escopo?)` | Checagem nas Cloud Functions. |
| `comando(nome, handler)` | Envelope de toda callable: autentica, verifica a versão da sessão, checa permissão e grava a auditoria. |
| `auditar(evento)` | Registro de eventos de jobs e gatilhos. |
| `agendar(modulo, tarefa)` | Registra a tarefa no job diário único. |
| `preencherPdf(modelo, campos)` / `FolhaPdf` (React) | Motor de PDF e formulário sobreposto (ATV-013, ATV-041). |

## Questões
- **Q-26 Revogação do consentimento:** depois de revogar, os relatórios já enviados continuam no sistema (obrigação da congregação) e o publicador passa a relatar só em papel/"em nome de"? Ou a revogação leva à anonimização?
