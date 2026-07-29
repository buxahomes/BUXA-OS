# Scout Runtime Engine SDK Architecture Decisions

## 1. Document Status

**Document ID:** `SCOUT-RUNTIME-ENGINE-SDK-ARCHITECTURE-DECISIONS`

**Version:** `1.0.0`

**Status:** Accepted

**Owner:** Scout Runtime Engine Layer Governance

**Foundation dependency:** `runtime-foundation-v1.0.0`

**Foundation tag commit:** `2eb52d62d877dd8b5c5736721423dc1b2d5b5d36`

**Design dependency:** `Engine_SDK_Architecture_Design.md` version `1.0.0-design`

These decisions are binding inputs to Engine SDK Phase 1. They do not modify the released Runtime Foundation.

---

## 2. Purpose

This record closes every implementation question identified by the Engine SDK architecture design. Implementation must use these decisions directly. A conflict must stop implementation and be raised through the compatibility process; it must not be resolved by inventing architecture in code or schema.

---

## 3. Relationship to Foundation v1.0.0

The Foundation remains the sole authority for canonical Engines, Runtime States and transitions, dynamic targets, Runtime Objects, Decisions, Engine Context, Engine Result, approval, policy, and the canonical schema namespace. The SDK references these authorities and may narrow implementation behaviour. It cannot expand authority or weaken a Foundation rule.

Historical Alpha and Beta reviews are provenance only. Explicit compatibility retained by `runtime-foundation-v1.0.0` remains authoritative.

---

## 4. Relationship to Engine SDK Architecture Design

`Engine_SDK_Architecture_Design.md` defines the target architecture and phases. This record resolves its open questions and takes precedence for the decisions below. Neither document is part of the frozen Foundation.

---

## 5. Decision Summary

| ADR | Decision |
|---|---|
| ADR-01 | Controlled `scout.engine` identifiers |
| ADR-02 | Controlled `scout.capability` identifiers |
| ADR-03 | Controlled `scout.decision` references plus closed Foundation mapping |
| ADR-04 | Exact Foundation pin for certification |
| ADR-05 | Independent semantic versions for each contract layer |
| ADR-06 | Certified capabilities are separate package-local files |
| ADR-07 | Engine Profile is embedded in the Manifest |
| ADR-08 | Deployment selection remains outside the Manifest |
| ADR-09 | Minimal Tool adapter metadata only |
| ADR-10 | Closed, default-deny permission vocabulary |
| ADR-11 | Five controlled side-effect levels |
| ADR-12 | Certified I/O uses approved Runtime Objects only |
| ADR-13 | Reproducible SHA-256 evidence; signatures deferred |
| ADR-14 | One Engine Registry owned by Engine Layer Governance |
| ADR-15 | Seven-value package lifecycle |
| ADR-16 | Separate five-value certification status |
| ADR-17 | Canonical identifiers, not labels, are authoritative |
| ADR-18 | Writing is not an Engine |
| ADR-19 | Retrieval Engine is the first implementation |
| ADR-20 | Every open question is resolved or registered as deferred |

---

## 6. ADR-01 Engine ID Syntax

`engine_id` must match:

```text
^scout\.engine\.(input|retrieval|business_judgement|planning|execution|quality|learning|memory|exception|approval|policy)\.[a-z][a-z0-9]*(?:_[a-z0-9]+)*$
```

Examples include `scout.engine.retrieval.default`, `scout.engine.retrieval.enterprise_search`, and `scout.engine.execution.content_production`.

Rules:

- lowercase ASCII only;
- no spaces, hyphens, empty segments, or consecutive separators;
- the Engine type must be one of the eleven enum values;
- the implementation name is package-local and stable;
- no version, environment, region, endpoint, replica, or deployment term;
- deprecated reasoning and feedback aliases are rejected;
- `engine_version` is stored separately;
- `engine_id` remains stable across deployments.

Identity is separated as follows:

| Layer | Identifier |
|---|---|
| Canonical Engine type | `engine_type` plus Foundation canonical name |
| Implementation | `engine_id` plus `engine_version` |
| Package release | implementation identity plus immutable checksum |
| Deployment instance | `deployment_instance_id` in deployment configuration |

