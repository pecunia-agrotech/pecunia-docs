# 3. Contratos e Tipagem Estática Derivados do OpenAPI

* **Status:** Aceito
* **Data:** 2026-10-20

---

## Contexto e Problema

Integrar sistemas desenvolvidos em tecnologias diferentes (Python/FastAPI no backend e TypeScript/React no frontend) frequentemente acarreta divergências nos nomes dos campos, tipos de dados incompatíveis e erros em tempo de execução quando a API sofre modificações.

---

## Decisão

Utilizar a especificação **OpenAPI 3.1** gerada automaticamente pelo FastAPI como contrato oficial entre as duas camadas e automatizar a geração dos tipos TypeScript.

1. O backend expõe o esquema do sistema em `backend/openapi.json`.
2. O frontend possui um script (`npm run build:contracts`) que consome este esquema e gera o arquivo `src/core/types/api.generated.ts`.
3. Todos os DTOs de requisição e resposta consumidos pelo cliente de API (`src/core/services/api.ts`) devem obrigatoriamente importar e utilizar as definições geradas.

---

## Consequências

### Positivas
* **Detecção Precoce de Quebras (*Build-Time Failure*):** Se o backend alterar a estrutura de um DTO, o comando `npm run typecheck` ou o processo de compilação do frontend falhará imediatamente.
* **Autocomplete e Produtividade:** Desenvolvedores contam com sugestão autocompletada de todos os campos da API no ambiente de desenvolvimento (IDE).

### Negativas / Desafios
* **Passo Obrigatório no Build:** Exige a execução do processo de sincronização de contratos sempre que uma rota do backend for criada ou modificada.