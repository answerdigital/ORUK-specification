# ORUK-specification

## UK Profile Generation Process

The following diagram illustrates how the Open Referral UK (ORUK) specification is generated from the international HSDS (Human Services Data Specification) using the HSDS Profile Wizard:

```mermaid
flowchart TD
    A[HSDS International<br/>Specification] --> B[HSDS Profile Wizard]

    D[UK Schema Patches<br/>profile/ directory] --> B
    C[UK Profile JSON<br/>Configuration] --> B
    B --> E{Schema Processing}

    E -->|Apply patches| F[Generate UK Schemas<br/>schema/ directory]

    F --> H[UK ORUK Specification<br/>JSON Schema Files]

    E --> I[Generate OpenAPI]
    C -->|Profile metadata<br/>title, URL, description| I

    I --> J[UK ORUK OpenAPI<br/>JSON Specification<br/>schema/openapi.json]

    style A fill:#e1f5ff
    style B fill:#fff4e1
    style H fill:#e8f5e9
    style J fill:#e8f5e9
    style C fill:#f3e5f5
    style D fill:#f3e5f5

    subgraph inputs[Input Sources]
        A
        C
        D
    end

    subgraph processing[Profile Generation]
        B
        E
        F
        I
    end

    subgraph outputs[UK Specification Outputs]
        H
        J
    end

    click D "./UK/profile/" "View UK schema patches"
    click H "./UK/schema/" "View generated UK schemas"
```

### Process Steps

1. **Input Sources**:
   - **HSDS International Specification**: Base schemas from the international Human Services Data Specification
   - **[Profile Configuration](./UK/profile.json)**: Metadata including title, base URL, version, and description for the UK profile
   - **[Schema Patches](./UK/profile/)** (`profile/` directory): UK-specific modifications to extend or restrict HSDS schemas

2. **Profile Wizard Processing**:
   - Fetches HSDS schemas from branch `3.0`
   - Applies UK schema patches to customize fields, add UK-specific attributes, or remove unnecessary elements
   - Updates `$id` values based on the UK profile's base URL
   - Generates the patched schemas, including the explicitly defined list-response schemas

3. **Generated Outputs**:
   - **[UK JSON Schemas](./UK/schema/)**: Localized schema files in the `UK/schema/` directory
   - **[OpenAPI JSON](./UK/schema/openapi.json)**: API specification document describing the UK ORUK REST API endpoints

### Regenerating the UK profile

#### GitHub Action

Run [Generate profile schemas (HSDS Profile Wizard)](./.github/workflows/hsds-profile-wizard.yml)
manually from the repository's **Actions** tab. The workflow checks out `main`,
generates schemas using [UK/profile.json](./UK/profile.json) and
[UK/profile](./UK/profile/), and commits changes under [UK/schema](./UK/schema/)
to a new `feature/hsds-profile-wizard-<run id>-<attempt>` branch. It provides a
comparison link for opening a pull request; it does not update `main` directly
or create a branch when generation produces no changes.

Inputs match the source action:

- `hsds_branch`: HSDS specification branch (default `3.0`).
- `wizard_ref`: wizard branch or tag (default `main`).

The run summary records the wizard commit, changed files, and generation output.
If a `UK/schema/compiled` directory is added by separate tooling in future,
the action preserves it, matching the source workflow.