---

## 7. ADR-02 Capability ID Syntax

`capability_id` must match:

```text
^scout\.capability\.[a-z][a-z0-9]*(?:_[a-z0-9]+)*\.[a-z][a-z0-9]*(?:_[a-z0-9]+)*$
```

Examples include `scout.capability.retrieval.authoritative_search`, `scout.capability.retrieval.source_ranking`, and `scout.capability.execution.longform_drafting`.

The first variable segment is the domain and the second is the capability name. The ID contains no deployment or version term. `capability_version` is separate. A capability ID is immutable for one semantic lineage and grants no Engine authority. A breaking change requires a major capability version, or a new ID when its meaning changes completely.

---

## 8. ADR-03 Decision Type Identifiers

An SDK Decision reference must match:

```text
^scout\.decision\.[a-z][a-z0-9]*(?:_[a-z0-9]+)*$
```

Regex validity is necessary but insufficient. The complete ID must exist in the SDK Decision reference table generated from and reviewed against `shared/decision_model.md`. Unknown IDs fail validation.

Initial specialised references are:

| SDK identifier | Foundation category | Foundation type or governed outcome |
|---|---|---|
| `scout.decision.retry` | `retry_decision` | `retry` |
| `scout.decision.recovery` | `recovery_decision` | `recover` |
| `scout.decision.rollback` | `rollback_decision` | `rollback` |
| `scout.decision.revision` | `quality_decision` | `revise` |
| `scout.decision.approval` | `approval_decision` | canonical approval outcome such as `approve`, `reject`, or `defer` |
| `scout.decision.policy` | `policy_decision` | a canonical type selected under Policy Decision semantics |

Direct references such as `scout.decision.selection` and `scout.decision.commit_transition` may be exposed when they map exactly to canonical `decision_type` values.

These are typed references, not aliases for Foundation fields. The full Decision remains governed by `decision_model.md`. Adding an ID requires an existing Foundation semantic, an explicit mapping, Decision governance review, and SDK schema and fixture updates. SDK-only Decision semantics are prohibited.

---

## 9. ADR-04 Foundation Compatibility

### Certified

A certified Manifest must declare exactly:

```text
foundation_version: runtime-foundation-v1.0.0
```

Its evidence records the exact tag, commit, SDK version, schema versions, test suite, and content hash. Ranges are prohibited.

### Experimental

An experimental package declares one exact tested Foundation version and may add an informational range. The range grants no compatibility and cannot support `conformant` or `certified` status.

### Incompatible

A package is incompatible when its pinned Foundation is unavailable, tag or checksum differs, references fail, or behaviour conflicts with Foundation. It cannot be registered or active.

### Migration

A Foundation change requires compatibility assessment, a new immutable Engine version, updated exact pin, declaration migration, full conformance, new evidence, and renewed registration and activation approval.

---

## 10. ADR-05 SDK Versioning

The following independent semantic versions are required:

- `sdk_version`;
- `engine_version`;
- `manifest_schema_version`;
- `capability_schema_version`;
- `registry_schema_version`;
- `capability_version`;
- `test_suite_version`.

| Change | Classification | Consequence |
|---|---|---|
| Remove or reinterpret required SDK behaviour | Breaking | SDK major, migration, recertification |
| Add optional SDK behaviour | Backward compatible | SDK minor |
| Correct implementation without contract change | Patch | SDK patch; evidence records validator version |
| Reject a previously valid document | Schema breaking | Schema major and migration |
| Add optional schema field | Schema compatible | Schema minor |
| Tighten security after valid prior behaviour | Breaking unless correcting invalid behaviour | Migration and affected recertification |
| Incompatible Engine behaviour | Breaking | Engine major and new certification |
| Backward-compatible Engine capability | Compatible | Engine minor and new release certification |
| Engine defect correction | Patch | Engine patch and new release evidence |
| Incompatible Capability semantics | Breaking | Capability major and consumer migration |
| Documentation only | Documentation | No machine-version change |

