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
    E -->|Compile core schemas| G[Compiled Schemas<br/>service.json<br/>organization.json<br/>location.json<br/>service_at_location.json]

    F --> H[UK ORUK Specification<br/>JSON Schema Files]
    G --> H

    H --> I[Generate OpenAPI]
    C -->|Profile metadata<br/>title, URL, description| I

    I --> J[UK ORUK OpenAPI<br/>YAML Specification<br/>openapi.yaml]

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
        G
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
   - **Profile Configuration** (`profile.json`): Metadata including title, base URL, version, and description for the UK profile
   - **[Schema Patches](./UK/profile/)** (`profile/` directory): UK-specific modifications to extend or restrict HSDS schemas

2. **Profile Wizard Processing**:
   - Fetches HSDS schemas from specified branch (default or version-specific)
   - Applies UK schema patches to customize fields, add UK-specific attributes, or remove unnecessary elements
   - Updates `$id` values based on the UK profile's base URL
   - Compiles core schemas (service, organization, location, service_at_location)

3. **Generated Outputs**:
   - **[UK JSON Schemas](./UK/schema/)**: Localized schema files in the `UK/schema/` directory
   - **OpenAPI YAML**: API specification document describing the UK ORUK REST API endpoints

