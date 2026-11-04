# Questões em aberto (rodada 2)

- ✅ **Q-01 Ajudante:** quais poderes o ajudante tem? Por exemplo: ver relatórios do grupo e conciliar, mas não aprovar nem fechar?
- ✅ **Q-02 Identidade sem senha:** como o PWA não tem senha, quem tiver o link do grupo pode enviar um relatório com qualquer nome. A conciliação pelo SG basta como controle? Ou cada publicador recebe o próprio link/QR de ativação depois de cadastrado?
- ✅ **Q-03 Troca de celular:** quando o publicador troca de aparelho, o SG refaz a conciliação?
- ✅ **Q-04 Faixas da apuração anual:**
  - campo ≥ 600 → continua;
  - 560 ≤ campo < 600 → continua, sem análise;
  - campo < 560, mas campo + crédito ≥ 560 → análise da CS;
  - campo + crédito < 560 → cancelamento sugerido.
  É isso? E como fica quem começou a ser pioneiro no meio do ano (meta proporcional)?
- ✅ **Q-05 Mês do relatório tardio:** o tardio aceito também conta para o status (regular/irregular) do mês original? A sugestão é que sim.
- ✅ **Q-06 Reabertura pelo secretário:** reabrir o mês da congregação reabre também os grupos?
- ✅ **Q-07 Domínio:** usar um domínio próprio ou um subdomínio grátis (`*.pages.dev`)?
- ✅ **Q-08 S-205:** quando o modelo for enviado, revisar os campos da petição.
- **Q-09 Formato da importação do AS 2026:** enviar um exemplo (anonimizado) do CSV do Google Forms.

## Decisões
- Q-01: o ajudante não tem nenhum poder; é só um registro.
- Q-02 e Q-03: ativação por QR individual (NUC-001); troca de aparelho = novo QR.
- Q-04: faixas confirmadas; meta proporcional (ATV-032 e ATV-035).
- Infraestrutura: GCP + Firebase (ADR-001 aceito).
- Q-05: o tardio aceito conta para o mês original (status e S-21), com data registrada, e entra no próximo relatório da congregação. O mês já fechado não muda (ATV-021).
- Q-06: a reabertura da congregação vale só para o secretário; os grupos usam a liberação temporária (ATV-020).
- Q-07: domínio próprio só na produção; até lá, *.web.app.
- Q-08: S-205b analisado (ATV-022).

## Rodada 3
- ✅ **Q-10 Bloqueio do dia 7:** a interpretação do ATV-003 está correta? Ou, a partir do dia 7, **todos** os relatórios do mês (inclusive os enviados no prazo) passam a exigir aprovação explícita do SG?
- ✅ **Q-11 S-21 de 2026:** os PDFs foram preenchidos **digitalmente** (com campos) ou são **digitalizações de papel**? Digitalização exige OCR e revisão manual.
- ✅ **Q-12 Petição em papel:** anexar a digitalização da petição em papel? Isso usa o Cloud Storage em São Paulo, com custo de centavos.
- ✅ **Q-13 Atraso após o fechamento do grupo:** um envio em atraso que chega entre os dias 16 e 31, com o grupo já fechado: o SG aprova sem precisar de liberação temporária? E se o mês já estiver fechado na congregação (depois do dia 20), entra como tardio no próximo relatório?
- Decisões da rodada 3: Q-10 → ATV-002 e ATV-003 (aprovação só a partir do dia 7; os não identificados aguardam conciliação). Q-12 → ATV-027 (anexo obrigatório). Q-13 → ATV-017 (fila para o próximo ciclo).
- ✅ **Q-14 Retroativo nos totais:** na 1ª rodada você disse que o retroativo aceito "deverá constar no próximo relatório da congregação". Agora ficou "somente no S-21". Ele entra nos totais do próximo relatório da congregação aberto, ou fica **apenas** no S-21?
- Q-11: os S-21 de 2026 são PDFs editáveis preenchidos digitalmente. A extração é direta pelos campos, sem OCR (ATV-043).
- Q-14: o retroativo vai para o S-21 do mês original e entra nos totais do próximo relatório da congregação ainda aberto (ATV-021).