One version field must never stand in for another layer.

---

## 11. ADR-06 Capability Reference Model

Certified Manifests reference separate Capability files by `capability_id`, `capability_version`, `capability_file`, and optional content hash.

Inline Capability declarations are allowed only for `experimental` packages. They are rejected for conformance, certification, registration, or activation.

One ID may be implemented by multiple Engines only when its semantics, version, I/O, States, Decisions, permissions, quality, and side effects remain equivalent. Otherwise a different ID is required.

A dependency declares capability ID, compatible version, optional fixed `engine_id`, required or optional status, and failure behaviour. Unbound providers are resolved through Orchestrator or deployment selection. Dependencies grant no invocation authority.

Files remain package-local; SDK v1 has no Capability Registry. A deprecated Capability records date, replacement, support window, and migration guidance and cannot be selected by default for new workflows.

---

## 12. ADR-07 Engine Profile Placement

Engine Profile is embedded in the Manifest. SDK v1 creates no separate Profile schema.

Stable Manifest sections cover execution behaviour, quality, security and permission ceilings, memory boundaries, recovery and checkpoint support, observability, and Tool declarations.

Deployment configuration owns `deployment_instance_id`, environment, region, endpoint, process, queue, replicas, scaling, concrete resources, environment variables, credentials, secret bindings, and provider selection. Deployment may narrow but cannot broaden the Manifest.

---

## 13. ADR-08 Deployment Selection

Deployment selection belongs to Orchestrator or deployment configuration.

| Concept | Authority |
|---|---|
| Canonical Engine type | Foundation Engine Contract |
| Implementation identity | Manifest |
| Package and compatibility record | Engine Registry |
| Active eligibility | Registry lifecycle and certification |
| Default implementation | Deployment selection policy |
| Deployment instance and infrastructure | Deployment configuration |

The Registry may record active eligibility and non-binding priority. Actual selection filters by Engine type, active lifecycle, certified status, exact Foundation compatibility, Capability, permissions, Tools, deployment health, and policy.

Credentials, endpoints, instance IDs, environment variables, and scaling never belong in the Manifest or Registry.

---

## 14. ADR-09 Tool Adapter Metadata

SDK v1 defines only:

| Field | Meaning |
|---|---|
| `tool_id` | Stable Tool integration ID |
| `tool_type` | Controlled adapter category |
| `permissions` | ADR-10 permissions |
| `side_effect_level` | ADR-11 level |
| `input_contract` | Canonical schema ID or adapter contract |
| `output_contract` | Canonical schema ID or adapter contract |
| `timeout` | Maximum duration or profile |
| `retry_policy` | Bounded attempts, retry conditions, backoff, and idempotency |

Dependencies also declare interface version and required or optional status. Only Tools declared by both Manifest and Capability are exposed. Optional absence uses only a declared fallback; required absence blocks or routes through governed exception handling.

Permissions are enforced before invocation. Side effects are recorded through existing Runtime Events and Engine Result evidence. Tool retries cannot exceed Tool, Capability, Engine, policy, or invocation limits. External or irreversible actions require idempotency and approval.

Tool itself is not a Runtime Object. `tool_request` and `tool_result` remain planned Foundation Registry entries with planned schemas. SDK v1 cannot consume, produce, activate, or redefine them. A full Tool protocol is deferred.

Credentials, tokens, keys, and secrets never appear in Manifest, Capability, Registry, Runtime Event, or Engine Result.

---

## 15. ADR-10 Permission Model

The closed enum is:

- `read_internal`;
- `write_internal`;
- `read_restricted`;
- `write_external`;
- `publish_external`;
- `communicate_external`;
- `invoke_tool`;
- `network_access`;
- `repository_modify`;
- `secret_access`;
- `irreversible_side_effect`.

Manifest permissions are ceilings. Capability permissions must be a subset and cannot escalate.

```text
effective permission
= Manifest ceiling
∩ Capability request
∩ deployment availability
∩ Policy Decision
∩ Approval Decision where required
```

