# 🛠️ Dicionário de Engenharia Assistida por IA (Workflow de Desenvolvimento)

> **Nota de Arquitetura & Governança:** As definições e ferramentas listadas neste documento referem-se EXCLUSIVAMENTE ao **processo de desenvolvimento, pesquisa técnica e geração de código** utilizado pela equipe. A aplicação **Pecunia AgroTech** em tempo de execução é 100% offline, local e determinística, não realizando qualquer chamada a APIs ou serviços externos de IA.

---

### 1. Ferramentas de Suporte ao Desenvolvimento (DevTools & Pesquisa)

* **Gemini / Gemini Flash:** Modelos de linguagem utilizados na **Curadoria de Contexto** inicial, estruturação das especificações técnicas (SPECs) do domínio leiteiro e auxílio no desenho dos DTOs e Clean Architecture.
* **Codex:** Assistente de codificação utilizado no ambiente de desenvolvimento para suporte na escrita de boilerplate, refatorações pontuais e geração primária de rotinas de teste.
* **Assistentes de Pesquisa IA (Google Chrome AI / Brave AI Chat):** Ferramentas de apoio para consulta rápida de documentação técnica (FastAPI, Next.js, Pytest, Vitest), resolução de erros de sintaxe e verificação de boas práticas no navegador.

---

### 2. Conceitos do Processo de Engenharia (Workflow)

* **Piloto (Desenvolvedor):** Papel ativo do engenheiro de software responsável por orquestrar as ferramentas, definir regras de arquitetura, validar tipos/contratos e aprovar commits.
* **Curador de Contexto:** Processo de seleção e refino das regras de negócio da pecuária leiteira fornecidas aos modelos para garantir respostas precisas e alinhadas aos requisitos do hackathon.
* **SPEC (Especificação Técnica):** Documento ou instrução com critérios de aceite executáveis que guiaram a geração de código e suítes de testes.
* **Harness:** Estrutura local de scripts (`setup.ps1`, `setup.sh`, `run_local.py`) e testes automatizados que validam e garantem a integridade do código gerado.

---

### 3. Runtime da Aplicação (Motor Especialista Exclusivamente Local)

* **Trace (Traço de Inferência):** Caminho lógico e auditável percorrido pelo **Motor de Inferência local (Regras R01–R07)** ao avaliar a Base de Conhecimento em SQLite. Funciona sem algoritmos estocásticos ou chamadas de rede.
