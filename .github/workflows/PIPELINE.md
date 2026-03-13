# Pipeline Completo

O pipeline completo roda no repositório **out-test-pipeline**.  
Ele orquestra os testes de dois outros repositórios na mesma organização.

## Ordem de execução

1. **playwright-api** — Testes de API (out-e2e-playwright)
2. **playwright-e2e** — Testes E2E (out-e2e-playwright)
3. **k6** — Testes de performance (out-performance-k6)

## Repositórios necessários

Os 3 repositórios precisam existir na mesma organização:

- `SEU_ORG/out-test-pipeline` (este — orquestrador)
- `SEU_ORG/out-e2e-playwright`
- `SEU_ORG/out-performance-k6`

## Disparo

- **workflow_dispatch** — Execução manual em Actions
- **push / PR** na branch `main` de out-test-pipeline

## Testes apenas API + E2E

Para rodar **somente** os testes de API e E2E (sem K6), use o workflow **Playwright API + E2E** no repositório **out-e2e-playwright**.
