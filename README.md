# out-test-pipeline

Pipeline unificado de testes CI/CD que orquestra a execução dos projetos OUT em uma única execução.

---

## Descrição

Este projeto centraliza e executa os testes em um pipeline único. O pipeline faz checkout dos repositórios **out-e2e-playwright** e **out-performance-k6** para rodar, em sequência: testes de API, E2E e performance.

> **Nota:** O projeto **out-e2e-webdriverIO** não integra o pipeline devido à configuração necessária de ambiente Android. Deixando este projeto para integração no futuro.

---

## Arquitetura / Estrutura de pastas

```
out-test-pipeline/
├── .github/workflows/
│   ├── pipeline.yml    # Definição do pipeline CI/CD
│   └── PIPELINE.md     # Documentação detalhada
├── README.md
└── ANALISE.md          # Análise da estrutura dos repos


```

### Repositórios integrados ao pipeline

| Projeto              | Tipo de teste | Integrado |
|----------------------|---------------|-----------|
| out-e2e-playwright   | API + E2E     | ✅ Sim    |
| out-performance-k6   | Performance   | ✅ Sim    |
| out-e2e-webdriverIO  | Mobile        | ❌ Não (config Android) |

---

## Versões utilizadas

| Ferramenta     | Versão    |
|----------------|-----------|
| Node.js        | 20        |
| Runner (GitHub)| ubuntu-22.04 |
| Playwright     | Chromium  |
| K6             | stable (repositório oficial) |

---

## Dependências

### Node.js e npm

```bash
# Instalar Node.js 20 (recomendado via nvm ou fnm)
nvm install 20
nvm use 20
```

### out-e2e-playwright (Playwright)

```bash
cd out-e2e-playwright  # ou clone o repositório
npm ci
npx playwright install --with-deps chromium
```

### K6 (para testes de performance locais)

```bash
# Ubuntu/Debian
sudo gpg --no-default-keyring --keyring /usr/share/keyrings/k6-archive-keyring.gpg \
  --keyserver hkp://keyserver.ubuntu.com:80 \
  --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69

echo "deb [signed-by=/usr/share/keyrings/k6-archive-keyring.gpg] https://dl.k6.io/deb stable main" \
  | sudo tee /etc/apt/sources.list.d/k6.list

sudo apt-get update
sudo apt-get install -y k6
```

---

## Como executar os testes e gerar o relatório

### Pipeline via GitHub Actions

O pipeline é disparado em:
- **push** na branch `main`
- **pull_request** para `main`
- **workflow_dispatch** (manual)



### Extras - Execução local

#### Testes Playwright (API)

```bash
cd out-e2e-playwright
npm run playwright:api
# Relatório em: playwright-report/
```

Variáveis de ambiente (opcionais):
- `API_POSTS_BASE_URL` (default: `https://jsonplaceholder.typicode.com`)

#### Testes Playwright (E2E)

```bash
cd out-e2e-playwright
npm run playwright:e2e
# Relatório em: playwright-report/
```

Variáveis de ambiente (opcionais):
- `BASE_URL` (default: `https://the-internet.herokuapp.com`)
- `USUARIO` (default: `tomsmith`)
- `SENHA` (default: `SuperSecretPassword!`)

#### Testes K6 (Performance)

Os testes K6 rodam no subprojeto `out-performance-k6` (checkout no pipeline):

```bash
cd k6  # ou clone out-performance-k6
npm ci
npm run test-smoke
# Relatório em: k6/src/reports/
```

Variável de ambiente (opcional):
- `BASE_URL_TEST` (default: `https://jsonplaceholder.typicode.com`)

### Relatórios e artifacts

| Job        | Artifact              | Caminho              |
|------------|-----------------------|----------------------|
| Playwright API | playwright-api-report   | `playwright/playwright-report/` |
| Playwright E2E | playwright-e2e-report  | `playwright/playwright-report/` |
| K6         | k6-report             | `k6/src/reports/`    |

Em caso de falha no Playwright, o relatório é enviado como artifact no GitHub Actions.

Obs: todos os relatorios é gerado automaticamente, rodando localmente ou acessando via github