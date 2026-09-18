# Basic form submission (deterministic)

- **Source:** [`tests/determinisc.test.yaml`](../../tests/determinisc.test.yaml)
- **Generated spec:** [`tests/determinisc.yaml.spec.ts`](../../tests/determinisc.yaml.spec.ts)
- **Goal:** Verify user can create a new project
- **Execution mode:** Deterministic actions with explicit Playwright locators

## Environment

- **Base URL:** `https://static.shiplight.ai`
- **Page:** `/testing/forms/basic-form.html`
- **Authentication:** None

## Test data

| Field | Value |
|---|---|
| Full Name | `Bruno Henrique Araujo` |
| Email Address | `RuadosQAs@gmail.com` |
| Birth Date | `2024-01-01` |
| Country | `Brazil` |
| Message | `Rua dos QAs` |
| Terms and Conditions | Agreed |

## Steps

1. Navigate to `/testing/forms/basic-form.html`.
2. Fill the Full Name field:
   - Click `getByRole('textbox', { name: 'Full Name *' })`.
   - Clear its existing content.
   - Enter `Bruno Henrique Araujo`.
3. Fill the Email Address field:
   - Click `getByRole('textbox', { name: 'Email Address *' })`.
   - Clear its existing content.
   - Enter `RuadosQAs@gmail.com`.
4. Set the Birth Date field to `2024-01-01`.
5. Select Brazil from the Country dropdown:
   - Open `getByRole('button', { name: 'Choose a country' })`.
   - Focus `getByRole('textbox', { name: 'Search countries...' })`.
   - Enter `Brazil` in the search field.
   - Select `getByText('🇧🇷 Brazil')`.
6. Fill `getByRole('textbox', { name: 'Message' })` with `Rua dos QAs`.
7. Click `getByRole('checkbox', { name: 'I agree to the Terms and' })`.
8. Click `getByRole('button', { name: 'Submit Form' })`.

## Expected result

The `#form-result` element contains `Success!`.

## Notes

All interactions use explicit actions and locators, so this test avoids AI locator resolution during the form interaction steps.
