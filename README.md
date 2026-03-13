# out-test-pipeline

Repositório orquestrador do pipeline de testes: **API → E2E → K6**.

Executa em sequência:
1. Testes de API (Playwright) — `out-e2e-playwright`
2. Testes E2E (Playwright) — `out-e2e-playwright`
3. Testes de performance (K6) — `out-performance-k6`

## Quando usar

- **Pipeline completo aqui:** para rodar API + E2E + K6 em sequência (workflow_dispatch ou push/PR na main).
- **Só API + E2E:** use o workflow **Playwright API + E2E** no repositório `out-e2e-playwright`.

## Repositórios necessários

- `out-e2e-playwright`
- `out-performance-k6`

Ambos na mesma organização do `out-test-pipeline`.
