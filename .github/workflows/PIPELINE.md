# Pipeline: Completo (Playwright → K6)

## Ordem de execução
1. **Playwright API** — Testes de API (out-test-pipeline)
2. **Playwright E2E** — Testes E2E (out-test-pipeline)
3. **K6** — Testes de performance (out-performance-k6)

> **WebdriverIO** (out-e2e-webdriverIO) não está no pipeline devido à config de ambiente Android. Execução apenas local.

## Onde o pipeline está

O arquivo `pipeline.yml` está em **out-test-pipeline**. Executa os testes:
- API   - Integrated Test
- E2E   - E2E Test
- K6    - Performance Test

## Repositórios necessários

Na mesma organização:
- `/out-test-pipeline` — orquestrador + Playwright (API + E2E)
- `/out-performance-k6` — K6 (checkout em `k6/` no pipeline)
- `/out-e2e-webdriverIO` — fora do pipeline (Android)

## Relatórios por projeto

| Projeto   | Comando de teste      | Caminho dos artifacts |
|-----------|-----------------------|------------------------|
| Playwright | `npm run playwright:api`, `npm run playwright:e2e` | `playwright-report/` |
| K6        | `npm run test-smoke`  | `k6/src/reports/`      |

## Secrets

- `BASE_URL_TEST` (opcional): URL da API para K6. Se não for definido, usa JSONPlaceholder.