Missing permission or evidence denies by default. Restricted and secret access require policy evaluation. Publishing, external communication, and irreversible effects require explicit policy and approval. Repository modification requires task-specific repository authority. Network and Tool permissions are limited to declared destinations and Tool operations.

Declarations express intent and limits; they do not replace policy or approval.

---

## 16. ADR-11 Side-Effect Levels

| Level | Meaning | Minimum control |
|---|---|---|
| `none` | No persistent change outside invocation-local computation | Ordinary bounded retry |
| `internal_reversible` | Internal change with tested inverse | Policy, audit, checkpoint or rollback evidence |
| `internal_persistent` | Durable internal change not automatically reversible | Policy, audit, idempotency, explicit retry |
| `external_reversible` | External change with verified compensation | Policy and applicable approval, audit, idempotency, compensation |
| `external_irreversible` | External change without reliable reversal | Explicit Policy and Approval Decisions, full audit, strict idempotency, no blind retry |

Free text is prohibited. Every Capability declares one level. Effective level is the highest level across the Capability and invoked Tools. Uncertain persistent or external completion must route through governed retry, recovery, rollback, or manual handling rather than automatic replay.

---

## 17. ADR-12 Provisional Runtime Object Policy

| Registry status | Certified consume | Certified produce | Experimental |
|---|---|---|---|
| `approved` | Allowed when authority and schema pass | Allowed when producer authority and schema pass | Allowed |
| `provisional` | Prohibited by default | Prohibited | Reference only under explicit experimental policy |
| `planned` | Prohibited | Prohibited | Prohibited until promoted and contractually available |
| `deprecated` | Audit or migration only | Prohibited for new output | Explicit migration policy |
| conflict or unknown | Fail closed | Fail closed | Fail closed |

Provisional reference requires `experimental` certification, non-active lifecycle, Manifest declaration, Policy Decision, Approval Decision where required, isolation, exclusion from certified output, and no authoritative persistence unless promoted.

Registry status, schema status, owner, and authority must agree. Conflict blocks registration and activation.

Current Foundation compatibility consequence: `policy_decision` and `checkpoint` are provisional. A certified Policy Engine cannot produce Policy Decision, and a certified Capability cannot depend on Checkpoint as a Runtime Object, until the applicable object is promoted through separate Foundation governance. This does not block SDK Phase 1 or the first Retrieval Engine implementation. The SDK must report the restriction; it must not waive it.

---

## 18. ADR-13 Conformance Evidence

Evidence records deterministic content hash, `hash_algorithm` (default `sha256`), timestamp, validator ID and version, Foundation tag and commit, SDK and schema versions, test-suite version, Engine ID and version, result, fixture summary, input checksums, warnings, and failures.

Evidence is immutable after certification. Revalidation creates a new record. Reproducibility requires canonical ordering, deterministic JSON serialisation, recorded dependencies and fixtures, and controlled network behaviour.

Unsigned evidence may certify SDK v1 only inside the trusted repository workflow. Digital signatures are deferred. A future signed profile wraps or references the unchanged v1 hash and adds signer, algorithm, signature, time, and trust chain, so v1 records remain valid.

---

## 19. ADR-14 Engine Registry Ownership

One canonical Registry is approved:

- Registry: `12_AI_Agents/Scout/Runtime/engines/sdk/registry/engine_registry.json`;
- schema: `12_AI_Agents/Scout/Runtime/engines/sdk/schemas/engine_registry.schema.json`;
- owner: Scout Runtime Engine Layer Governance;
- human contract: future `engines/sdk/engine_specification.md`.

It indexes multiple implementations under only the eleven Foundation types and records Engine identity and version, Manifest checksum, exact compatibility, lifecycle, certification and evidence, activation eligibility, optional selection priority, Capabilities, and deprecation.

The accepted types map exclusively to Input Engine, Retrieval Engine, Business Judgement Engine, Planning Engine, Execution Engine, Quality Engine, Learning Engine, Memory Engine, Exception Engine, Approval Engine, and Policy Engine.

It does not select deployment instances, store secrets, grant Runtime Object authority, or create Engine types. Reasoning Engine, Feedback Engine, Writing Engine, and unknown aliases are rejected.

