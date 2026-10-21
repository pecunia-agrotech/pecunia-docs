# 👥 Governança de Equipe e Papéis de IA — Pecunia AgroTech

Estrutura baseada no modelo de governança e papéis de squad para desenvolvimento assistido por IA (RERUM 2026).

## 1. Curador de Contexto & Tech Lead: Eduardo Maia
- **Responsabilidade:** Mantém os ativos do processo (`AGENTS.md`, ADRs, catálogo de prompts, regras de produção da Base de Conhecimento e governança dos modelos de IA).
- **Escopo:** Concepção da solução, Clean Architecture em Python, motor determinístico e orquestração.

## 2. Piloto Frontend & Revisor de Backend: Alonso
- **Modo Piloto:** Conduz os agentes na especificação e implementação do frontend (Next.js, Tailwind, Recharts, Shadcn UI).
- **Modo Revisor:** Revisa pull requests e código do backend em contexto limpo.

## 3. Piloto de QA/Domínio & Revisor de Frontend: Lucas
- **Modo Piloto:** Conduz os agentes na criação da suíte de testes automáticos (`pytest`), verificação própria e validação zootécnica.
- **Modo Revisor:** Revisa pull requests do frontend em contexto limpo contra as SPECs de aceitação.