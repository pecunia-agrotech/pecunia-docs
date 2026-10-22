# ADR 003: Persistência Local, Concorrência e Idempotência com SQLite

* **Status:** Aceito
* **Data:** 2026-10-20

## Contexto
O projeto exige operação 100% local, sem dependência de serviços externos (como PostgreSQL ou Redis) durante a execução do produtor[cite: 28, 30]. No entanto, a aplicação necessita de suporte resiliente a leituras/escritas concorrentes, prevenção de condições de corrida (*race conditions*) e tratamento de edições simultâneas[cite: 28].

## Decisão
1. **Motor de Banco:** Adotar SQLite configurado com *Write-Ahead Logging* (WAL), `foreign_keys = ON` e `busy_timeout = 15000 ms`[cite: 28].
2. **Estratégia de Transações:** Leituras usam *snapshots* transacionais isolados; escritas utilizam `BEGIN IMMEDIATE` para evitar bloqueios de escrita concorrentes[cite: 28].
3. **Lock Otimista:** Atualizações e exclusões exigem verificação de versão (`version`). Conflitos de edição retornam HTTP 409[cite: 28].
4. **Idempotência:** Criação de lançamentos e cenários utilizam chave de idempotência com hash do *payload*[cite: 28].

## Consequências
* **Positivas:** Execução offline simples (zero infraestrutura externa), com alta integridade transacional e prevenção contra sobrescrita de dados[cite: 28, 30].
* **Negativas:** Requer tratamento explícito de exceções de concorrência e rascunhos de edição na camada de interface do frontend[cite: 28].