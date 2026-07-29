# SDK Conformance Infrastructure

Version: `1.0.0`

This directory defines Phase 2 validation, certification, evidence, fixtures, and release gates. It contains no Engine implementation or executable Runtime behavior.

## Navigation

- [Conformance Validator](conformance_validator.md)
- [Certification Process](certification_process.md)
- [Validation Matrix](validation_matrix.md)
- [Fixture Catalog](fixture_catalog.md)
- [Evidence Requirements](evidence_requirements.md)
- [Certification Checklist](certification_checklist.md)
- [Compatibility Matrix](compatibility_matrix.md)
- [SDK Release Checklist](sdk_release_checklist.md)

Positive fixtures resolve complete passing SDK artifacts. Negative fixtures are deterministic mutation envelopes, except `duplicate_keys.json`, which is intentionally invalid strict JSON.

The authoritative inputs remain [Architecture Design](../../Engine_SDK_Architecture_Design.md), [Architecture Decisions](../../Engine_SDK_Architecture_Decisions.md), and the frozen `runtime-foundation-v1.0.0` Foundation.

