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
- ✅ **Q-10 Bloqueio do dia 7:** a interpretação do RN-003a está correta? Ou, a partir do dia 7, **todos** os relatórios do mês (inclusive os enviados no prazo) passam a exigir aprovação explícita do SG?
- ✅ **Q-11 S-21 de 2026:** os PDFs foram preenchidos **digitalmente** (com campos) ou são **digitalizações de papel**? Digitalização exige OCR e revisão manual.
- ✅ **Q-12 Petição em papel:** anexar a digitalização da petição em papel? Isso usa o Cloud Storage em São Paulo, com custo de centavos.
- ✅ **Q-13 Atraso após o fechamento do grupo:** um envio em atraso que chega entre os dias 16 e 31, com o grupo já fechado: o SG aprova sem precisar de liberação temporária? E se o mês já estiver fechado na congregação (depois do dia 20), entra como tardio no próximo relatório?
- Decisões da rodada 3: Q-10 → RN-003 e RN-003a (aprovação só a partir do dia 7; os não identificados aguardam conciliação). Q-12 → RN-046 (anexo obrigatório). Q-13 → RN-024 (fila para o próximo ciclo).
- ✅ **Q-14 Retroativo nos totais:** na 1ª rodada você disse que o retroativo aceito "deverá constar no próximo relatório da congregação". Agora ficou "somente no S-21". Ele entra nos totais do próximo relatório da congregação aberto, ou fica **apenas** no S-21?
- Q-11: os S-21 de 2026 são PDFs editáveis preenchidos digitalmente. A extração é direta pelos campos, sem OCR (RN-091).
- Q-14: o retroativo vai para o S-21 do mês original e entra nos totais do próximo relatório da congregação ainda aberto (RN-032).

## Rodada 4 (ADR-002)
- **Q-15 Envio offline:** o publicador toca em "Enviar" no dia 6, sem internet, e o celular só sincroniza no dia 8. O envio vale pela hora do **servidor** (fica em atraso) ou pela do **aparelho** (fica no prazo)? A hora do aparelho pode ser alterada pelo usuário. Sugestão: servidor, com uma tolerância ⚙ de até 24h quando o aparelho registrou o envio antes do prazo, e a decisão final do SG.
- **Q-16 Conflito de interesse:** o SG é publicador do próprio grupo. Quem aprova o relatório **em atraso, retroativo ou reenvio do próprio SG**: ele mesmo, o secretário ou outro ancião? E quando o secretário é o SG?
