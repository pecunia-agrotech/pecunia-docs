# Arquitetura e decisões

## Responsabilidades

`backend/main.py` é o composition root HTTP: injeta repositório e serviços, valida origens locais e transforma exceções em mensagens PT-BR. `src/domain` contém entidades, objetos de valor, contratos abstratos, fatos e inferência determinística. Entidades e motor não importam FastAPI nem SQLite. Os DTOs Pydantic ficam em módulo separado das entidades. `src/application` coordena persistência, análises e exportações. `src/infrastructure` implementa SQLite e migração.

O frontend organiza componentes por funcionalidade. `core/services/api.ts` centraliza HTTP. Tipos são gerados de OpenAPI em `core/types/api.generated.ts`. `useResource` cancela requisições antigas e impede respostas de outro período/contexto de substituir o estado atual. `farm-scope` compartilha mês, dia, contexto e proteção de rascunhos. O navegador apenas formata os valores financeiros recebidos.

SQLite usa WAL, foreign keys e busy_timeout de 15000 ms. Leituras compostas usam snapshot transacional; escritas usam BEGIN IMMEDIATE. Atualizações e exclusões exigem versão; conflitos retornam 409. Criação de lançamentos e cenários tem chave de idempotência com hash do payload. Repetir chave com conteúdo diferente é rejeitado. A interface preserva rascunhos e pede revisão antes de reaplicar uma edição conflitante.

Valores monetários são inteiros em centavos; volume é inteiro em mililitros. Decimal e ROUND_HALF_UP controlam conversões e arredondamento. O frontend não armazena totais oficiais.

## Fórmulas

- Receita bruta = receita de leite + outras receitas.
- Custos variáveis = alimentação + outras despesas.
- Resultado gerencial = receita bruta − variáveis − fixos alocados.
- IOFC = receita de leite − alimentação; IOFC/vaca = IOFC ÷ vacas em lactação.
- Preço médio = receita de leite ÷ litros comercializados.
- Participação alimentar = alimentação ÷ receita de leite × 100.
- Custo por litro = custos totais ÷ litros.
- Equilíbrio estático = máximo(0, custos totais − outras receitas) ÷ preço médio.
- Relação de troca = preço médio do leite (R$/L) ÷ preço de referência do insumo (R$/kg), em kg/L.

Cada indicador verifica suas dependências. Receita de leite sem volume completo invalida os indicadores dependentes de litros. Alimentação não lançada não é presumida zero para IOFC; um valor zero confirmado de fixos difere de configuração ausente.

## Decisões provisórias isoladas

1. **Rateio:** o mês completo é dividido pelos dias corridos efetivos, inclusive futuros ou sem lançamentos. O quociente inteiro em centavos é distribuído por dia, com resíduos nos últimos dias; a soma diária reconcilia exatamente o mensal. A referência mensal ÷ 30 é somente informativa. Meses incompletos não são projeções de faturamento.
2. **Custos:** todos os custos informados são atribuídos ao leite. COE compreende variáveis e fixos desembolsáveis; COT acrescenta a depreciação informada. Não se estima remuneração familiar ou capital não informado. Evitar dupla contagem de combustível nos fixos e nos lançamentos.
3. **Equilíbrio:** é estático; mantém custos constantes, abate outras receitas e não prevê resposta biológica. Não é modelo de margem de contribuição marginal.
4. **Troca:** adota leite/insumo em kg/L; não confundir com conversão alimentar ou um índice USDA.
5. **Histórico:** parâmetros são mensais e independentes por contexto. Alterar o número de vacas do mês recalcula os indicadores daquele mês. Não há inventário diário de animais.
6. **Demonstração:** dados simulados são persistidos num contexto próprio, não misturados ao real. Não existe promoção automática de cenários para o real.

Essas interpretações precisam de validação com a organização do projeto antes de uso contábil ou recomendações comerciais. Estão concentradas em `working_memory.py`, `value_objects.py` e nos serviços.

## Inferência e explicabilidade

Todas as regras são avaliadas em ordem determinística; prevalece a maior gravidade. O relatório preserva também as regras não ativadas e os fatos utilizados.

| Regra | Condição | Estado | CF |
|---|---|---|---|
| R01 | Base incompleta, volume/receita ausentes, alimentação ausente ou configuração incompleta | Atenção | 0,99 |
| R02 | Receita de leite positiva e IOFC < 0 | Crítico | 0,99 |
| R03 | Resultado gerencial < 0 | Crítico | 0,98 |
| R04 | Litros < equilíbrio estático disponível | Crítico | 0,95 |
| R05 | Participação alimentar > 60% | Atenção | 0,80 |
| R06 | Relação de troca < 1 | Atenção | 0,75 |
| R07 | Receita de leite positiva e IOFC exatamente zero | Atenção | 0,85 |

CF é peso heurístico do sistema, não probabilidade, previsão nem validação científica dos limiares. R05 e R06 são critérios do projeto, não parâmetros universais de produção. COMO, PORQUE, evidências, fórmulas, pesos, recomendações e limitações são produzidos no backend. Referências conceituais identificadas no relatório não constituem fontes dos pesos heurísticos.

## Segurança local

Rotas têm prefixo `/api/v1`. Respostas financeiras usam DTOs tipados e erros localizados. Escritas via navegador verificam Origin; CORS permite loopback configurado. Não há autenticação: uso restrito ao computador local. CSV tem BOM UTF-8, delimitador ponto e vírgula e sanitização de fórmulas nos campos textuais, sem alterar valores numéricos negativos. Markdown não executa HTML. Exportações usam o mesmo snapshot dos indicadores e registros.
