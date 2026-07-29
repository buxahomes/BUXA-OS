# Engine SDK Validation Rules

## Validation Order

1. Parse every JSON document strictly and reject duplicate keys.
2. Validate all three schemas against Draft 2020-12.
3. Resolve schemas only through canonical `buxa.ai` IDs and declared local files.
4. Validate Manifest, Capability, Registry, templates, and examples.
5. Verify exact Foundation tag and commit.
6. Verify Engine ID, Engine type, canonical name, and version correlation.
7. Verify external Capability file ID, version, Engine type, permissions, and side-effect correlation.
8. Verify Registry type uniqueness, implementation uniqueness, counts, defaults, lifecycle, certification, and compatibility.
9. Verify Runtime Object status and producer or consumer authority against `../../registry/runtime_objects.json`.
10. Verify all Runtime States and transitions against `../shared/state_model.md`.
11. Verify all dynamic mechanisms and their typed governance.
12. Verify Decision references against the closed mapping to `../shared/decision_model.md`.
13. Verify permissions are a Capability subset of the Manifest and effective grants are default-deny.
14. Verify Tool declarations, side-effect ceilings, idempotency, retry, and timeout bounds.
15. Verify Engine Result fixtures against the Foundation schema and `runtime_execution_evidence_v1`.
16. Verify Markdown references, fences, whitespace, and `git diff --check`.

## Semantic Rules Beyond JSON Schema

JSON Schema cannot alone prove:

- Manifest Engine type matches the Engine ID segment;
- Registry defaults reference an implementation in the same type entry;
- Registry counts equal data;
- Capability files match their Manifest references;
- Capability permissions and side effects do not escalate;
- Runtime Object authority matches the canonical Engine;
- State transitions are allowed from declared sources;
- Decision ID mapping matches category and type;
- content hashes reproduce;
- no deployment data or secrets are embedded.

The conformance validator MUST implement these checks.

## Fail-Closed Rules

Unknown identifiers, unresolved references, deprecated aliases, planned objects, conflicting Registry status, missing governance evidence, undeclared Tools, permission escalation, invalid transitions, and incompatible Foundation versions are errors.

Warnings cannot override an error.

## Canonical IDs

SDK schema IDs use `https://buxa.ai/scout/runtime/engines/sdk/schemas/`. Foundation schema references use their released canonical IDs. No `buxa.local`, network fallback, alias, or diagnostic substitution is permitted.

