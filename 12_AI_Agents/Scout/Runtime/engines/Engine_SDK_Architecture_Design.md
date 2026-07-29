# Scout Runtime Engine SDK Architecture Design

**Document ID:** `SCOUT-RUNTIME-ENGINE-SDK-ARCHITECTURE-DESIGN`

**Version:** `1.0.0-design`

**Status:** Architecture Design

**Owner:** Scout Runtime

**Foundation dependency:** `runtime-foundation-v1.0.0`

**Foundation tag commit:** `2eb52d62d877dd8b5c5736721423dc1b2d5b5d36`

**Target:** Scout Runtime Engine Layer v1

---

## 1. Executive Summary

The Engine SDK should be a thin, language-neutral conformance layer between the released Runtime Foundation and individual Engine implementations. It should standardise how an implementation declares identity, capabilities, compatibility, permissions, dependencies, invocation consumption, canonical Engine Result production, testing, and registration. It must reference Foundation authorities and must not restate their semantics.

SDK v1 should contain:

- one human-readable Engine specification;
- one Engine Manifest schema;
- one Capability schema;
- one Engine Registry schema and one registry instance;
- a small template and conformance-fixture set;
- no separate Engine Profile schema.

An Engine Profile should be a structured section of the Manifest. Capability declarations should be versioned machine-readable files owned by an Engine package and referenced by its Manifest. They should not form a second global registry. The Engine Registry should index implementations of the eleven canonical taxonomy members; it must never create new Engine types.

Every SDK-conformant v1 Engine must return the Foundation Engine Result using `runtime_execution_evidence_v1`. Foundation-level Alpha Engine Results remain valid for backward compatibility, but an implementation that emits only the Alpha envelope cannot be certified as SDK v1 conformant.

“Writing Engine” should not be added to the canonical taxonomy. Writing is an execution Skill within a governed multi-Engine workflow: Business Judgement selects the communication approach, Planning creates the Execution Blueprint, Execution invokes the writing Skill, and Quality evaluates the output.

The recommended first implementation is the Retrieval Engine. It exercises the central SDK boundaries without needing irreversible side effects.

---

## 2. Design Context

The Shared Runtime Foundation is frozen at `runtime-foundation-v1.0.0` with Architecture Certification `YES` and Release Decision `PASS`. The Foundation owns Runtime semantics. The Engine SDK starts the implementation layer and depends on those semantics.

Authority flows as follows:

```text
runtime-foundation-v1.0.0
        ↓
Engine SDK contracts and validators
        ↓
Canonical Engine implementation packages
        ↓
Capabilities and Skills
        ↓
Deployment adapters and configured instances
```

Historical Alpha and Beta reviews are provenance only. They are not inputs to SDK validation. Compatibility behaviour explicitly retained by the released Foundation remains authoritative.

---

## 3. Foundation Constraints

The SDK must use these authorities directly:

| Domain | Foundation authority |
|---|---|
| Engine taxonomy and Engine Result semantics | `shared/engine_contract.md` |
| Engine Context | `shared/engine_context.md` |
| Runtime States, transitions, dynamic targets, terminal semantics | `shared/state_model.md` |
| Decision semantics and Decision types | `shared/decision_model.md` |
| Runtime Object architecture | `shared/runtime_objects.md` |
| Runtime Object machine registry | `../registry/runtime_objects.json` |
| Runtime schemas | `../schemas/*.schema.json` |

The SDK must not:

- define, alias, or commit Runtime State;
- create Runtime Object types;
- create a second Engine Result or Context envelope;
- duplicate Decision fields or lifecycle rules;
- add canonical Engine types;
- extend dynamic-target mechanisms;
- transfer approval or policy authority;
- create a competing Runtime registry or schema namespace.

If an implementation cannot be expressed under these constraints, it is incompatible with Foundation v1.0.0. The SDK should report that incompatibility rather than silently expanding the Foundation.

---

## 4. Engine SDK Purpose

The Engine SDK is:

- a declarative package contract for Engine implementations;
- a validation boundary against the released Foundation;
- a standard invocation and result adapter interface;
- a conformance-test architecture;
- a registration and compatibility mechanism;
- a reusable implementation scaffold.

The Engine SDK is not:

- a Runtime Orchestrator;
- a Runtime State or Decision authority;
- a Runtime Object registry;
- an Engine implementation;
- a domain workflow;
- a Skill or Tool protocol;
- deployment infrastructure;
- a prompt store;
- a secrets or credentials store.

Boundary definitions:

