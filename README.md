# shiplight-interpreted-automation-test

Projeto de testes end-to-end criado com o [Shiplight](https://docs.shiplight.ai) — um framework que roda sobre o Playwright e permite escrever testes em YAML usando linguagem natural ("intents"), com resolução automática de locators por IA, self-healing e um debugger visual.

## Propósito do projeto

Este repositório é um scaffold de testes para validar fluxos de formulário, comparando três estilos diferentes de automação suportados pelo Shiplight:

- **`tests/interpreted.test.yaml`** — teste 100% interpretado por IA. Cada passo é descrito apenas como uma intenção em linguagem natural (ex: `Enter "Bruno Henrique Araujo" in the Full Name field`), e o agente do Shiplight decide em tempo de execução qual elemento da página manipular. Roda contra o formulário de demonstração público `https://static.shiplight.ai/testing/forms/basic-form.html`.
- **`tests/determinisc.test.yaml`** — a mesma jornada do teste anterior, mas "travada" com `action` e `locator` explícitos (Playwright puro) para cada passo, tornando a execução determinística e mais rápida, sem depender de IA a cada rodada.
- **`tests/self-handling.test.yaml`** — teste local contra a página `index.html` deste repositório (um mini sistema de "Cadastro de Usuário" em `file://`). Este teste depende de um backend local rodando em `http://127.0.0.1:8000` (o formulário envia um `POST /usuarios`) e do arquivo `usuarios.db`. Há uma inconsistência conhecida documentada em `knowledge/cadastro-usuario.md`: o `label` do campo Nome aponta para `for="nome"`, mas o `<input>` real usa `id="test"`, e o script de envio procura `document.getElementById('nome')` — por isso o cadastro não é concluído.

Cada arquivo `.test.yaml` é transpilado automaticamente pelo Shiplight para um arquivo `.yaml.spec.ts` equivalente (ex: `tests/interpreted.yaml.spec.ts`), que é o que o Playwright de fato executa.

O objetivo do projeto é servir como base de testes de regressão para os fluxos de formulário acima, além de documentar o comportamento esperado (`specs/context.md`) e os achados de cada execução (pasta `knowledge/`).

## Estrutura do repositório

```
.
├── .env                        # credenciais/API keys (não versionado)
├── auth/example.login.ts       # fixture de exemplo para login com sessão persistida
├── index.html                  # página local usada pelo teste "self-handling"
├── knowledge/                  # observações e bugs encontrados durante execuções
├── playwright.config.ts        # configuração do Playwright + Shiplight
├── specs/context.md            # contexto do projeto (escopo, riscos, dados, etc.)
├── tests/
│   ├── interpreted.test.yaml       # teste 100% IA (linguagem natural)
│   ├── determinisc.test.yaml       # teste determinístico (locators fixos)
│   ├── self-handling.test.yaml     # teste local (arquivo index.html)
│   └── *.yaml.spec.ts              # specs gerados automaticamente (não editar à mão)
├── shiplight-report/            # relatório HTML da última execução
└── package.json
```

## Pré-requisitos

- **Node.js 22 ou superior**
- **npm**
- Uma forma de autenticação com a IA do Shiplight — uma das duas:
  - um **token da Shiplight** (`SHIPLIGHT_API_TOKEN`), ou
  - uma **API key de um provedor de IA** (Google, Anthropic ou OpenAI)

## Passo a passo para executar

### 1. Instalar as dependências

```bash
npm install
```

### 2. Configurar as credenciais

Edite o arquivo `.env` na raiz do projeto e defina **uma** das opções abaixo:

```bash
# Opção A: usar o proxy de IA do Shiplight (recomendado)
SHIPLIGHT_API_TOKEN=...

# Opção B: usar sua própria API key de IA
GOOGLE_API_KEY=...
# ou
ANTHROPIC_API_KEY=...
# ou
OPENAI_API_KEY=...

# Opcional: forçar um modelo específico
WEB_AGENT_MODEL=...
```

Se optar pelo token da Shiplight, gere-o com:

```bash
npx shiplight setup-api-token
```

Esse comando abre um fluxo de autenticação e grava `SHIPLIGHT_API_TOKEN` automaticamente no `.env`.

### 3. Instalar o navegador usado pelos testes (Chromium)

```bash
npx playwright install chromium
```

### 4. Rodar os testes

Rodar toda a suíte (todos os `.test.yaml` transpilados):

```bash
npm test
```

Isso equivale a `shiplight test`, que roda o Playwright com todas as flags padrão dele.

Rodar em modo "headed" (com o navegador visível):

```bash
npm run test:headed
```

Rodar apenas um teste específico:

```bash
npx shiplight test tests/interpreted.test.yaml
npx shiplight test tests/determinisc.test.yaml
npx shiplight test tests/self-handling.test.yaml
```

> **Sobre o teste `self-handling`:** ele aponta para `file:///users/brunoaraujo/Documents/test/index.html` e espera um backend respondendo em `http://127.0.0.1:8000/usuarios`. Ajuste o `base_url` no YAML para o caminho local correto da sua máquina e suba o backend que atende esse endpoint antes de rodar este teste — caso contrário o cadastro não será concluído (ver bug conhecido em `knowledge/cadastro-usuario.md`).

### 5. Ver o relatório da execução

Após rodar os testes, abra o relatório HTML gerado (com capturas de tela, vídeos e traces por passo):

```bash
npx shiplight report
```

ou abra diretamente `shiplight-report/latest/index.html` no navegador.

### 6. Depurar um teste visualmente (opcional)

```bash
npx shiplight debug tests/interpreted.test.yaml
```

Abre um debugger visual interativo para o teste indicado (a URL é impressa no terminal; use `--port N` para uma porta fixa).

## Comandos úteis do Shiplight

| Comando | Finalidade |
|---|---|
| `shiplight setup-api-token` | Autentica com a Shiplight e grava `SHIPLIGHT_API_TOKEN` no `.env` |
| `shiplight test [args]` | Roda a suíte de testes (repassa flags do Playwright) |
| `shiplight test:headed` | Roda a suíte com o navegador visível |
| `shiplight debug <arquivo>` | Abre o debugger visual para um teste YAML |
| `shiplight report [pasta]` | Gera/mescla o relatório HTML |
| `shiplight transpile [glob]` | Transpila os `.test.yaml` para `.yaml.spec.ts` (normalmente automático) |
| `shiplight spec <topico>` | Imprime a referência de autoria: `yaml` ou `actions` |

Referência completa de flags e opções: [docs.shiplight.ai/local/cli-reference](https://docs.shiplight.ai/local/cli-reference).

## Autenticação em fluxos protegidos

`auth/example.login.ts` é um exemplo de fixture de login reutilizável: ele navega até `baseUrl`, preenche usuário/senha, salva o `storageState` da sessão em `.auth/` e reaproveita essa sessão em execuções futuras (evitando logar a cada teste). Copie e adapte esse arquivo para o fluxo de login real da aplicação sob teste antes de usá-lo.

## Observações

- A pasta `.shiplight/` e `shiplight-report/` são geradas automaticamente a cada execução e não devem ser versionadas (já estão no `.gitignore`).
- Nunca coloque segredos diretamente nos arquivos de teste (`.yaml`) — use variáveis de ambiente via `.env`.
- O arquivo `specs/context.md` concentra o contexto de negócio do projeto (escopo, papéis de usuário, estratégia de dados) e deve ser atualizado conforme o projeto evolui.