---

## 20. ADR-15 Engine Package Lifecycle

The Registry owns:

```text
proposed → draft → validated → registered → active → deprecated → retired
```

Normal transitions follow the arrow. `validated` may return to `draft` if validation becomes stale. `registered` may become `deprecated` before activation. Any non-retired version may be retired immediately for critical security or governance cause.

Content change creates a new Engine version in `draft`; immutable releases do not move backward. Activation requires certified status, exact compatibility, valid Registry entry, dependencies, and deployment approval.

Deprecation requires reason, date, replacement where available, support window, and migration. Retirement prevents new selection but preserves Manifest, evidence, Registry history, and audit references.

These are package values, not Runtime States.

---

## 21. ADR-16 Certification Status

The separate certification enum is:

- `experimental`: intentionally non-certifiable and isolated;
- `conformant`: automated checks pass for recorded inputs;
- `certified`: conformant evidence accepted by Engine Layer Governance;
- `deprecated`: historically valid certification is no longer preferred;
- `revoked`: evidence or approval invalid; activation prohibited.

```text
experimental → conformant → certified → deprecated
                              ↓
                           revoked
```

Conformant may return to experimental when evidence becomes stale. Any status may be revoked.

Package lifecycle describes repository and selection maturity. Certification describes evidence acceptance. Registry presence describes discoverability. Engine invocation status describes one invocation. Runtime State describes workflow position. The Registry stores lifecycle and certification separately and rejects `active` without `certified` or `retired` with activation eligibility.

---

## 22. ADR-17 Canonical Reference Language

Machine references use `engine_id` and `engine_type`, `capability_id`, Runtime Object `object_type`, canonical Runtime State name, mapped ADR-03 Decision ID, canonical schema `$id`, exact Foundation tag, and exact semantic versions.

Display labels may accompany IDs but are non-authoritative. Validators compare exact identifiers and do not normalise aliases, case, or whitespace.

SDK schema IDs use `https://buxa.ai/scout/runtime/engines/sdk/schemas/`. Foundation references use released canonical IDs. File paths are packaging references only.

---

## 23. ADR-18 Writing Capability Placement

Writing is not a canonical Engine. It is an Execution Skill or package-local Execution Capability invoked through an Execution Blueprint in a governed multi-Engine workflow.

Retrieval provides knowledge; Business Judgement selects claims and approach; Planning creates the Blueprint; Execution invokes writing; Quality validates output; Approval and Policy participate for governed external use.

Styles, prompts, templates, brand rules, and domain knowledge belong in the Skill package, Capability configuration, authorised Engine Context, or Knowledge Package. They cannot expand Engine taxonomy.

---

## 24. ADR-19 First Implementation Target

Retrieval Engine is the first implementation after SDK Phases 1 and 2.

It exercises Manifest, Capability files, Registry, Task and Engine Context input, Knowledge Package output, optional Tools, read permissions, controlled network use, waiting and retry, cancellation and timeout, Decision and transition references, `runtime_execution_evidence_v1`, provenance, confidence, quality, and conformance without forcing irreversible effects.

No Retrieval Engine implementation is created by this ADR.

---

## 25. ADR-20 Open-Question Closure

| Design question | Classification | Resolution |
|---|---|---|
| Reference implementation language | Deferred to SDK implementation | Language-neutral contracts remain fixed |
| Signed conformance evidence | Resolved | SHA-256 in v1; future signature wrapper |
| Engine ID syntax | Resolved | ADR-01 |
| Tool metadata | Resolved | ADR-09 |
| Deployment selection | Deferred to Orchestrator design | Boundary fixed by ADR-08 |
| Provisional Runtime Objects | Resolved | ADR-12 |
| Decision identifiers | Resolved | ADR-03 |
| Certified use of provisional Policy Decision and Checkpoint | Requires Foundation change before affected activation | ADR-12 remains fail-closed |

No Foundation change is required for SDK Phase 1 or the Retrieval first implementation. Promotion of provisional dependencies is required before certification and activation of affected later Engines or Capabilities.

