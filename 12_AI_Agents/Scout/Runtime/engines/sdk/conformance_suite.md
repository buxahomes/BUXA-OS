# Engine SDK Conformance Suite

Version: `1.0.0`

## Required Foundation and Schemas

- Foundation: `runtime-foundation-v1.0.0`
- Foundation commit: `2eb52d62d877dd8b5c5736721423dc1b2d5b5d36`
- SDK: `1.0.0`
- Manifest schema: `1.0.0`
- Capability schema: `1.0.0`
- Registry schema: `1.0.0`
- Test suite: `1.0.0`

## Required Fixture Groups

Positive fixtures:

- valid Manifest and separate Capability;
- normal invocation and `runtime_execution_evidence_v1` Result;
- waiting Result;
- failure, cancellation, and timeout Results;
- registered static transition proposal;
- each declared dynamic-target mechanism;
- each declared permission and side-effect path.

Negative fixtures:

- invalid and deprecated Engine identifiers;
- unknown Capability and Decision identifiers;
- unknown, provisional, and planned Runtime Object misuse;
- invalid State and transition;
- missing dynamic-target Decision or checkpoint;
- inline certified Capability;
- permission or side-effect escalation;
- undeclared Tool;
- incompatible Foundation version;
- deployment data or secret in Manifest;
- invalid Result or missing evidence profile;
- lifecycle and certification conflict.

Phase 1 templates and examples validate schema shape. Full invocation and Result fixtures are required when the first Engine package is implemented.

## Evidence

One immutable evidence record MUST include:

- SHA-256 content hash and algorithm;
- validation timestamp;
- validator ID and version;
- Foundation tag and commit;
- SDK and schema versions;
- test-suite version;
- Engine ID and version;
- input checksums;
- fixture totals and outcomes;
- final result, warnings, and failures.

Evidence must be deterministically reproducible. Revalidation creates a new record. SDK v1 repository certification may be unsigned; future signatures wrap the unchanged evidence hash.

## Certification Process

1. `experimental`: package isolated; not eligible for registration.
2. `conformant`: all automated mandatory checks pass.
3. `certified`: Engine Layer Governance accepts exact conformance evidence.
4. `deprecated`: historical certification remains traceable but is no longer preferred.
5. `revoked`: activation is prohibited.

Package lifecycle is separate. Activation additionally requires `active` lifecycle, certified status, exact compatibility, valid Registry entry, dependencies, and deployment approval.

## Release Gate

Certification fails if any mandatory fixture fails, evidence is not reproducible, the Foundation differs, an authority reference is invalid, or Foundation source was modified by the Engine package task.

