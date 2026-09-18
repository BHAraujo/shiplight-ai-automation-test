# Shiplight Test Project Context

## App profile

- Name: Application under test not provided yet.
- Framework: Unknown.
- Key pages and features: The scaffold includes a starter form flow against the Shiplight static demo at `https://static.shiplight.ai/testing/forms/basic-form.html`.

## Risk profile

- The product-specific risk areas are not known yet.
- The starter flow covers form completion, native date input, a country selector, required consent, and successful submission.

## Testing scope

- In scope: Product journeys and environments identified by the project owner.
- Current starter scope: Basic form submission happy path from `tests/example.test.yaml`.
- Out of scope: None decided yet.

## User roles

- Roles and permission levels are not known yet.

## Data strategy

- Test data creation and cleanup rules are not known yet.
- The starter flow uses isolated mock form data and does not define persistent cleanup.

## Targets

- Base URL: `https://static.shiplight.ai/` for the generated starter test only.
- Authentication: No authentication is configured for the starter flow.
- Special setup: Run under Node.js 22 or newer, as required by Shiplight.

## Known facts and decisions

- This is a newly scaffolded Shiplight test project.
- Credentials must be supplied through environment variables; raw secrets do not belong in specs or tests.
- The generated project uses the Shiplight YAML test format and Playwright configuration.

## Open questions

- What application should replace the generated static-demo target?
- What are the application base URL, deployment environments, and authentication requirements?
- Which user roles and permissions need coverage?
- How should test data be created, isolated, and cleaned up?
- Which product journeys and risk areas are highest priority?