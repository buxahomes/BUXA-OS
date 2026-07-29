# Compatibility Matrix

| Foundation | SDK | Engine | Manifest | Capability | Registry | Certification |
|---|---|---|---|---|---|---|
| `runtime-foundation-v1.0.0` | `1.0.x` | independent SemVer | `1.0.x` | `1.0.x` | `1.0.x` | Eligible after full validation |
| `runtime-foundation-v1.0.0` | experimental range | experimental only | declared version | declared version | not active | Not certifiable |
| Any untested Foundation | any | any | any | any | any | Incompatible |
| Foundation tag/commit mismatch | any | any | any | any | any | Fatal |
| SDK major mismatch | incompatible until migration | new Engine version as required | migrate | migrate | migrate | Recertify |
| Schema major mismatch | matching SDK required | migrate package | exact supported major | exact supported major | exact supported major | Recertify |

Patch compatibility is not assumed when a patch tightens security or rejects prior valid behavior; the ADR breaking-change rules govern. Every certification stores exact versions even where a compatible family is documented.

