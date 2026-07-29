# Certification Process

## Status Flow

`Draft → Validated → Conformant → Certified → Deprecated`

`Revoked` is an exceptional terminal certification outcome reachable from any post-Draft status.

Draft and Validated correspond to package lifecycle `draft` and `validated`. Conformant, Certified, Deprecated, and Revoked are certification statuses. They are not Runtime States or Engine invocation statuses.

## Promotion

| Promotion | Requirement |
|---|---|
| Draft → Validated | Strict parsing, schemas, references, and local tests pass |
| Validated → Conformant | Complete matrix and fixtures pass; evidence reproducible |
| Conformant → Certified | Engine Layer Governance accepts immutable evidence |
| Certified → Deprecated | Replacement or sunset approved; migration published |
| Any → Revoked | Integrity, security, compatibility, or governance failure |

Certified activation additionally requires Registry `active` lifecycle, exact compatibility, dependencies, and deployment approval. Certification alone does not activate an Engine.

Content change creates a new version and new evidence. Earlier records remain immutable.

