# Fixture Catalog

## Positive

| Fixture | Subject | Expected |
|---|---|---|
| `manifest.json` | Phase 1 example Manifest | Pass |
| `capability.json` | Phase 1 example Capability | Pass |
| `registry.json` | Empty eleven-type Registry | Pass |
| `registry_entry.json` | Conformant non-active entry | Pass |
| `retrieval_manifest.json` | Retrieval Manifest with semantic checks | Pass |
| `retrieval_capability.json` | Retrieval Capability with Foundation checks | Pass |
| `engine_package.json` | Correlated Manifest and Capability | Pass |
| `sdk_package.json` | Complete SDK package | Pass |

`registry_entry_subject.json` is support data for the Registry Entry fixture and is not a ninth fixture.

## Negative

Each negative JSON file contains `expected_validator_error`, `error_category`, and `recommended_remediation`. Cases cover missing and invalid Engine identity, Foundation compatibility, Capability duplication and identity, permissions, Runtime Object/State/Decision validity, conformance, schema and semantic versions, Registry references and lifecycle, broken files, and duplicate keys.

The duplicate-key file must fail during Stage 1. All other envelopes must parse, materialize exactly one intended invalid condition, and fail in the declared category.

