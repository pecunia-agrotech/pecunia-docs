# 7. Estratégia de Isolamento de Contextos e Cenários de Simulação

* **Status:** Aceito
* **Data:** 2026-10-20

---

## Contexto e Problema

Os produtores rurais necessitam simular cenários hipotéticos (ex.: "E se o preço da ração aumentar em 15%?" ou "E se a produção de leite subir 200 litros/dia?") sem arriscar alterar, poluir ou corromper os dados financeiros reais da fazenda.

---

## Decisão

Adotar o conceito de **Contextos/Cenários Isolados** no nível de persistência e expor o alternador no frontend.

1. **Classificação de Contextos:**
   - `real`: O contexto oficial da fazenda com dados reais informados pelo produtor.
   - `demo`: Um ambiente pré-populado seguro para exploração das funcionalidades do sistema e treinamentos.
   - `<UUID-do-Cenário>`: Cenários de simulação criados a partir da cópia dos dados de um mês específico.

2. **Propagação de Contexto no Frontend:**
   - Todas as chamadas de busca (`GET`) e alteração (`POST`, `PUT`, `DELETE`) enviam o parâmetro `scenario_id` selecionado.
   - A interface altera a cor e o badge visual do topo da aplicação quando o usuário está navegando fora do contexto `real`.

---

## Consequências

### Positivas
* **Simulação Segura:** Permite que o produtor faça experimentos e testes de sensibilidade econômica de forma isolada.
* **Clareza Visual:** O indicador de contexto no topo previne que o usuário confunda um cenário hipotético com os lançamentos oficiais da fazenda.

### Negativas / Desafios
* **Gerenciamento de Estado:** Todas as consultas da aplicação devem obrigatoriamente repassar o parâmetro de contexto para não misturar dados reais com simulados.