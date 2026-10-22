# 4. Gerenciamento de Estado de Escopo Global via Context API (`FarmScopeContext`)

* **Status:** Aceito
* **Data:** 2026-10-20

---

## Contexto e Problema

Diversos componentes da interface (como Dashboard, Calendário, Extrato Diário, Diagnóstico e Histórico) dependem simultaneamente de três parâmetros fundamentais:
- O mês ativado para consulta (ex.: `2026-10`);
- A data específica selecionada no calendário (ex.: `2026-10-22`);
- O contexto/cenário de simulação ativo (`real`, `demo` ou o UUID de um cenário hipotético).

Repassar esses valores via *prop drilling* por múltiplos níveis de componentes tornaria a base de código complexa e frágil.

---

## Decisão

Criar o provedor de estado centralizado **`FarmScopeContext`** (`src/core/use-cases/farm-scope.tsx`), utilizando a Context API do React.

O contexto provê globalmente para a aplicação:
1. `month`: Mês ativo no formato `YYYY-MM`.
2. `date`: Data ativa no formato `YYYY-MM-DD`.
3. `scenario_id`: ID do contexto/cenário atual.
4. Métodos para alterar o mês, data, cenário ativo e disparar a recarga global de dados.

---

## Consequências

### Positivas
* **Navegação Sincronizada:** Qualquer alteração no seletor de mês ou cenário atualiza instantaneamente todos os componentes da tela ativa.
* **Simplicidade:** Elimina a necessidade de instalar bibliotecas complexas de gerenciamento de estado como Redux ou Zustand.

### Negativas / Desafios
* **Re-renderizações:** Componentes consumidores do contexto são renderizados novamente quando o mês ou cenário é alterado (mitigado pelo isolamento dos componentes visuais).