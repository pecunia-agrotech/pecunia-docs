# ADR 002: Precisão Financeira em Centavos e Manipulação de Decimais

* **Status:** Aceito
* **Data:** 2026-10-20

## Contexto
Cálculos de demonstrativos financeiros e DRE utilizando tipos de ponto flutuante nativos (`float`) acumulam erros de arredondamento inerentes à representação binária (IEEE-754). Em um sistema de análise econômica e financeira da pecuária leiteira, divergências de centavos comprometem a confiabilidade do fechamento gerencial e da DRE.

## Decisão
1. **Armazenamento:** Todos os valores monetários são armazenados e trafegados no backend como números inteiros representando centavos (R$ 10,50 = 1050 centavos). Volumes de leite são armazenados como inteiros em mililitros[cite: 28].
2. **Cálculos:** As operações de conversão e cálculo de indicadores usam o módulo `Decimal` do Python com a regra de arredondamento explícita `ROUND_HALF_UP`[cite: 28].
3. **Papel do Frontend:** O frontend não realiza cálculos ou somatórios oficiais; limita-se a formatar visualmente os valores recebidos dos DTOs[cite: 28].

## Consequências
* **Positivas:** Eliminação completa de erros de arredondamento binário, consistência determinística nos relatórios e conciliação exata do DRE diário e mensal[cite: 28].
* **Negativas:** Exige conversões explícitas para exibição no frontend e validação rigorosa de DTOs nas rotas da API[cite: 28].