| Layer | Responsibility |
|---|---|
| Runtime Foundation | Defines shared Runtime truth and governance |
| Engine SDK | Declares and validates implementation conformance |
| Engine implementation | Performs one canonical Engine responsibility |
| Domain capability | Describes a bounded operation exposed by an implementation |
| Skill | Performs a reusable business function, normally under Execution |
| Tool | Provides a technical integration or operation |
| Runtime Orchestrator | Selects Engines, supplies Context, validates proposals, commits State |
| Configuration | Supplies non-secret operational settings within declared bounds |
| Deployment adapter | Connects an Engine package to a process, service, queue, or platform |

---

## 5. Architecture Principles

1. **Reference before definition.** Foundation concepts are referenced by canonical identifier and version.
2. **One taxonomy, many implementations.** Multiple implementations may serve one canonical Engine type.
3. **One implementation, bounded capabilities.** Capability expansion does not create Engine taxonomy expansion.
4. **Declarations do not grant authority.** Manifests describe requested and supported behaviour; policy, approval, registry, and Orchestrator checks grant runtime permission.
5. **Immutable releases.** A released Manifest and its referenced capability files are immutable for that implementation version.
6. **Default deny.** Undeclared objects, States, Decision types, tools, permissions, and side effects are prohibited.
7. **Canonical evidence.** SDK v1 Engines emit `runtime_execution_evidence_v1`.
8. **Language neutrality.** Machine contracts define behaviour without selecting an implementation language.
9. **Deployment separation.** Credentials, endpoints, replicas, and instance identifiers remain outside the package Manifest.
10. **Testable governance.** Every declaration must support positive and negative conformance tests.

---

## 6. Canonical Directory Structure

The smallest scalable target structure is:

```text
engines/
├── Engine_SDK_Architecture_Design.md
├── shared/
├── sdk/
│   ├── README.md
│   ├── engine_specification.md
│   ├── schemas/
│   │   ├── engine_manifest.schema.json
│   │   ├── engine_capability.schema.json
│   │   └── engine_registry.schema.json
│   ├── registry/
│   │   └── engine_registry.json
│   ├── templates/
│   │   ├── engine_README.template.md
│   │   ├── engine_manifest.template.json
│   │   ├── engine_capability.template.json
│   │   └── conformance_checklist.template.md
│   ├── examples/
│   │   ├── positive/
│   │   └── negative/
│   └── conformance/
│       └── README.md
├── input/
├── retrieval/
├── business_judgement/
├── planning/
├── execution/
├── quality/
├── learning/
├── memory/
├── exception/
├── approval/
└── policy/
```

The eleven Engine-type directories should be created only when implementation begins. Where one canonical type has multiple implementations, its directory should contain one subdirectory per `engine_id`. Generated validation reports should remain build artifacts and should not become normative sources.

File classes:

| Class | Examples | Authority |
|---|---|---|
| Normative human-readable | `engine_specification.md` | SDK behavioural contract |
| Normative machine-readable | schemas and Engine Registry | SDK validation authority |
| Engine-specific | Manifest, capability files, README, tests | Implementation package |
| Non-normative guidance | templates and positive examples | Authoring aid |
| Negative fixtures | invalid conformance cases | Validator evidence |
| Generated | reports, compiled clients, caches | Non-authoritative artifact |

---

## 7. Engine Identity Model

Engine type identity and implementation identity must be separate.

Canonical Engine type identity:

- `engine_type`: one fixed machine identifier mapped one-to-one to a Foundation canonical Engine name;
- `canonical_name`: the exact Foundation taxonomy name.

Implementation identity:

- `engine_id`: globally unique, stable identifier for one implementation lineage;
- `display_name`: human-readable implementation name;
- `version`: immutable implementation release version;
- `owner`: accountable repository or organisational owner.

Deployment instance identity:

- `deployment_instance_id`;
- environment, region, endpoint, process, and replica identity.

Deployment identity must be supplied by deployment configuration or the invocation environment. It must not be part of the immutable Engine identity.

Recommended required identity fields:

- `manifest_schema_version`;
- `engine_id`;
- `engine_type`;
- `canonical_name`;
- `display_name`;
- `version`;
- `owner`;
- `sdk_version`;
- `foundation_compatibility`.

`entrypoint` is required for an executable package but is packaging metadata, not Engine identity. `status` belongs to the Engine Registry lifecycle, not the Manifest. `supported_runtime_objects`, States, Decision types, capabilities, permissions, and dependencies are behavioural declarations, not identity.

