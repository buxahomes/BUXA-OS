# Retrieval Engine v1 Certification Checklist

- [x] Canonical Engine identity and exact Foundation pin
- [x] SDK version and schema versions declared
- [x] Manifest validates
- [x] Five immutable Capability files validate
- [x] Capability content hashes match the Manifest
- [x] Registry candidate validates
- [x] Candidate is non-active, non-default, and not activation-eligible
- [x] Runtime Object references resolve
- [x] Conformant outputs use approved Runtime Objects only
- [x] Runtime State references and static transitions resolve
- [x] Governed retry uses `retry_target`
- [x] Decision identifiers resolve through ADR-03
- [x] Permissions are default-deny and inherit from the Manifest ceiling
- [x] Side-effect level is `none`
- [x] Engine Result examples use `runtime_execution_evidence_v1`
- [x] Positive fixtures pass
- [x] Negative fixtures fail for their intended reason
- [x] JSON, Markdown, links, whitespace, and diff checks pass
- [ ] Executable Retrieval adapter implemented
- [ ] Production connector and deployment controls validated
- [ ] Engine Layer Governance accepts certification evidence
- [ ] Deployment approval granted
- [ ] Registry promoted to `active` or selected as default

Certification recommendation: remain `conformant`, not `certified`.

