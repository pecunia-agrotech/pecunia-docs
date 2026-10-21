# ADR 001: Adoção de Motor Especialista Determinístico sem Dependência de LLM na Decisão

- **Status:** Aprovado
- **Data:** 2026-10-20
- **Contexto:** Necessidade de garantir 100% de precisão matemática nos cálculos zootécnicos e financeiros sem risco de alucinação em ambiente offline/Localhost.
- **Decisão:** O núcleo de decisão é executado por um Motor de Inferência determinístico $O(1)$ em Python, enquanto a IA atua na curadoria de contexto e NLU.
- **Consequências:** Isenção total de erros de cálculo, tempo de execução < 1ms e suporte total a funcionamento 100% offline.