---

## 8. Engine Manifest

The Manifest is the immutable, machine-readable declaration for one Engine implementation version.

Required sections:

| Section | Content | Enforcement |
|---|---|---|
| `identity` | Engine type and implementation identity | Schema and taxonomy validator |
| `compatibility` | exact Foundation dependency, SDK range, schema versions | Compatibility validator |
| `entrypoint` | adapter type and package-local entrypoint | Package validator |
| `capability_refs` | versioned package-local capability files | Reference validator |
| `runtime_objects` | consume, produce, update, reference sets | Runtime Object Registry validator |
| `runtime_states` | valid-current, entry, and proposed-next State references | State Model validator |
| `decisions` | produced, consumed, required-before-action and transition types | Decision Model validator |
| `profile` | combined operational constraints | SDK validator |
| `permissions` | requested permissions and side-effect classes | Policy and deployment validator |
| `dependencies` | Engine, Skill, Tool, schema, and package dependencies | Dependency validator |
| `conformance` | fixture locations and required test suites | Conformance runner |
| `deprecation` | replacement and sunset metadata, or null | Registry validator |

Informational fields may include description, maintainers, documentation path, support contact, and source path. Informational fields must not alter authority.

Runtime-enforced fields include taxonomy identity, compatibility, object access, State references, Decision obligations, permission ceilings, tool dependencies, result profile, and conformance status.

One Engine type may have multiple implementations. Each implementation has its own `engine_id` and versioned Manifest. One Manifest describes one implementation version only.

The exact v1 Foundation dependency should be declared as `runtime-foundation-v1.0.0` and bound to the release tag. A compatible range may be recorded for future selection, but certification must retain the exact Foundation version against which tests ran.

Manifests must not contain prompts, prompt bodies, secrets, credentials, tokens, private keys, environment endpoints, or deployment replicas.

---

## 9. Capability Model

A Capability is a declarative statement of what one Engine implementation can do within its canonical Engine responsibility.

A Capability is not:

- an Engine type;
- a Skill implementation;
- a Tool;
- a Task type;
- Runtime Object production authority;
- permission to act.

Recommended required fields:

- `capability_id`, unique within `engine_id`;
- `version`;
- `name`;
- `description`;
- `engine_type`;
- `input_object_types`;
- `output_object_types`;
- `required_context`;
- `allowed_runtime_states`;
- `proposed_next_states`;
- `required_decision_types`;
- `quality_requirements`;
- `approval_requirements`;
- `policy_requirements`;
- `tool_requirements`;
- `side_effect_level`;
- `idempotency`;
- `retry_profile`;
- `timeout_profile`.

Every object reference must resolve through `runtime_objects.json`. Object production declarations must be a subset of the producing authority granted to the canonical Engine. State references must resolve through `state_model.md`. Decision declarations must use canonical Decision types.

Capabilities should be stored as package-local files referenced by the Manifest. They should not be globally registered in SDK v1. The Engine Registry indexes implementations and may expose capability IDs for discovery, but the Manifest remains the capability ownership boundary.

Capability IDs should be stable within an `engine_id`. Reusing a name across Engines does not imply equivalence.

---

## 10. Engine Profile Model

SDK v1 should use one combined `profile` section in the Manifest. Separate profile schemas would create unnecessary versioning surfaces and increase the risk of contradictory declarations.

The profile should contain bounded subsections:

- `execution`: concurrency, idempotency support, deterministic mode, resource classes;
- `decision`: Decision obligations and evidence handling references;
- `quality`: local validation and Quality Engine hand-off requirements;
- `security`: data classifications and required controls;
- `memory`: read and proposal boundaries; no direct ownership transfer;
- `recovery`: supported retry, recovery, rollback, and checkpoint behaviours;
- `observability`: required Runtime Events, metrics, tracing, and audit correlation;
- `tools`: declared Tool dependency IDs and permission classes.

Information placement:

| Information | Location |
|---|---|
| Stable package identity and ceilings | Manifest |
| Operation-specific inputs and outcomes | Capability |
| Cross-capability operational posture | Manifest profile |
| Endpoints, replicas, credentials, concrete limits | Deployment configuration |
| Authorised task-specific data and evidence | Engine Context |

The profile must reference Foundation requirements and may narrow them. It must never relax them.

---

## 11. Invocation Boundary

The SDK should expose one consumption interface around the released Engine Contract. It should not define a new Context schema.

The invocation adapter should provide:

