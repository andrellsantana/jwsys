# Constituição do JWSys

Princípios que valem para todos os módulos. Uma mudança aqui exige um ADR.

1. **Spec primeiro.** Nenhum código é escrito sem uma `spec.md` aprovada. Cada regra tem um ID (`RN-xxx`), e cada critério de aceite vira um teste automatizado.
2. **Simplicidade para o publicador.** O envio do S-4 precisa funcionar para idosos e leigos: sem senha, poucas telas e linguagem do dia a dia.
3. **Privacidade por padrão (LGPD).** Os dados são pessoais e sensíveis (art. 11, convicção religiosa). A controladora é a congregação. Os dados ficam armazenados no Brasil. Regras:
   - consentimento registrado, com versão do termo e data;
   - menor privilégio, com isolamento por grupo garantido no banco (RLS);
   - trilha de auditoria;
   - retenção parametrizável.
4. **Nada de regra fixa no código.** Metas de horas, prazos, limites e flags ficam na tabela de **Parâmetros**, versionados por ano de serviço e carregados na inicialização.
5. **Histórico imutável.** Envios, reenvios, aprovações e fechamentos nunca são sobrescritos: cada mudança gera um novo registro, com autor, data e motivo.
6. **Offline-first no PWA.** O rascunho fica salvo no aparelho, e o envio vai para uma fila que é transmitida quando a conexão voltar.
7. **Custo zero como meta e R$ 25/mês como teto.** Toda escolha de infraestrutura passa por um ADR com uma seção de FinOps.
8. **Somente pt-BR**, com fuso horário `America/Sao_Paulo` em todos os prazos.
9. **Um mantenedor.** Uma única linguagem (TypeScript) no sistema inteiro, poucas peças móveis e serviços gerenciados.
