# 1. Separação Rígida de Responsabilidades (Frontend Passivo vs. Backend Autônomo)

* **Status:** Aceito
* **Data:** 2026-10-20

---

## Contexto e Problema

O **Pecunia AgroTech** é um sistema de gestão econômica e financeira para a pecuária leiteira. A aplicação lida com regras de negócio críticas, tais como:
- Cálculo da Demonstração do Resultado do Exercício (DRE);
- Métricas zootécnicas e financeiras (IOFC, Custo por Litro, Equilíbrio Estático, Relação de Troca);
- Motor de inferência determinístico baseado em heurísticas para diagnósticos.

A duplicação ou divisão dessas regras entre o frontend e o backend poderia gerar inconsistências nos dados, divergência de valores exibidos e problemas de manutenção futura.

---

## Decisão

Fica estabelecido que o **frontend atuará estritamente como uma camada de apresentação e gerenciamento de estado da interface de usuário**. 

1. **Responsabilidades do Frontend:**
   - Capturar dados fornecidos pelo usuário através de formulários e validar apenas o formato dos campos (ex.: campos obrigatórios, valores numéricos maiores que zero).
   - Enviar requisições estruturadas para o backend FastAPI via HTTP REST.
   - Renderizar de forma clara e responsiva os indicadores, tabelas, gráficos e relatórios retornados pelo backend.
   - Gerenciar navegação, datas ativas e alternância de cenários.

2. **Responsabilidades Exclusivas do Backend:**
   - Realizar todos os cálculos financeiros, conversões de valores e fecho DRE.
   - Executar o motor de inferência heurístico e gerar o relatório em Markdown.
   - Garantir a persistência e integridade das transações.

---

## Consequências

### Positivas
* **Fonte Única da Verdade (*Single Source of Truth*):** Garantia de que qualquer cliente que consuma a API obterá rigorosamente os mesmos resultados.
* **Segurança e Confiabilidade:** Impossibilidade de manipulação dos resultados financeiros no lado do cliente.
* **Facilidade de Manutenção:** Alterações nas regras de cálculo ou novas heurísticas no motor de diagnóstico exigem atualização apenas na camada do backend.

### Negativas / Desafios
* **Dependência de Rede:** Qualquer alteração nos dados necessita de uma chamada de rede para recalcular os totais e indicadores do mês.
* **Necessidade de Otimização de Resposta:** A API backend precisa responder rapidamente para não comprometer a fluidez da experiência do usuário (UX).