- Foundation invocation identity;
- the authorised Engine Context as an opaque, validated Foundation object;
- Task and relevant Runtime Object references selected from that Context;
- Decision references and required approval or policy evidence;
- current Runtime State and expected State version;
- cancellation and timeout signals;
- checkpoint references where applicable;
- Tool availability filtered by Manifest, policy, and deployment;
- resource limits and non-secret configuration;
- trace and correlation identifiers.

Validation order:

1. validate invocation and Context identity;
2. verify Engine type and implementation compatibility;
3. verify current State is declared and registered;
4. verify referenced Runtime Objects and schemas;
5. verify prerequisite Decisions, approvals, and policies;
6. intersect declared permissions with policy and deployment grants;
7. expose only authorised Tools and resources;
8. invoke the Engine implementation.

The adapter may provide typed accessors and validation helpers. It must preserve the original Context and references for audit and must not mutate or reinterpret them.

---

## 12. Engine Result Boundary

Every SDK v1 implementation must return the Foundation Engine Result validated by `../schemas/engine_result.schema.json` and governed by `shared/engine_contract.md`.

SDK v1 certification requires:

- the canonical top-level Engine Result fields;
- one final Engine Invocation Status;
- `evidence_profile: runtime_execution_evidence_v1`;
- invocation identity evidence;
- Decision evidence references without copied Decision semantics;
- Runtime State Transition evidence where a proposal exists;
- Runtime Events;
- evidence collection;
- validation results;
- produced Runtime Object references or embedded objects where permitted;
- failure, cancellation, timeout, waiting, and recovery evidence as applicable.

The SDK must provide a result builder that can only emit schema-valid Foundation results. It must not introduce an alternate envelope.

Alpha compatibility policy:

- the Foundation validator continues to accept the documented Alpha envelope;
- SDK adapters may consume or replay valid Alpha results;
- new SDK v1 Engines must emit `runtime_execution_evidence_v1`;
- an Alpha-only producer is Foundation-compatible but not SDK v1 conformant.

`waiting` is a final disposition for one invocation. Resumption occurs through a new invocation with preserved workflow evidence, not by mutating the prior Engine Result. `blocked`, `failed`, `cancelled`, and `timeout` do not independently commit corresponding Runtime States.

---

## 13. Runtime State Interaction

An Engine declares where it can operate; it never owns Runtime State.

Each Capability should declare:

- `valid_current_states`;
- `entry_states`;
- `proposed_next_states`;
- supported `dynamic_target_mechanisms`;
- waiting or blocked outcomes it may report.

Direct State enumeration is preferable to named SDK State profiles in v1. Named profiles could become a competing transition abstraction and obscure Foundation changes. Repetition is acceptable because validation remains mechanical and the State Model remains authoritative.

Rules:

- every State name must exist in the 70-state registry;
- proposed static targets must be authorised for the source State;
- dynamic targets must use only the seven Foundation mechanisms;
- the required Decision, checkpoint, origin context, and target validation must be present;
- terminal restrictions remain absolute;
- only the Runtime Orchestrator may validate and commit the proposal;
- waiting and blocked invocation outcomes do not invent Runtime States.

---

## 14. Decision Interaction

The Manifest and Capabilities may declare references and obligations only.

Declarations should distinguish:

- `produced_decision_types`;
- `consumed_decision_types`;
- `required_before_action`;
- `required_before_transition`;
- `approval_dependencies`;
- `policy_dependencies`;
- `retry_authorisation`;
- `recovery_authorisation`;
- `rollback_authorisation`;
- `revision_authorisation`.

Every type must resolve to `decision_model.md`. Decision identity and evidence are passed by reference. The SDK must not reproduce the universal Decision structure.

An Engine declaration that it can produce a Decision does not grant approval, policy, or human authority. The Decision must still satisfy its canonical owner, evidence, validity, and authority rules.

---

## 15. Runtime Object Interaction

The Manifest should use separate sets for:

- `consumes`;
- `produces`;
- `updates`;
- `references`.

Validation rules:

- every type must exist in `runtime_objects.json`;
- production and consumption must match registry authorities;
- schema-backed objects must validate before return or use;
- immutable objects cannot appear in `updates`;
- embedded objects retain their own identities and schemas;
- approved objects are available by default;
- provisional objects require an explicit SDK compatibility flag and runtime policy approval;
- planned objects cannot be used by a conformant Engine;
- schema-deferred objects must be validated under their normative human-readable contract.

The SDK must never infer authority from a Capability description.

---

