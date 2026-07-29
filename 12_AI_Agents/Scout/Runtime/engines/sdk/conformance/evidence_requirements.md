# Evidence Requirements

Certification evidence MUST include:

- validated Manifest and its SHA-256 hash;
- every validated Capability and hash;
- Registry entry and registration outcome;
- Foundation tag and commit;
- SDK version;
- Manifest, Capability, Registry, and test-suite versions;
- validator ID and version;
- complete validation report;
- positive and negative fixture outcomes;
- validation and certification timestamps;
- final result, warnings, failures, and governance decision.

Machine JSON uses deterministic key ordering and UTF-8 encoding for hashing. Inputs, validator dependencies, and fixture catalog are recorded. Evidence is immutable after certification; a new run creates a new record.

Unsigned evidence is valid only inside the trusted repository workflow. Future signatures wrap the unchanged evidence hash.

