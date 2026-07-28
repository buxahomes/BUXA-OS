# Scout Runtime Registry

## Purpose

The `registry/` directory contains governed machine-readable registries used by Scout Runtime.

Registries identify approved, provisional, conflicting, deprecated, and planned architectural entities. They are operational data, not JSON Schemas and not substitutes for normative Shared Foundation specifications.

## Authority Model

For Runtime Objects:

- `../engines/shared/runtime_objects.md` is the normative governance authority.
- `runtime_objects.json` is the complete machine-readable object registry.
- `../schemas/runtime_object_registry.schema.json` is the machine validation authority for the registry data.

A registry entry points to its normative owner and authoritative schema where one exists. Registration does not transfer semantic ownership to this directory.

## Files

| File | Purpose |
|---|---|
| `README.md` | Explains registry scope, navigation, and governance. |
| `runtime_objects.json` | Machine-readable registry of recognised Scout Runtime Objects. |

## Registry Data Rules

Registry data MUST:

- be strict JSON without comments or duplicate keys;
- validate against its declared registry schema;
- use deterministic entry ordering;
- preserve unique canonical names and object types;
- reference existing normative owners and non-null schema paths;
- use only recognised producing and consuming authorities;
- preserve unresolved conflicts instead of silently resolving them;
- remain synchronized with the human-readable normative specification.

## Change Process

A registry change requires:

1. source evidence;
2. authority and ownership review;
3. compatibility assessment;
4. normative specification update;
5. registry data update;
6. schema update where structure changes;
7. semantic and machine validation;
8. human approval where architecture changes.

Planned or conflicting entries MUST NOT be treated as approved production contracts.

## Validation

Validation includes:

- JSON parsing with duplicate-key rejection;
- Draft 2020-12 schema validation;
- object type and canonical name uniqueness;
- schema and owner path existence;
- related-object resolution;
- authority recognition;
- metadata count verification;
- Markdown and local-reference checks.

JSON Schema cannot enforce uniqueness of a property across different array entries; semantic validation MUST enforce unique `object_type` and `canonical_name`.