## 16. Permissions and Side Effects

Permissions should be declarative, composable, and default-deny.

Recommended permission dimensions:

- data action: read, internal write, external write;
- side-effect class: none, reversible, irreversible;
- data classification: public, internal, restricted, secret-mediated;
- Tool invocation;
- network access;
- repository modification;
- publishing;
- communication;
- approval requirement;
- policy requirement.

The Manifest declares the maximum requested permission envelope. A Capability declares the subset it may need. Deployment configuration supplies available infrastructure. Runtime policy and approval determine the effective grant:

```text
effective permission
= manifest ceiling
∩ capability request
∩ deployment availability
∩ policy grant
∩ approval grant
```

Any missing grant produces denial, waiting, or governed escalation. A Manifest declaration never replaces a Policy Decision or Approval Decision. Secrets are resolved through a deployment secret provider and exposed only as opaque handles; they never appear in the Manifest, Engine Context evidence, logs, or Engine Result.

---

## 17. Tool Integration

Tools are technical integrations, not Runtime Objects. The current Runtime Object Registry separately contains planned `Tool Request` and `Tool Result` object types with deferred schemas. Those planned evidence objects do not make a Tool itself a Runtime Object and do not define an implemented Tool protocol.

SDK v1 should support Tool dependency declarations containing:

- `tool_id`;
- compatible interface version;
- required operations;
- permission and side-effect classes;
- timeout and retry expectations;
- optional versus required status.

The deployment adapter resolves a declared `tool_id` to an available implementation after policy and permission checks. Undeclared Tools must not be exposed.

Until the planned Tool Request and Tool Result contracts become authoritative and schema-backed, the SDK should not invent a complete Tool protocol or treat those planned objects as available. Tool activity should be captured through existing Runtime Events, Engine Result evidence, side-effect records, metrics, and Exception Records as applicable.

Implementation of the registered Tool Request and Tool Result plans requires a separate compatibility design. The Engine SDK may reference their registry status but must not redefine, activate, or assign schemas to them.

---

## 18. Quality and Conformance

An Engine package is conformant only when it has:

- a valid immutable Manifest;
- valid referenced Capability declarations;
- exact Foundation compatibility evidence;
- canonical Engine taxonomy identity;
- canonical Engine Result fixtures using `runtime_execution_evidence_v1`;
- valid State, Runtime Object, Decision, permission, and Tool references;
- positive fixtures for normal and supported conditional paths;
- negative fixtures for every declared governance boundary;
- deterministic validation output;
- valid Markdown and JSON.

Use one Registry lifecycle rather than a second conformance-status taxonomy:

- `proposed`: identity reserved, design incomplete;
- `draft`: implementation under development;
- `validated`: automated conformance passed;
- `registered`: accepted into the canonical Engine Registry;
- `active`: selectable by the Orchestrator;
- `deprecated`: supported for migration but not preferred;
- `retired`: prohibited for new selection.

“Certified” should be a signed or recorded conformance result associated with an Engine version, not another lifecycle state.

---

## 19. Engine Registry

There should be one Engine Registry at `sdk/registry/engine_registry.json`, validated by `sdk/schemas/engine_registry.schema.json` and owned by Scout Runtime Engine Layer governance.

Each entry should contain:

- canonical `engine_type` and exact `canonical_name`;
- `engine_id` and implementation `version`;
- Manifest path and checksum;
- exact Foundation and SDK versions validated;
- Registry lifecycle status;
- conformance result reference and timestamp;
- activation eligibility;
- default-selection priority;
- deprecation or replacement metadata;
- supported Capability IDs.

One taxonomy Engine may have multiple implementations. At most one implementation version should be the default for a deployment selection profile, but the global Registry need not claim one universal default.

Aliases may describe implementation names only. `Reasoning Engine` and `Feedback Engine` must be rejected as machine Engine types or authorities.

The Registry is an Engine implementation index. It does not replace `runtime_objects.json` and cannot grant Runtime Object production authority.

---

## 20. Versioning and Compatibility

Version relationships:

| Version | Meaning |
|---|---|
| Foundation version | Frozen Runtime semantic dependency |
| SDK version | SDK behaviour and validation-suite version |
| Engine implementation version | Executable package release |
| Manifest schema version | Manifest document syntax |
| Capability version | Operation declaration version |
| Engine Registry schema version | Registry document syntax |

All SDK and Engine versions should follow semantic versioning.

