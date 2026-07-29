# Validation Matrix

| Target | Parse | Schema/meta | Compatibility | Semantic | References | Fixtures |
|---|---|---|---|---|---|---|
| Manifest schema | JSON/duplicates | Draft 2020-12 | SDK namespace | required fields and closed enums | local pointers | schema negatives |
| Capability schema | JSON/duplicates | Draft 2020-12 | schema version | IDs, permissions, effects | State/Object/Decision | capability negatives |
| Registry schema | JSON/duplicates | Draft 2020-12 | registry version | lifecycle/certification rules | implementation refs | registry negatives |
| Manifest | JSON/duplicates | Manifest schema | exact Foundation/SDK | identity/profile/ceilings | Capability files | positive and negative |
| Capability | JSON/duplicates | Capability schema | Capability schema | Engine and permission correlation | Foundation refs | positive and negative |
| Engine Registry | JSON/duplicates | Registry schema | Foundation/SDK | 11 unique types/counts/defaults | Manifest/evidence | positive and negative |
| Registry entry | JSON/duplicates | `$defs/implementation` | exact compatibility | lifecycle/certification | Manifest/Capability | entry fixtures |
| Engine package | all JSON | all applicable | exact pins | cross-file invariants | all package refs | package fixture |
| SDK package | all JSON | all schemas | SDK release | architecture boundaries | all local links | SDK fixture |
| Markdown documents | N/A | CommonMark fence rules | current docs | stale/contradictory language | local links | release audit |
| Positive fixtures | strict | target schema | required | declared checks | resolvable | must pass |
| Negative fixtures | strict except duplicate-key case | materialized target | per case | expected category | per case | must fail correctly |

Every matrix row is mandatory for certification where the target exists.

