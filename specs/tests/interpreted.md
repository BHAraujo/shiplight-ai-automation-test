# Basic form submission (interpreted)

- **Source:** [`tests/interpreted.test.yaml`](../../tests/interpreted.test.yaml)
- **Generated spec:** [`tests/interpreted.yaml.spec.ts`](../../tests/interpreted.yaml.spec.ts)
- **Goal:** Verify the basic form can be completed and submitted successfully
- **Execution mode:** Interpreted intents resolved by Shiplight AI at runtime

## Environment

- **Base URL:** `https://static.shiplight.ai/`
- **Page:** `/testing/forms/basic-form.html`
- **Authentication:** None

## Test data

| Field | Value |
|---|---|
| Full Name | `Bruno Henrique Araujo` |
| Email Address | `RuadosQAs@gmail.com` |
| Birth Date | `2024-01-01` |
| Country | `Brazil` |
| Message | `Rua dos Test` |
| Terms and Conditions | Agreed |

## Steps

1. Navigate to `/testing/forms/basic-form.html`.
2. Enter `Bruno Henrique Araujo` in the Full Name field.
3. Enter `RuadosQAs@gmail.com` in the Email Address field.
4. Select `01/01/2024` from the Birth Date dropdown.
5. Select `Brazil` from the Country dropdown.
6. Enter `Rua dos Test` in the Message text area.
7. Select the `I agree to the Terms and Conditions` checkbox.
8. Click the `Submit Form` button.

## Expected result

The page displays `Success! Form submitted successfully.`

## Notes

The field interactions and element resolution are intentionally delegated to Shiplight AI. Runtime execution may take longer than the deterministic variant because locators are not specified in the source YAML.