---

## 26. Implementation Constraints

Implementation must keep Foundation read-only, implement exact regexes and enums, use Draft 2020-12 and canonical IDs, maintain one Registry, keep Capabilities package-local, embed Profile in Manifest, separate deployment configuration, reject aliases, require `runtime_execution_evidence_v1`, and fail closed on conflict.

It must create no Foundation State, Object, Decision, Engine, schema alias, or dynamic-target mechanism.

---

## 27. SDK Phase 1 Inputs

Phase 1 is supplied with the accepted design, this ADR, frozen tag and commit, canonical Engine mapping, identifier rules, compatibility model, version fields, Capability reference model, Profile placement, permissions, side-effect levels, approved-only certified object policy, namespace, and evidence model.

Phase 1 outputs are limited to SDK README, Engine specification, Manifest schema, and Capability schema. Implementers may not introduce unresolved architecture into them.

---

## 28. Deferred Work Register

| Item | Owner | Trigger | Earliest phase | Blocks SDK v1 | Expected output |
|---|---|---|---|---|---|
| Reference implementation language | Engine SDK technical owner | Before executable validator | Phase 1 | No for schemas; yes for validator | `Engine_SDK_Reference_Implementation_Decision.md` |
| Deployment selection policy | Runtime Orchestrator owner | Multiple eligible implementations | Orchestrator design / Phase 3 | No | `Engine_Deployment_Selection_Design.md` |
| Full Tool protocol | Tool architecture owner | Standard Tool exchange required | Separate Tool design, no earlier than Phase 2 | No | `Tool_Contract_Architecture_Design.md` |
| Tool Request and Tool Result activation | Foundation governance | Tool design requires planned objects | After Tool design | No | Separate Foundation compatibility review |
| Signed evidence | Engine Layer governance and security | External distribution or cross-repository trust | Post-v1 minor | No | `Engine_Conformance_Signature_Profile.md` |
| Deployment configuration schema | Orchestrator and deployment owners | First deployable Engine | Phase 3 | No for SDK; yes for activation | `Engine_Deployment_Configuration_Design.md` |
| Policy Decision promotion | Foundation governance | Before certified Policy Engine output | Before Policy Engine certification | No for SDK v1; yes for Policy Engine activation | Separate Foundation compatibility review and authoritative schema decision |
| Checkpoint promotion | Foundation governance | Before certified checkpoint-dependent Capability | Before affected Engine certification | No for SDK v1; yes for affected activation | Separate Foundation compatibility review and authoritative schema decision |

Deferred items must not be silently implemented in Phase 1.

---

## 29. Validation Requirements

Implementation and Engine packages must validate strict JSON, duplicate keys, Draft 2020-12, canonical-ID-only resolution, exact Foundation tag, identifier regexes and mapping tables, Engine taxonomy, Runtime State and transitions, dynamic targets, Runtime Object status and authority, Decision mappings, Capability dependencies, Profile, permissions, side effects, Tool declarations, Engine Result, positive and negative fixtures, evidence reproducibility, Markdown references and fences, trailing whitespace, and `git diff --check`.

---

## 30. Acceptance Criteria

This ADR is complete when all twenty decisions are explicit; regexes are exact; Decision IDs require closed mapping; certified compatibility is pinned; versions remain separate; certified Capabilities use files; Profile placement is singular; deployment is external; Tool metadata is minimal; permissions and side effects are closed enums; provisional and planned objects cannot enter certified output; evidence is reproducible; one Registry is fixed; lifecycle and certification are distinct; writing is excluded from taxonomy; Retrieval is first; deferrals have owners and triggers; and no Foundation file changes.

---

## 31. Final Decision

These decisions are accepted as the complete architecture input for Engine SDK Phase 1.

SDK Phase 1 is ready to begin. Implementers must not invent alternate identifiers, statuses, compatibility, permissions, Registry ownership, Capability or Profile placement, Tool contracts, or Engine types.

The Foundation remains frozen at `runtime-foundation-v1.0.0`. Future incompatibility must be raised separately.