## Rodada 4 (ADR-002)
- ✅ **Q-15 Envio offline:** o publicador toca em "Enviar" no dia 6, sem internet, e o celular só sincroniza no dia 8. O envio vale pela hora do **servidor** (fica em atraso) ou pela do **aparelho** (fica no prazo)? A hora do aparelho pode ser alterada pelo usuário. Sugestão: servidor, com uma tolerância ⚙ de até 24h quando o aparelho registrou o envio antes do prazo, e a decisão final do SG.
- ✅ **Q-16 Conflito de interesse:** o SG é publicador do próprio grupo. Quem aprova o relatório **em atraso, retroativo ou reenvio do próprio SG**: ele mesmo, o secretário ou outro ancião? E quando o secretário é o SG?
- ✅ **Q-17 Histórico de quem entrou no grupo:** o SG do Grupo 4 deve ver os relatórios de Claudia de quando ela estava no Grupo 2? Pela regra atual, não: esses meses continuam pertencendo ao Grupo 2.
- Q-15: hora do servidor + tolerância de 24h com a marca "offline no prazo"; o SG decide (ATV-010).
- Q-16: o SG aprova os próprios envios (ATV-018). Q-17: não vê os relatórios de outro grupo, só o histórico de status (CAD-002).

## Rodada 5 (UX do secretário)
- ✅ **Q-18 Fechar a congregação com grupo aberto:** o secretário pode fechar a congregação no dia 20 se algum grupo ainda não estiver fechado? Pela regra, o fechamento automático do dia 16 deveria ter fechado todos, mas um grupo pode estar sob liberação temporária.
- ✅ **Q-19 CS no sistema:** a CS aprova as petições na própria tela (cada membro registra a aprovação) ou o secretário registra a aprovação em nome dela?
- ✅ **Q-20 S-21 em lote:** gerar todos os S-21 do ano de serviço num ZIP é útil? Para quando (fim do AS, transferência, sob demanda)?
- Rodada 5: Q-18 → ATV-019a; Q-19 → ATV-022a, ATV-027a, CAD-005; Q-20 → ATV-041a. Linguagem: TypeScript (ADR-001). Protótipos: 100% responsivos.

## Rodada 6 (ADR-003 e UX)
- ✅ **Q-21 Paleta:** fixar uma paleta como identidade do sistema ou deixar cada pessoa escolher a sua em Ajuda › Aparência?
- ✅ **Q-22 Atribuição manual:** além das designações (coordenador, secretário, SS, SG), o secretário pode dar um papel a alguém sem designação formal (ex.: um ancião para ajudar a lançar o S-3)? Com prazo de validade?
- ✅ **Q-23 Anciãos e servos no sistema:** os anciãos que não são da CS nem SG precisam de algum acesso no MVP (ex.: consulta)?
- Q-21: cada pessoa escolhe a sua paleta (UX). Q-22: concessão manual pelo administrador, pelo CCA e pelo SEC, com validade opcional (ADR-003 §6).
- ✅ **Q-24 Master e dados sensíveis:** a conta de administrador do sistema deve ter acesso aos dados de Pastoreio e Contas, ou só à parte técnica (parâmetros, papéis, backups), sem ver o conteúdo sensível?
- ✅ **Q-25 "Ao mudar de papel":** a desconexão em tempo real vale quando os poderes do usuário **são alterados** (ADR-003 §8), ou também quando ele mesmo **troca o papel ativo** na tela (ex.: de Publicador para SG)? A troca na tela não precisaria desconectar.
- Q-23: os anciãos não têm acesso especial no MVP; os papéis são concedidos conforme as funcionalidades forem desenvolvidas e conforme a necessidade (ADR-003 §6).
- Q-24: o master vê tudo, com termo de confidencialidade obrigatório (ADR-003 §7).
- Q-25: desconecta só ao perder poder; ganhar poder ou trocar o papel na tela não desconecta (ADR-003 §8).
- ✅ **Q-26 Revogação do consentimento:** ver specs/03-modulos/nucleo/spec.md (H-NUC-08).
- Q-26: opção (a) (H-NUC-08 CA-2).
- **Q-27 a Q-30:** ver specs/03-modulos/cadastro/spec.md.