SDK schemas should use the canonical subordinate namespace `https://buxa.ai/scout/runtime/engines/sdk/schemas/`. This extends the released `buxa.ai` authority without competing with the Foundation schema directory. References to Foundation schemas must use their released canonical IDs.

Compatibility rules:

- certification records the exact Foundation tag and SDK version;
- a declared Foundation range is advisory until tested against each included release;
- Foundation major changes require Engine revalidation and normally an Engine major release if behaviour changes;
- backward-compatible SDK validation additions may be minor releases;
- stricter validation that rejects previously conformant packages is breaking;
- patch releases cannot add authority or change declared side effects;
- deprecated Capabilities remain version-addressable during a documented migration window;
- Registry history is append-oriented and must preserve replaced versions.

Ordinary Engine updates must not require Foundation changes.

---

## 21. Engine Package Lifecycle

The Registry statuses `proposed`, `draft`, `validated`, `registered`, `active`, `deprecated`, and `retired` describe an Engine package version. They are not Runtime States and must never appear in State Transition proposals.

Lifecycle rules:

- automated tests may advance `draft` to `validated`;
- governance review advances `validated` to `registered`;
- authorised deployment governance advances `registered` to `active`;
- deprecation requires replacement guidance and a sunset date where known;
- retirement prevents new invocation selection but preserves audit records;
- changing immutable package content creates a new version and returns it to `draft`.

---

## 22. Templates and Examples

SDK implementation should later provide:

- Engine README template;
- Manifest template;
- Capability template;
- combined profile guidance inside the Manifest template;
- positive invocation and successful Result example;
- waiting Result example;
- failure, cancellation, and timeout examples;
- static State Transition proposal example;
- governed dynamic-target example;
- permission-denial example;
- invalid taxonomy, State, object, Decision, and Tool negative fixtures;
- conformance checklist.

Examples must use actual Foundation States, Runtime Objects, Decision types, and canonical Engine names. They must remain non-normative and must identify the Foundation version validated.

---

## 23. Canonical Engine Taxonomy Alignment

SDK v1 recognises exactly:

1. Input Engine
2. Retrieval Engine
3. Business Judgement Engine
4. Planning Engine
5. Execution Engine
6. Quality Engine
7. Learning Engine
8. Memory Engine
9. Exception Engine
10. Approval Engine
11. Policy Engine

Recommended machine `engine_type` values are `input`, `retrieval`, `business_judgement`, `planning`, `execution`, `quality`, `learning`, `memory`, `exception`, `approval`, and `policy`. The Manifest schema must map each value to exactly one canonical name.

No capability, Skill, domain, vendor, model, or implementation label can extend this enum. A new canonical Engine requires a later Foundation governance change outside the SDK.

---

## 24. Writing Capability Placement

“Writing Engine” is not architecturally valid under Foundation v1.0.0.

The correct placement is a multi-Engine workflow profile whose execution operation is a writing Skill:

1. Retrieval Engine assembles authoritative knowledge.
2. Business Judgement Engine determines audience, claim, communication approach, and constraints.
3. Planning Engine creates the Execution Blueprint and selects the writing Skill.
4. Execution Engine invokes the writing Skill and produces the output.
5. Quality Engine verifies accuracy, brand, completeness, compliance, and traceability.
6. Approval and Policy Engines participate when publication or governed claims require them.

Writing is therefore primarily a Skill package used by an Execution Engine Capability, composed through a multi-Engine workflow. It may have supporting capability declarations in Planning and Quality, but it is not an implementation of Business Judgement Engine and not a new canonical Engine.

This model also applies to research, publishing, content production, and review: domain labels describe Skills, Capabilities, or workflows unless their responsibility exactly matches an existing canonical Engine.

---

## 25. First Implementation Recommendation

Implement the Retrieval Engine first after SDK Phase 2.

Reasons:

- it has a clear single responsibility;
- it consumes Task and authorised Engine Context;
- it produces a schema-backed Knowledge Package;
- it exercises provenance, evidence, confidence, and validation;
- it proposes registered retrieval transitions;
- it can exercise waiting and knowledge-incomplete paths;
- it normally has no irreversible external side effects;
- the Foundation Engine Result example already demonstrates a Retrieval invocation;
- its conformance fixtures can be deterministic.

Input Engine is smaller but does not exercise enough of the Context, evidence, and object-production boundaries. Execution Engine is more comprehensive but would force Tool and side-effect design too early.

---

## 26. Security and Governance

SDK validators and adapters must prevent:

