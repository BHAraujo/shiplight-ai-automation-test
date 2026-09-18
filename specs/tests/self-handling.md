# Cadastro de usuario

- **Source:** [`tests/self-handling.test.yaml`](../../tests/self-handling.test.yaml)
- **Generated spec:** [`tests/self-handling.yaml.spec.ts`](../../tests/self-handling.yaml.spec.ts)
- **Name:** Cadastro de usuario
- **Goal:** Cadastrar um novo usuario com dados validos
- **Execution mode:** Deterministic actions against the local page

## Environment

- **Base URL:** `file:///users/brunoaraujo/Documents/test/`
- **Page:** `index.html`
- **Backend dependency:** `POST http://127.0.0.1:8000/usuarios`
- **Authentication:** None

## Test data

| Field | Value | Locator |
|---|---|---|
| Nome | `QA Test AI` | `getByPlaceholder('Escreva seu nome aqui')` |
| E-mail | `emailQATestAI@gmail.com` | `getByPlaceholder('exemplo@email.com')` |
| Telefone | `11992837465` | `locator('#telefone')` |
| Endereco | `Rua dos teste` | `locator('#endereco')` |

## Steps

1. Navigate to `index.html`.
2. Fill the Nome field with `QA Test AI`.
3. Fill the E-mail field with `emailQATestAI@gmail.com`.
4. Fill the Telefone field with `11992837465`.
5. Fill the Endereco field with `Rua dos teste`.
6. Click the `Cadastrar Usuário` button.

## Expected result

The page displays `Usuário cadastrado com sucesso!`.

## Known limitation

The expected result currently does not occur because the local page has an inconsistent Nome field wiring:

- The `Nome` label references `for="nome"`, while the input uses `id="test"`.
- The submission script calls `document.getElementById('nome')`, which returns `null`.
- The `POST /usuarios` request is therefore not sent and the success message is not displayed.

The backend must be running at `http://127.0.0.1:8000` and the `base_url` must point to the actual local directory containing `index.html` before rerunning the test.
