# SDK Release Checklist

## Validation

- [ ] Strict JSON and duplicate-key checks complete.
- [ ] All schemas pass Draft 2020-12 meta-validation.
- [ ] Canonical IDs and references resolve without fallback.
- [ ] Complete semantic matrix passes.

## Fixtures

- [ ] All required positive fixtures pass.
- [ ] All required negative fixtures fail correctly.
- [ ] Fixture catalog and expected remediation are current.

## Schemas and Registry

- [ ] Manifest, Capability, and Registry schemas are versioned.
- [ ] Registry has exactly eleven canonical types.
- [ ] Defaults reference certified active implementations or are null.
- [ ] Counts and hashes reproduce.

## Documentation and Compatibility

- [ ] SDK and conformance documentation agree.
- [ ] Links, fences, and whitespace pass.
- [ ] Compatibility matrix reflects exact tested versions.
- [ ] Deferred Tool, Orchestrator, and deployment work remains out of scope.

## Certification and Release

- [ ] Evidence record is complete and immutable.
- [ ] Governance acceptance is recorded.
- [ ] Foundation source diff is empty for the SDK release task.
- [ ] Release notes and migration notes are ready.
- [ ] Approved release tag follows repository governance.
- [ ] `git diff --check` passes before commit.

This checklist authorizes neither commit, tag, nor push; those actions require a separate explicit task.