| Threat | Control |
|---|---|
| Unregistered Engine authority | Taxonomy and Engine Registry validation |
| Deprecated Engine alias | Exact enum and authority rejection |
| Unregistered Runtime Object | Runtime Object Registry lookup |
| Invalid State proposal | State and transition validation |
| Dynamic-target bypass | Mechanism, Decision, checkpoint, origin, and Orchestrator checks |
| Decision bypass | Required Decision reference and validity checks |
| Approval or policy bypass | Effective-permission intersection and evidence checks |
| Undeclared Tool invocation | Adapter allowlist |
| Undeclared side effect | Capability and permission denial |
| Secret disclosure | Opaque secret provider and redaction |
| Foundation mutation | Read-only pinned dependency and checksum verification |
| Manifest tampering | Immutable version, checksum, and conformance record |

The SDK should fail closed. Validation failure returns a governed failure or blocked Engine Result and appropriate evidence; it must not infer missing authority.

---

## 27. Validation Architecture

The implementation phase should provide one deterministic conformance command that performs:

1. strict JSON parsing;
2. duplicate-key detection;
3. Draft 2020-12 meta-schema validation;
4. canonical-ID-only schema resolution;
5. exact Foundation tag and checksum validation;
6. canonical Engine taxonomy validation;
7. Runtime State and transition validation;
8. dynamic-target mechanism validation;
9. Runtime Object Registry and schema validation;
10. Decision-type and prerequisite validation;
11. Manifest validation;
12. Capability validation;
13. permission and side-effect validation;
14. Tool dependency declaration validation;
15. Engine Result and all example validation;
16. Alpha compatibility consumption tests;
17. positive and negative conformance tests;
18. local Markdown-reference validation;
19. CommonMark fence validation;
20. trailing-whitespace and `git diff --check` validation.

Conformance output should record validator version, exact Foundation tag, SDK version, Engine ID and version, fixture counts, failures, warnings, timestamp, and content checksums.

---

## 28. Recommended File Plan

| Path | Purpose | Status | Format | Owner | Dependencies | SDK v1 |
|---|---|---|---|---|---|---|
| `engines/sdk/README.md` | Navigation, use, authority boundaries | Non-normative guidance | Human | Engine SDK | Foundation, specification | Required |
| `engines/sdk/engine_specification.md` | SDK behavioural contract | Normative | Human | Engine SDK | All Foundation specifications | Required |
| `engines/sdk/schemas/engine_manifest.schema.json` | Manifest validation | Normative | Machine | Engine SDK | Foundation IDs and registries | Required |
| `engines/sdk/schemas/engine_capability.schema.json` | Capability validation | Normative | Machine | Engine SDK | Manifest schema, Foundation registries | Required |
| `engines/sdk/schemas/engine_registry.schema.json` | Engine Registry validation | Normative | Machine | Engine SDK | Manifest and taxonomy | Required |
| `engines/sdk/registry/engine_registry.json` | Canonical implementation index | Normative data | Machine | Engine Layer governance | Registry schema, Manifests | Required |
| `engines/sdk/conformance/README.md` | Conformance-suite behaviour and outputs | Normative procedure | Human | Engine SDK | SDK schemas, Foundation validators | Required |
| `engines/sdk/templates/engine_README.template.md` | Package documentation scaffold | Non-normative | Human | Engine SDK | Specification | Required |
| `engines/sdk/templates/engine_manifest.template.json` | Manifest authoring scaffold | Non-normative | Machine example | Engine SDK | Manifest schema | Required |
| `engines/sdk/templates/engine_capability.template.json` | Capability authoring scaffold | Non-normative | Machine example | Engine SDK | Capability schema | Required |
| `engines/sdk/templates/conformance_checklist.template.md` | Human release checklist | Non-normative | Human | Engine SDK | Conformance procedure | Required |
| `engines/sdk/examples/positive/*` | Valid canonical fixtures | Non-normative evidence | Machine | Engine SDK | SDK and Foundation schemas | Required |
| `engines/sdk/examples/negative/*` | Required rejection fixtures | Non-normative evidence | Machine | Engine SDK | SDK and Foundation schemas | Required |
| `engines/<engine_type>/<engine_id>/README.md` | Implementation documentation | Normative for package use | Human | Engine owner | SDK specification | Phase 3 |
| `engines/<engine_type>/<engine_id>/engine_manifest.json` | Immutable implementation declaration | Normative data | Machine | Engine owner | Manifest schema | Phase 3 |
| `engines/<engine_type>/<engine_id>/capabilities/*.json` | Package-local Capability declarations | Normative data | Machine | Engine owner | Capability schema | Phase 3 |
| `engines/<engine_type>/<engine_id>/tests/*` | Conformance and behaviour tests | Verification | Machine | Engine owner | SDK suite | Phase 3 |
| `engines/<engine_type>/<engine_id>/src/*` | Engine implementation | Executable | Machine | Engine owner | Manifest entrypoint | Phase 3 |

