# Conformance Validator

## Stages and Order

| Stage | Checks | Blocking |
|---|---|---|
| 1 Parse | UTF-8, strict JSON, duplicate keys | Yes |
| 2 Schema | Draft 2020-12 meta-schema and instance validation | Yes |
| 3 Compatibility | Foundation tag/commit, SDK and schema versions | Yes |
| 4 Identity | Engine, Capability, Decision IDs and taxonomy mapping | Yes |
| 5 Foundation | Runtime Objects, States, transitions, dynamic targets | Yes |
| 6 Package | Manifest-to-Capability correlation and file hashes | Yes |
| 7 Permissions | Ceiling, subset, side effects, approval, policy | Yes |
| 8 Registry | uniqueness, counts, defaults, lifecycle, certification | Yes |
| 9 References | canonical schema IDs and repository-local files | Yes |
| 10 Fixtures | positive pass; negative fail for expected category | Yes |
| 11 Evidence | deterministic hashes and complete report | Yes |
| 12 Documentation | links, fences, whitespace, diff check | Release blocking |

Validation stops only when continuing would make later results misleading. Otherwise it reports all independent errors.

## Fixture Materialization

A mutation envelope loads `base_fixture`, applies the single declared JSON Pointer operation, and validates the resulting subject. `replace_many` applies all entries atomically. `append_copy` copies the referenced source value. The raw duplicate-key fixture is parsed without materialization.

## Output

The validator emits:

- run ID, validator ID/version, time;
- exact Foundation and SDK versions;
- input paths and SHA-256 hashes;
- stage outcomes;
- errors and warnings with category, path, rule, and remediation;
- positive and negative fixture totals;
- final `pass` or `fail`;
- certification eligibility.

## Levels

- `INFO`: trace evidence; never blocks.
- `WARNING`: non-mandatory concern; cannot override an error.
- `ERROR`: mandatory failure; blocks conformance.
- `FATAL`: integrity, parser, Foundation, or validator failure; aborts dependent stages.

Certification requires zero ERROR and FATAL outcomes, every positive fixture passing, every negative fixture failing in its expected category, reproducible evidence, exact compatibility, and governance acceptance.

