# Regras — Cadastro (CAD)

Os valores marcados com `⚙` vêm de Parâmetros (PAR-001). O ID antigo aparece entre parênteses.

- **CAD-001** _(RN-033)_ O secretário move publicadores de grupo (com histórico por data de vigência) e define o SG e os ajudantes.
- **CAD-002** _(RN-036)_ **Visibilidade do SG = o grupo que ele supervisiona hoje, com todo o histórico desse grupo.** Cada relatório guarda o grupo a que o publicador pertencia **naquele mês**.
  - Exemplo: o SG passa do Grupo 1 para o Grupo 4. Ele deixa de ver o Grupo 1 imediatamente. No Grupo 4, vê todos os relatórios de todos os meses, inclusive de Silvia e Jandira, que saíram do grupo, e os atuais de Claudia e Roberto.
  - Os meses anteriores à entrada de Claudia e Roberto no Grupo 4 **não são visíveis** para o SG do Grupo 4: eles pertencem ao grupo de origem. O SG vê apenas o **histórico de status** deles (ativo/inativo; regular/irregular/inativo, mês a mês), calculado pelo sistema, sem os números dos relatórios.
- **CAD-003** _(RN-020)_ O SG pode acrescentar observações e registrar saúde, mudança e remoção, cada uma com data. O secretário também pode ajustar esses três últimos.
- **CAD-004** _(RN-065)_ As situações cadastrais (mudou-se com transferência do S-21, removido, falecido) tiram o publicador das contagens a partir da data registrada.
- **CAD-005** **Composição da CS:** o secretário registra quem é o coordenador do corpo de anciãos, o secretário e o superintendente de serviço, com datas de vigência. Esses 3 recebem o papel CS.
- **CAD-006** **Dados do publicador:** os do S-21 (nome completo, nascimento, batismo, sexo, outras ovelhas/ungido), idade calculada, endereço completo (tipo de logradouro, logradouro, número, complemento, comunidade, bairro, cidade, UF, CEP), contato (telefone, e-mail), contato de emergência (nome, telefone, e-mail) e grupo. Só o SEC e o master cadastram e editam.
- **CAD-007** **Perfis (designações) com vigência:** PUB (implícito), ancião, servo ministerial, pioneiro regular, pioneiro especial, missionário em campo, pioneiro enfermo (marca no pioneiro regular), SG (com o grupo), SEC, SS, CCA, CCP, CL e os perfis que os módulos futuros declararem. Cada um tem início e fim, e mudanças geram o recálculo de acesso (NUC-008, NUC-011).
- **CAD-008** **Validações de perfil:** ancião, servo ministerial, SG, SEC, SS, CCA, CCP e CL exigem sexo masculino e batismo. SEC, SS e CCA exigem ancião vigente. Cada um desses perfis únicos (SEC, SS, CCA, CCP, CL) tem só 1 titular vigente por vez, e cada grupo tem só 1 SG vigente.
- **CAD-009** **Grupos:** quantidade variável. Criar, renomear e desativar (só sem membros vigentes). Os grupos desativados continuam no histórico.
- **CAD-010** **Mudança de grupo por mês:** a mudança vale a partir de um mês (`AAAAMM`). Os relatórios desse mês em diante pertencem ao novo grupo; os anteriores continuam no grupo antigo (CAD-002).
- **CAD-011** **Duplicidade:** o cadastro avisa quando já existe alguém com o mesmo nome e a mesma data de nascimento.
- **CAD-012** **Situação cadastral** (ativo, mudou-se, removido, falecido) com data e observação. "Mudou-se" oferece gerar os S-21 para transferência (contrato com ATV). Ninguém é apagado: as situações só retiram das contagens (CAD-004).
