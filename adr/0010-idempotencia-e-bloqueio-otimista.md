# 6. Idempotência e Prevenção de Conflitos por Bloqueio Otimista

* **Status:** Aceito
* **Data:** 2026-10-20

---

## Contexto e Problema

A aplicação precisa proteger a integridade dos registros em dois cenários críticos:
1. **Envios Duplicados de Formulário:** Cliques múltiplos em botões de salvar lançamentos ou criar cenários podem gerar registros repetidos.
2. **Edições Concorrentes:** Dois usuários editando o mesmo lançamento financeiro ou configuração de custos fixos simultaneamente poderiam sobrescrever dados sem aviso prévio.

---

## Decisão

Implementar uma estratégia combinada de **Idempotência no envio** e **Bloqueio Otimista no controle de versão**:

1. **Idempotência (`Idempotency-Key`):**
   - Para toda operação de escrita (`POST /api/v1/records`, `POST /api/v1/scenarios`), o frontend gera um UUID único e o envia no cabeçalho HTTP `Idempotency-Key`.
   - Reenvios da mesma requisição pelo cliente não duplicam registros no banco de dados.

2. **Bloqueio Otimista (`version`):**
   - Todas as entidades mutáveis contêm um campo numérico de controle de versão (`version`).
   - Operações de edição (`PUT`) e exclusão (`DELETE`) devem enviar a versão do registro conhecido pelo cliente.
   - Caso a versão informada seja inferior à do banco de dados, o backend recusa a alteração com o status **HTTP 409 Conflict**, e o frontend orienta o usuário a recarregar a tela.

---

## Consequências

### Positivas
* **Zero Duplicação:** Previne lançamentos duplicados provocados por inconsistência de rede ou cliques múltiplos.
* **Integridade Transacional:** Evita que um usuário sobrescreva edições realizadas por outro sem seu conhecimento.

### Negativas / Desafios
* **Tratamento de Interface:** Exige que a UI informe adequadamente o usuário sobre a necessidade de recarregar a tela quando ocorre um conflito de versão (HTTP 409).