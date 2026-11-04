# Regras — Parâmetros do Sistema (PAR)

Os valores marcados com `⚙` vêm de Parâmetros (PAR-001). O ID antigo aparece entre parênteses.

- **PAR-001** _(RN-090)_ Parâmetros versionados por vigência e editáveis só pelo **secretário** e pelo **master**.
- **PAR-002** _(RN-044)_ Os meses especiais são marcados pelo secretário em uma agenda, à medida que forem anunciados.
- **PAR-003** **Catálogo declarado pelos módulos:** cada módulo declara no manifesto os parâmetros dele (chave, descrição, tipo, unidade, mínimo/máximo, valor padrão). Nenhum valor de regra fica fixo no código.
- **PAR-004** **Vigência por mês:** cada alteração vale a partir de um mês (`AAAAMM`) igual ou posterior ao mês aberto mais antigo. Um mês já fechado na congregação continua calculado com os valores vigentes na época. Não existe alteração retroativa.
- **PAR-005** **Alteração auditada:** toda alteração exige motivo e mostra o antes, o depois e a vigência. A versão anterior nunca é apagada.
- **PAR-006** **Agenda travada no fechamento:** um mês especial não pode ser marcado ou desmarcado depois que aquele mês foi fechado na congregação.
- **PAR-007** **Leitura rápida:** os valores vigentes são carregados na inicialização do app e das funções, e atualizados quando uma nova versão é publicada (sem esperar reinício).
