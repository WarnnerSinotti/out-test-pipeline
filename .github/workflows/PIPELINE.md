# Pipeline Completo

O pipeline completo roda no repositório **out-test-pipeline**.  
Ele orquestra os testes de dois outros repositórios na mesma organização.

## Ordem de execução

1. **playwright-api** — Testes de API (out-e2e-playwright)
2. **playwright-e2e** — Testes E2E (out-e2e-playwright)
3. **k6** — Testes de performance (out-performance-k6)
4. **wdio-mobile** — Testes mobile (out-mobile-tests)

## Repositórios necessários

Os 4 repositórios precisam existir na mesma organização:

- `SEU_ORG/out-test-pipeline` (este — orquestrador)
- `SEU_ORG/out-e2e-playwright`
- `SEU_ORG/out-performance-k6`
- `SEU_ORG/out-mobile-tests`

## Disparo

- **workflow_dispatch** — Execução manual em Actions
- **push / PR** na branch `main` de out-test-pipeline

## Configuração para testes mobile

O job **wdio-mobile** precisa do APK do app. Configure uma das opções:

1. **APK no repositório:** Adicione `android/app.apk` no repositório out-mobile-tests
2. **Download via URL:** Configure o secret `APK_URL` no repositório out-test-pipeline com a URL de download do APK

## Testes apenas API + E2E

Para rodar **somente** os testes de API e E2E (sem K6), use o workflow **Playwright API + E2E** no repositório **out-e2e-playwright**.