No standalone `engine_profile.schema.json` is recommended for SDK v1. The combined profile belongs in the Manifest schema.

---

## 29. Implementation Phases

### Phase 1 — SDK Contract

Create:

- SDK README;
- Engine specification;
- Manifest schema;
- Capability schema.

Gate:

- ownership review confirms no Foundation duplication;
- taxonomy, Foundation version, State, Object, Decision, and permission declarations are machine-validatable;
- positive and negative schema examples pass;
- no Registry or Engine implementation is activated.

### Phase 2 — Registration and Conformance

Create:

- Engine Registry schema and empty or seed Registry;
- templates;
- conformance procedure;
- positive and negative fixtures;
- deterministic validation command.

Gate:

- one canonical Registry only;
- all Foundation references resolve through the release tag;
- every required rejection case fails for the correct reason;
- Registry cannot add Engine types or grant Runtime Object authority;
- architecture review approves Phase 3.

### Phase 3 — First Engine

Create:

- Retrieval Engine package;
- Manifest and Capability files;
- implementation adapter;
- positive, waiting, incomplete-knowledge, failure, cancellation, timeout, and transition fixtures;
- conformance evidence.

Gate:

- Retrieval Engine is `validated` then governance-approved as `registered`;
- all returned results use `runtime_execution_evidence_v1`;
- all State, Object, Decision, permission, and Tool checks pass;
- Foundation source remains unchanged;
- independent Engine Layer architecture review passes before activation.

---

## 30. Acceptance Criteria

Engine SDK v1 is acceptable when:

- no Foundation definition or ownership is duplicated;
- exactly one Engine Registry exists;
- the Registry accepts only the eleven canonical Engine types;
- deprecated aliases are rejected as machine authorities;
- every Manifest validates strictly without duplicate keys;
- every Capability validates strictly without duplicate keys;
- every package declares an exact tested Foundation dependency;
- every SDK v1 Engine emits the canonical Engine Result with `runtime_execution_evidence_v1`;
- every Runtime State reference resolves and every proposed transition is authorised;
- every Runtime Object reference resolves and authority matches the registry;
- every Decision type and prerequisite resolves to the Decision Model;
- permissions are default-deny and cannot replace approval or policy;
- undeclared Tools and side effects are rejected;
- positive fixtures pass and negative fixtures fail as expected;
- Alpha Engine Results remain consumable but cannot certify a new SDK v1 producer;
- Markdown links and fences pass;
- `git diff --check` passes;
- no Runtime Foundation source file is modified.

---

## 31. Risks and Open Questions

The following questions should be resolved during SDK implementation without changing Foundation v1.0.0:

1. Which implementation language should host the first reference SDK and validator while preserving language-neutral contracts?
2. Should conformance evidence be cryptographically signed in SDK v1 or checksummed first and signed in a later minor release?
3. What repository-wide identifier syntax should `engine_id` use: reverse-domain, URN-like, or controlled slug?
4. Which Tool interface metadata is already available outside the Foundation, and what minimal adapter contract can reference it?
5. Where should deployment-instance selection profiles live if multiple active implementations exist?
6. Should provisional Runtime Objects be completely prohibited in SDK v1 or enabled only through an experimental conformance mode?
7. Which exact Decision-type identifiers should be exposed as machine vocabulary if the Decision schema remains deferred?

None of these questions requires a Foundation modification. If implementation evidence later proves otherwise, the concern must be raised through a separate Foundation compatibility review.

---

## 32. Final Recommendation

Approve the compact SDK architecture for phased implementation:

- one Engine specification;
- three SDK schemas;
- one Engine Registry;
- Manifest-local Capability files;
- one combined Manifest profile;
- strict Foundation-pinned validation;
- Retrieval Engine as the first implementation;
- writing as an Execution Skill inside a governed multi-Engine workflow.

Do not create a Writing Engine, a Capability Registry, a separate Engine Profile schema, a Tool protocol, or any Foundation extension in SDK v1.

This architecture gives future Engines a consistent development and governance contract while keeping `runtime-foundation-v1.0.0` singular, stable, and unchanged.
