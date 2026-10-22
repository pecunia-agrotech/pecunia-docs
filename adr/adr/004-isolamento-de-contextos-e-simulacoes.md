# ADR 004: Isolamento de Contextos entre Histórico Real, Demonstração e Cenários

* **Status:** Aceito
* **Data:** 2026-10-20

## Contexto
O produtor rural precisa realizar simulações financeiras ("E se o preço da ração subir?") e navegar por dados de demonstração sem colocar em risco a integridade contábil do histórico real da fazenda[cite: 28, 30].

## Decisão
1. **Isolamento de Escopo:** Separação estrita no banco de dados entre os contextos `REAL`, `DEMO` e `SCENARIO`[cite: 28, 30]. Os dados desses contextos nunca se somam ou se misturam em relatórios ou indicadores[cite: 28].
2. **Criação de Cenários:** Novas simulações são geradas por cópia e isolamento a partir de um mês do histórico real[cite: 28].
3. **Sem Promoção Automática:** Alterações efetuadas em cenários ou demonstrações nunca são promovidas para o histórico real de forma automática[cite: 28].

## Consequências
* **Positivas:** Garantia total de segurança dos dados reais do produtor, permitindo liberdade em simulações e auditorias[cite: 28, 30].
* **Negativas:** Necessidade de passar explicitamente o escopo de contexto em todas as consultas e DTOs da API[cite: 28].