# Identidade visual e padrões de UI

Decisões de 04/11/2026.

- **Componentes:** shadcn/ui (Radix) + Tailwind CSS. Os tokens de cor e tipografia ficam centralizados num tema único, compartilhado por todos os módulos.
- **Visual suave:** fundos claros levemente azulados, bordas muito leves, sombras sutis, cantos arredondados (14 px), cores de estado em tons pastel e azul acinzentado como cor de destaque. Paleta discreta, tipografia legível e sem ornamentos. **Sem marcas ou logotipos oficiais**, a não ser dentro dos próprios formulários (S-4, S-205b, S-21, S-3) reproduzidos dos PDFs.
- **Acessibilidade:** WCAG 2.2 AA obrigatório. Contraste ≥ 4,5:1, foco visível, alvos de toque ≥ 44 px (56 px nos controles do publicador), rótulos ARIA, navegação por teclado. Verificação automática com axe-core no CI.
- **Responsivo:** a tela do publicador é pensada primeiro para o celular. **Todas as telas são 100% responsivas** (celular, tablet e computador), inclusive as do secretário.
- **Idioma e formatos:** pt-BR, datas dd/mm/aaaa, fuso America/Sao_Paulo.
- **Opções de aparência** (fonte grande, alto contraste, modo escuro): desativadas por padrão, valem para todos os perfis (UX-PUB-003).
