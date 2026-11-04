# Cadastro (CAD) — Especificação

**Status:** em revisão (04/11/2026). **Regras:** CAD-001…CAD-012.

## Objetivo
Ser a fonte única de **quem é quem** na congregação: publicadores, grupos e perfis (designações) com vigência. Os outros módulos só leem o Cadastro pelos contratos dele.

---

## Histórias e critérios de aceite

### H-CAD-01 Cadastrar publicador
> Como **SEC**, quero cadastrar um publicador com os dados do S-21 e de contato.
- **CA-1** Formulário em abas: Dados S-21 · Endereço e contatos · Contato de emergência · Grupo.
- **CA-2** Obrigatórios: nome completo, sexo, outras ovelhas/ungido, grupo. Datas de nascimento e batismo podem ficar vazias (publicador não batizado).
- **CA-3** A idade é calculada e exibida, nunca digitada.
- **CA-4** CEP com máscara. A busca de endereço pelo CEP é opcional (Q-29).
- **CA-5** Aviso de possível duplicidade antes de salvar (CAD-011).
- **CA-6** Depois de salvar, o botão "Gerar QR de ativação" fica disponível (H-NUC-02).
- **CA-7** Tudo fica na auditoria.

### H-CAD-02 Editar e consultar o publicador
- **CA-1** O SEC edita todos os campos, com motivo nas alterações de nome, datas e sexo.
- **CA-2** A ficha mostra também o status (vindo de ATV), os perfis vigentes e passados, o histórico de grupos, os aparelhos ativos e o consentimento.
- **CA-3** O SG vê os publicadores do grupo que supervisiona, com os campos definidos em Q-28.
- **CA-4** O publicador vê os próprios dados no app (Q-30).

### H-CAD-03 Grupos
> Como **SEC**, quero organizar os grupos.
- **CA-1** Criar e renomear grupos. Desativar só sem membros vigentes.
- **CA-2** A tela do grupo mostra o SG vigente, os ajudantes e os membros.
- **CA-3** Definir o SG do grupo é registrar o perfil SG com o grupo (H-CAD-04).

### H-CAD-04 Perfis (designações)
> Como **SEC**, quero registrar as designações de cada pessoa, com datas.
- **CA-1** Na ficha, a aba "Perfis" lista os vigentes e os passados, com início e fim.
- **CA-2** Adicionar um perfil pede o tipo, o escopo (grupo, para SG) e a data de início. Encerrar pede a data de fim.
- **CA-3** As validações do CAD-008 são aplicadas, com mensagens claras (ex.: "O Grupo 2 já tem um SG vigente: Carlos Mendes. Encerre a designação dele antes.").
- **CA-4** Uma tela "Designações da congregação" mostra quem ocupa cada perfil único (SEC, SS, CCA, CCP, CL) e os SGs de cada grupo.
- **CA-5** Toda mudança dispara o recálculo de acesso. Quem perdeu poder é desconectado (NUC-011).

### H-CAD-05 Mover publicadores de grupo
> Como **SEC**, quero mudar publicadores de grupo a partir de um mês.
- **CA-1** Mudança individual na ficha, ou em lote (selecionar vários → grupo de destino → mês de vigência).
- **CA-2** A vigência não pode ser anterior ao mês aberto mais antigo.
- **CA-3** O histórico de grupos do publicador mostra cada período.
- **CA-4** O SG antigo deixa de ver os meses novos; o novo SG não vê os meses antigos (só o histórico de status) (CAD-002).

### H-CAD-06 Situação cadastral e saúde
> Como **SG ou SEC**, quero registrar saúde, mudança, remoção e falecimento.
- **CA-1** O SG registra saúde, mudou-se e removido, com data e observação, no grid (UX-SG-004). Só o SEC registra falecido e corrige qualquer situação.
- **CA-2** A situação retira o publicador das contagens a partir da data (CAD-004), sem apagar nada.
- **CA-3** "Mudou-se" oferece gerar os S-21 para transferência.
- **CA-4** Um publicador que voltou (ex.: mudou-se e retornou) pode ser reativado pelo SEC, com data.

### H-CAD-07 Membros da Comissão de Serviço
- **CA-1** A CS é sempre composta pelos titulares vigentes de CCA, SEC e SS (CAD-005). Não existe cadastro separado.
- **CA-2** A tela "Designações da congregação" mostra a CS atual.

## Contratos publicados
`cad.publicador(pubId)`, `cad.grupoNoMes(pubId, mes)`, `cad.membrosDoGrupo(grupoId, mes)`, `cad.perfisVigentes(pubId, data?)`, `cad.situacaoNoMes(pubId, mes)`, `cad.titular(perfil, data?)`, e eventos `cad.perfilAlterado` e `cad.grupoAlterado`.

## Questões
- **Q-27 Menores de idade:** publicadores com menos de 18 anos precisam do consentimento de um responsável (LGPD, art. 14)? Se sim, quem é o responsável no cadastro (outro publicador ou só o nome)?
- **Q-28 O que o SG vê:** nome, telefone, e-mail, contato de emergência, endereço e status dos publicadores do grupo? Também datas de nascimento e batismo, privilégios e perfis?
- **Q-29 Busca pelo CEP:** usar um serviço público (ex.: ViaCEP) para preencher o endereço? Só o CEP sai do sistema.
- **Q-30 Publicador vê os próprios dados:** no app, o publicador vê o próprio cadastro (só leitura) e pode pedir correção ao secretário?
