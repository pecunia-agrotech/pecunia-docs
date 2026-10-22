# Verificação · entrega v2

Execução local em 21/10/2026, Python 3.12.14, Node 24.21 e Next.js 16.4.2.

| Verificação executada | Resultado |
|---|---|
| backend/.venv/bin/pytest -q | 46 aprovados; aviso de depreciação Starlette TestClient/httpx |
| backend/.venv/bin/ruff check . | Aprovado |
| backend/.venv/bin/mypy src main.py | Aprovado, 19 arquivos de produção |
| npm run typecheck | Aprovado |
| npm run lint | Aprovado, sem warnings |
| npm test | 19 aprovados, 3 arquivos |
| npm run contracts:check | Aprovado: FastAPI/OpenAPI/TypeScript sincronizados |
| npm run build | Build de produção aprovado |
| npm run test:e2e | 2 roteiros impedidos de iniciar pelo macOS; não aprovados |
| Migração em cópia do banco anterior | 74 registros preservados; detalhes em migration-validation.json |
| Navegador integrado com API real | Dashboard, calendário retroativo, criação de cenário e configuração isolada conferidos |

Os testes de backend criam bancos temporários e exercitam a API integrada ao SQLite. Os testes React usam mocks controlados e não substituem a verificação de integração real. O escopo do mypy é o código de produção; fixtures de pytest são executadas e verificadas pelo pytest/ruff.

O Playwright foi configurado para descobrir apenas arquivos *.spec.ts, separadamente do Vitest. O Chromium instalado encerrou na inicialização com `bootstrap_check_in ... MachPortRendezvousServer: Permission denied (1100)`, antes das ações dos testes. Responsividade e teclado ainda precisam da execução desses roteiros num ambiente que permita iniciar o navegador. Não há alegação de aprovação E2E.

O teste integrado manual usou portas 13000/18000 e banco descartável em work/, preservando o processo e o banco da versão anterior nas portas 3000/8000. O cenário temporário foi removido por API ao terminar, após conferir que a demonstração mantinha o preço do insumo em R$ 1,25 e o cenário utilizava R$ 5,00. O banco original não recebeu operações de teste. Capturas dashboard.png e calendar.png documentam a interface.

Para reproduzir sem afetar dados privados, defina PECUNIA_DB_PATH para um novo arquivo temporário antes de iniciar a API. O teste de migração não é substituto de uma validação com outros bancos legados; faça backup antes de atualizar.
