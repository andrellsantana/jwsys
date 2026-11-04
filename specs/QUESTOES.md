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
- Q-02 e Q-03: ativação por QR individual (RN-002); troca de aparelho = novo QR.
- Q-04: faixas confirmadas; meta proporcional (RN-054 e RN-057).
- Infraestrutura: GCP + Firebase (ADR-001 aceito).
- Q-05: o tardio aceito conta para o mês original (status e S-21), com data registrada, e entra no próximo relatório da congregação. O mês já fechado não muda (RN-032).
- Q-06: a reabertura da congregação vale só para o secretário; os grupos usam a liberação temporária (RN-031).
- Q-07: domínio próprio só na produção; até lá, *.web.app.
- Q-08: S-205b analisado (RN-040).

## Rodada 3
- **Q-10 Bloqueio do dia 7:** a interpretação do RN-003a está correta? Ou, a partir do dia 7, **todos** os relatórios do mês (inclusive os enviados no prazo) passam a exigir aprovação explícita do SG?
- **Q-11 S-21 de 2026:** os PDFs foram preenchidos **digitalmente** (com campos) ou são **digitalizações de papel**? Digitalização exige OCR e revisão manual.
- **Q-12 Petição em papel:** anexar a digitalização da petição em papel? Isso usa o Cloud Storage em São Paulo, com custo de centavos.
