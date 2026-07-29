# Retrieval Engine v1 Conformance Report

## Result

`PASS` for SDK automated conformance. The package is `validated` and evidence status is `conformant`. It is not governance-certified, active, default, deployed, or production-ready.

## Validation scope

- Manifest `1.0.0` against the SDK Manifest schema
- five Capability files against the SDK Capability schema
- SDK Registry against its schema and candidate lifecycle rules
- seven Engine Result examples against the Foundation Engine Result schema
- Runtime State names, registered static transitions, and `retry_target`
- Runtime Object names and approved-output status
- ADR-03 Decision identifiers
- Capability references, SHA-256 hashes, permission inheritance, and side-effect ceiling
- eight positive fixtures and fifteen intended negative fixtures
- strict JSON, duplicate keys, Markdown fences, local links, trailing whitespace, and Git diff checks

## Evidence

Deterministic evidence is recorded in [conformance_evidence.json](conformance_evidence.json). Hashes cover the immutable Manifest and Capability files. No cryptographic signature or external certification is claimed.

## Limitations

The package contains declarations and fixtures, not a real Retrieval adapter. Abstract Tool dependencies are not Tool protocol implementations. `policy_decision` is provisional and used only as an experimental reference. The package is intentionally not activation-eligible and has no default Registry selection.

## Recommendation

Retain `conformant` and `validated`. Governance certification and activation should wait for an executable adapter, implementation-level tests, deployment controls, and formal Engine Layer Governance acceptance.

