# 2. Escolha da Stack Frontend (Next.js 16, React 19, Tailwind CSS e Recharts)

* **Status:** Aceito
* **Data:** 2026-10-20

---

## Contexto e Problema

A interface do Pecunia AgroTech precisa ser moderna, acessível, responsiva e de fácil manutenção por equipes de engenharia. Era necessário selecionar um ecossistema robusto para gerenciamento de páginas, navegação fluida, visualização gráfica de dados financeiros e componentes visuais acessíveis.

---

## Decisão

Adotar a seguinte stack tecnológica para a construção da interface do usuário:

1. **Next.js 16.4.2 (App Router):** Utilizado como framework base para estruturação das rotas e otimização do empacotamento da aplicação.
2. **React 19 & TypeScript:** Para desenvolvimento baseado em componentes reutilizáveis e garantia de tipagem estática do código.
3. **Tailwind CSS & Radix UI / Shadcn UI:** Para estilização utilitária rápida e componentes de interface acessíveis (diálogos, formulários, abas e seletores).
4. **Recharts:** Biblioteca de gráficos baseada em React para renderização dos gráficos de evolução diária e composição de custos.
5. **React Hook Form & Zod:** Para validação e gestão do estado dos formulários no lado do cliente.

---

## Consequências

### Positivas
* **Produtividade Acelerada:** O uso de componentes utilitários e primitivos de UI permitiu a criação rápida de telas padronizadas.
* **Tipagem Segura:** O TypeScript reduz erros em tempo de desenvolvimento.
* **Rich Graphing:** A biblioteca Recharts integra-se nativamente com a árvore do React.

### Negativas / Desafios
* **Tamanho do Bundle:** O uso de bibliotecas de componentes e gráficos exige monitoramento contínuo para evitar o inchaço do pacote final de produção.