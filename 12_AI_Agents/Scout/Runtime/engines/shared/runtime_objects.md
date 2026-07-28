# Scout Runtime Objects

**Document ID:** `SCOUT-RUNTIME-OBJECT-REGISTRY`
**Version:** `1.0.0`
**Status:** `Approved`
**Owner:** `Scout Runtime`
**Applies To:** Runtime Orchestrator, all registered Runtime Engines, Runtime schemas, Skills, Tools, workflows, and Runtime documentation
**Machine Registry:** `../../registry/runtime_objects.json`
**Validation Schema:** `../../schemas/runtime_object_registry.schema.json`
**Specification Type:** Mandatory Shared Foundation Standard

---

## 1. Purpose

This document defines the governance of recognised Scout Runtime Objects.

It establishes one canonical name, one canonical `object_type`, one normative owner, an explicit schema status, and explicit producer, consumer, lifecycle, containment, reference, integrity, and compatibility rules for every registered object.

The complete machine-readable registry is `../../registry/runtime_objects.json`. This document defines governance and provides a concise registry view; it does not duplicate every machine-readable field.

---

## 2. Scope

This specification governs:

- registration and naming of Runtime Objects;
- distinction between objects, envelopes, states, statuses, decisions, events, records, reports, snapshots, checkpoints, configuration, and references;
- ownership and schema authority;
- producer and consumer relationships;
- identity, versioning, mutability, retention, integrity, compatibility, aliases, and deprecation;
- conflicts and deferred registration decisions.

This specification does not redefine the internal semantics owned by `engine_contract.md`, `engine_context.md`, `state_model.md`, `decision_model.md`, or an authoritative Runtime schema.

---

## 3. Normative Language

- **MUST / SHALL** — mandatory requirement.
- **MUST NOT / SHALL NOT** — prohibited behaviour.
- **SHOULD** — recommended unless a documented exception exists.
- **SHOULD NOT** — discouraged unless justified.
- **MAY** — optional behaviour.
- **REQUIRED** — mandatory field, rule, or control.

A mandatory-rule exception requires documented authority, scope, reason, approval, audit evidence, and review or expiry date.

---

## 4. Registry Authority

`runtime_objects.md` is the normative governance authority for Scout Runtime Object registration.

`../../registry/runtime_objects.json` is the complete machine-readable authority for registered entries.

`../../schemas/runtime_object_registry.schema.json` is the machine validation authority for the registry file.

A Runtime specification, schema, Engine, Skill, Tool, or workflow MUST NOT create, rename, alias, merge, split, or redefine a Runtime Object without an approved registry amendment.

Where a registered object's normative owner or authoritative schema defines specialised semantics, that source remains authoritative for those semantics. Registry ownership identifies authority; it does not copy or replace it.

---

## 5. Runtime Object Definition

A Runtime Object is a governed, identifiable unit of Runtime information that has:

- one canonical name and `object_type`;
- an explicit meaning and classification;
- a normative owner;
- a producer and one or more authorised consumers where applicable;
- a defined lifecycle role;
- explicit identity, version, status, mutability, integrity, retention, and compatibility treatment;
- zero or one authoritative schema.

A recognised entry may be approved without a standalone schema when its normative specification defines it as an embedded payload, control record, envelope, event, reference, or snapshot. Missing schemas MUST be declared `planned`, `deferred`, or `not_applicable`.

---

## 6. Non-Object Categories

The following concepts are not automatically Runtime Objects:

- **Runtime State** is a registered control object; an individual state name is not a separate object.
- **Status** is a field value belonging to an object or envelope and is not an object.
- **Decision Context** is an authorised selection from Engine Context and is not a separate Runtime context object.
- **Configuration** controls Runtime behaviour and is not a Runtime Object unless a future registered object contract explicitly establishes one.
- **Schema identity** identifies a machine contract and is not object identity.
- **Reference** identifies another object and does not imply containment.
- **Log line** is observability data and is not automatically a Runtime Event or Exception Record.
- **Embedded payload** is not a separate object unless it carries a registered identity and conforms to its own normative contract.

Security checks and security-related Decisions are governed through the Decision and control models. No standalone **Security Decision** object is currently registered.

---

## 7. Object Taxonomy

The controlled primary classifications are:

- `input_object`
- `knowledge_object`
- `judgement_object`
- `planning_object`
- `execution_object`
- `quality_object`
- `learning_object`
- `memory_object`
- `exception_object`
- `governance_object`
- `control_object`
- `context_envelope`
- `invocation_envelope`
- `event_object`
- `snapshot_object`
- `reference_object`

Every entry MUST use exactly one primary classification. Relationships MAY connect objects across classifications but MUST NOT change their primary meaning.

---

## 8. Naming Rules

Each object MUST have one lowercase snake-case `object_type` and one canonical title-case name.

Aliases:

- MUST be explicitly registered;
- MUST NOT override the canonical name;
- MUST NOT imply equivalence between distinct objects;
- SHOULD exist only for retrieval or controlled migration.

Prohibited aliases identify terms that would create ambiguity or false equivalence. Similar names MUST NOT be treated as interchangeable.

---

## 9. Ownership Rules

Each object MUST have one normative owner path.

The normative owner defines meaning and governance. The producing authority creates an instance within delegated authority. A consumer may read or act on an instance but does not acquire ownership.

Schema ownership and document ownership are distinct:

- the normative document owns semantic governance;
- an authoritative schema owns machine validation;
- the registry records both without merging them.

Ownership changes require compatibility review and registry amendment.

---

## 10. Schema Authority

Each entry MUST declare one `schema_status`:

- `authoritative` — the non-null schema path is the machine authority;
- `planned` — repository evidence identifies a future schema, but it does not yet exist;
- `deferred` — a schema decision is intentionally unresolved;
- `not_applicable` — the current architecture uses an embedded or distributed representation and does not require a standalone schema.

An entry MUST NOT cite a nonexistent schema as authoritative. A schema filename MUST NOT be inferred from an object name.

---

## 11. Identity and Versioning

Object identity and schema identity are separate.

- `identity_field` identifies an object instance.
- `version_field` identifies object revision or applicable schema/specification version.
- `status_field` identifies lifecycle or invocation status where applicable.
- A reference MUST preserve canonical `object_type`, identity, and version information required by the normative owner.
- A breaking semantic or identity change requires a new version, migration guidance, compatibility review, and registry amendment.

Null fields mean the current source does not define the value; null MUST NOT be replaced with an invented convention.

---

## 12. Mutability

Registry entries use these controlled mutability values:

- `immutable`
- `append_only`
- `versioned`
- `controlled_lifecycle`
- `immutable_snapshot`

Mutation MUST follow the normative owner's authority rules. Immutable history, snapshots, Decisions, transitions, events, and records MUST NOT be silently overwritten.

---

## 13. Lifecycle

Every entry declares one primary lifecycle stage:

`intake`, `context_assembly`, `retrieval`, `judgement`, `planning`, `execution`, `quality`, `governance`, `exception`, `learning`, `memory`, `control`, `observability`, or `cross_lifecycle`.

Lifecycle role does not create Runtime State. Canonical Runtime State names, transitions, categories, and terminal semantics remain owned exclusively by `state_model.md`.

---

## 14. Producer and Consumer Model

Every producer and consumer MUST be a registered Engine, the Runtime Orchestrator, or Authorised Human Authority.

Production authority does not grant:

- schema ownership;
- State commit authority;
- approval authority beyond policy;
- authority to mutate another owner's object;
- authority to create unregistered object types.

Exception Handling and Human Governance are frameworks, not Engines, and therefore are not registered producing authorities.

---

## 15. Containment and Reference Model

Containment and reference are different relationships.

- Containment MUST be explicit in the containing schema or normative specification.
- A reference MUST NOT be interpreted as an embedded copy.
- A containing envelope MUST NOT make a referenced object interchangeable with the envelope.
- An embedded registered object retains its own identity, schema, ownership, lifecycle, and integrity requirements.
- A list of related objects is architectural linkage, not containment.

Engine Result may reference or embed a produced Runtime Object through `primary_output`. The produced object remains distinct from the invocation envelope.

Invocation and lifecycle ownership is distributed without overlap:

| Concept | Normative Owner | Machine Authority |
|---|---|---|
| Engine Invocation Status | `engine_contract.md` | `../../schemas/engine_result.schema.json` |
| Engine Result | `engine_contract.md` | `../../schemas/engine_result.schema.json` |
| Runtime State | `state_model.md` | State schema deferred; State Model remains normative |
| State Transition | `state_model.md` | Transition schema deferred; State Model remains normative |
| Runtime Event | `engine_contract.md` | Event schema deferred |
| Decision | `decision_model.md` | Decision schema deferred |

---

## 16. State and Status Boundary

Runtime State, object status, Decision status, Context status, Engine invocation status, validation status, approval status, freshness status, and integrity status are distinct domains.

- Runtime State is owned by `state_model.md`.
- Engine Result `status` is the final disposition of one Engine invocation and is owned by `engine_contract.md`.
- Object status belongs only to the object whose contract defines it.
- A status value MUST NOT be used as a State Transition target unless it is separately registered by the State Model as a canonical state.

The canonical Engine Invocation Status enum is `waiting`, `succeeded`, `partial_success`, `failed`, `blocked`, `cancelled`, and `timeout`. All seven values are final for one `invocation_id`; workflow resumability is governed separately.

`ready` and `running` are invocation lifecycle phases represented through Runtime Events, not Engine Result statuses. `success` is an invalid alias for `succeeded`. Runtime State `timed_out` MUST NOT replace invocation status `timeout`.

Similar spelling does not create equivalence. For example, Engine Result `status: cancelled` may support a proposal to Runtime State `cancelled`, but only the Runtime Orchestrator may validate and commit that transition.

The registry does not resolve existing State Model findings H-03 or H-04.

---

## 17. Decision Boundary

Decision and Business Judgement are distinct:

- **Decision** is a governed structured selection or outcome defined by `decision_model.md`.
- **Business Judgement** is a schema-backed judgement object that may contain or reference a Decision.

Approval Decision and Policy Decision are specialised Decisions. A Decision may be embedded in an owning Runtime Object, referenced by Engine Result, linked to a State Transition, or retained in audit history. Those relationships do not make the containing object a Decision.

---

## 18. Context Boundary

Engine Context is the sole Runtime context envelope.

Decision Context is not a separate Runtime Object. It is the authorised information selected from Engine Context for Decision formation.

Context Snapshot is an immutable snapshot of a delivered Engine Context. Context Reference identifies an external object without implying containment. Neither is interchangeable with Engine Context.

---

## 19. Envelope Boundary

Engine Result and Execution Result are distinct:

- **Engine Result** is the invocation envelope returned by every Engine and validated by `../../schemas/engine_result.schema.json`.
- **Execution Result** is the Runtime Object produced by the Execution Engine and validated by `../../schemas/execution_result.schema.json`.

An Engine Result MAY reference or embed an Execution Result. An Execution Result MUST NOT replace the Engine Result envelope.

Canonical interaction:

```text
Invocation lifecycle
└── observed through Runtime Events
        ↓
Engine Result
├── final Engine Invocation Status
├── produced Runtime Object or reference
├── Decision reference
├── failure, recovery, and Exception Record reference where required
└── optional State Transition proposal or reference
        ↓
Runtime Orchestrator validation
        ↓
Committed State Transition
        ↓
Runtime State
```

An Engine Result status MUST NOT commit or guarantee its proposed Runtime State. `succeeded` MUST NOT imply workflow completion, and `failed`, `blocked`, `cancelled`, or `timeout` MUST NOT automatically imply a terminal Runtime State.

---

## 20. Evidence and Integrity

Every registered object MUST preserve the provenance, evidence, checksum, validation, append-only history, or other integrity controls required by its normative owner.

Registry validation MUST verify:

- strict JSON and duplicate-key rejection;
- schema conformance;
- unique `object_type` and `canonical_name`;
- existing non-null schema and normative-owner paths;
- resolved `related_objects`;
- recognised producer and consumer authorities;
- metadata counts.

A schema-valid registry is not sufficient if these semantic checks fail.

---

## 21. Compatibility

Compatibility is evaluated across object identity, semantic meaning, owner version, schema version, producer/consumer contracts, references, containment, lifecycle, and aliases.

Breaking changes require:

1. documented reason;
2. affected-object analysis;
3. schema and specification review;
4. migration plan;
5. registry amendment;
6. validation updates;
7. explicit approval.

---

## 22. Deprecation and Alias Rules

Deprecation MUST preserve historical identity and relationships.

A deprecated entry MUST identify:

- replacement object where applicable;
- effective date;
- migration guidance;
- compatibility period;
- historical retention requirements.

Aliases MUST NOT conceal deprecation, conflict, or semantic change.

---

## 23. Registration Process

A proposed object follows:

1. search the registry and authoritative sources;
2. demonstrate a unique semantic need;
3. select one classification and canonical name;
4. identify the normative owner;
5. identify producer, consumers, lifecycle, identity, version, status, and mutability;
6. determine schema status;
7. define containment, references, integrity, retention, and compatibility;
8. identify aliases and conflicts;
9. update the normative document, machine registry, and validation schema where required;
10. validate and obtain approval.

An object MUST NOT enter production while its registry status is `planned` or `conflict`.

---

## 24. Amendment Process

Every amendment MUST include:

- change summary and rationale;
- affected entries and relationships;
- source evidence;
- compatibility and migration analysis;
- schema impact;
- validation results;
- owner and approval;
- effective date.

Machine registry and human-readable registry views MUST be updated together.

---

## 25. Validation

The registry MUST validate against `../../schemas/runtime_object_registry.schema.json`.

JSON Schema validation is supplemented by semantic validation for uniqueness, file existence, authority recognition, relationship resolution, deterministic ordering, and metadata counts.

Validation failures MUST block registry release.

---

## 26. Governance

People retain final authority over Runtime architecture.

The Runtime Orchestrator and Engines MUST use only registered objects within their approved capabilities.

The registry MUST NOT:

- override Business Operating System governance;
- redefine State, Context, Decision, or Engine contracts;
- infer approval from registration;
- treat provisional or planned objects as approved production contracts;
- silently resolve conflicts.

---

## 27. Canonical Registry

| Object Type | Canonical Name | Class | Owner | Schema Status | Registry Status |
|---|---|---|---|---|---|
| `task` | Task | input | `task.schema.json` | authoritative | approved |
| `engine_context` | Engine Context | context envelope | `engine_context.md` | deferred | approved |
| `engine_result` | Engine Result | invocation envelope | `engine_contract.md` | authoritative | approved |
| `knowledge_package` | Knowledge Package | knowledge | `knowledge_package.schema.json` | authoritative | approved |
| `business_judgement` | Business Judgement | judgement | `business_judgement.schema.json` | authoritative | approved |
| `execution_blueprint` | Execution Blueprint | planning | `execution_blueprint.schema.json` | authoritative | approved |
| `skill_output` | Skill Output | execution | `skill_output.schema.json` | authoritative | approved |
| `quality_report` | Quality Report | quality | `quality_report.schema.json` | authoritative | approved |
| `execution_result` | Execution Result | execution | `execution_result.schema.json` | authoritative | approved |
| `learning_candidate` | Learning Candidate | learning | `learning_candidate.schema.json` | authoritative | approved |
| `memory_record` | Memory Record | memory | `memory_record.schema.json` | authoritative | approved |
| `exception_record` | Exception Record | exception | `exception_record.schema.json` | authoritative | approved |
| `decision` | Decision | governance | `decision_model.md` | not applicable | approved |
| `approval_decision` | Approval Decision | governance | `decision_model.md` | deferred | conflict |
| `approval_record` | Approval Record | governance | `engine_context.md` | deferred | conflict |
| `runtime_state` | Runtime State | control | `state_model.md` | deferred | approved |
| `state_transition` | State Transition | control | `state_model.md` | deferred | approved |
| `state_snapshot` | State Snapshot | snapshot | `state_model.md` | deferred | approved |
| `checkpoint` | Checkpoint | snapshot | `state_model.md` | deferred | provisional |
| `policy_decision` | Policy Decision | governance | `decision_model.md` | deferred | provisional |
| `runtime_event` | Runtime Event | event | `engine_contract.md` | deferred | approved |
| `context_snapshot` | Context Snapshot | snapshot | `engine_context.md` | deferred | approved |
| `decision_snapshot` | Decision Snapshot | snapshot | `decision_model.md` | deferred | provisional |
| `context_reference` | Context Reference | reference | `engine_context.md` | deferred | provisional |
| `approval_request` | Approval Request | governance | `Runtime/README.md` | planned | planned |
| `feedback_record` | Feedback Record | learning | `Runtime/README.md` | planned | provisional |
| `execution_record` | Execution Record | execution | `Runtime/README.md` | planned | provisional |
| `tool_request` | Tool Request | execution | `schemas/README.md` | planned | planned |
| `tool_result` | Tool Result | execution | `schemas/README.md` | planned | planned |

The table is a concise view. `../../registry/runtime_objects.json` is authoritative for complete fields and relationships.

---

## 28. Registry Conflicts and Deferred Decisions

### 28.1 Approval Decision and Approval Record

`engine_contract.md` and `decision_model.md` use **Approval Decision**. `engine_context.md` lists **Approval Record**. The repository does not define whether Approval Record contains, persists, supersedes, or merely references Approval Decision.

Both entries are registered with `conflict` status. Neither term may be used as an alias for the other until an approved architecture decision resolves identity, schema, ownership, containment, and lifecycle.

### 28.2 Approval Request

Approval Request appears as a possible Core object and future schema. It remains `planned`; its identity and relationship to the two approval entries require definition.

### 28.3 Feedback Record and Learning Candidate

Feedback Record is recognised by Runtime navigation but lacks a schema or complete boundary. Learning Candidate remains the approved schema-backed learning proposal. Feedback Record MUST NOT be used as an alias for Learning Candidate.

### 28.4 Execution Record and Execution Result

Execution Record is named as historical Core/audit data but lacks a complete contract. Execution Result remains the approved schema-backed execution outcome. Neither is interchangeable with Engine Result.

### 28.5 Policy Decision

Policy Decision semantics are defined, but no standalone schema or definitive containment contract exists. It remains provisional and MUST be represented according to the Decision Model.

### 28.6 Snapshot and Checkpoint Boundaries

Context Snapshot, State Snapshot, Decision Snapshot, and Checkpoint have different subjects. Similar lifecycle use does not make them interchangeable. Decision Snapshot and Checkpoint remain provisional pending standalone schema decisions.

### 28.7 Security Decision

Security checks and security guard Decisions are supported, but no source defines a standalone Security Decision Runtime Object. No entry is registered. A future proposal must demonstrate a unique need and Decision Model relationship.

### 28.8 Planned Tool Objects

Tool Request and Tool Result are future schema candidates only. They remain planned and MUST NOT be treated as approved production objects.

---

## 29. Acceptance Criteria

The registry is acceptable when:

- every entry has all required machine fields;
- canonical names and object types are unique;
- classifications, statuses, mutability, lifecycle stages, and authorities use controlled values;
- all non-null schema and owner paths exist;
- all related-object references resolve;
- conflicts remain explicit;
- the machine registry validates against its schema;
- human and machine views are consistent;
- no source contract is silently renamed or redefined.

---

## 30. Prohibited Behaviours

The following are prohibited:

- inventing an unregistered Runtime Object;
- using an alias as a canonical type;
- treating similar names as interchangeable;
- treating a reference as containment;
- treating an envelope as its carried object;
- using Engine invocation status as Runtime State;
- using Decision Context as a second Context object;
- treating Business Judgement as generic Decision;
- treating Approval Decision and Approval Record as resolved;
- citing a missing schema as authoritative;
- changing object meaning without versioning and registry amendment;
- deleting deprecated or conflicting identity history;
- allowing schema-valid but semantically invalid registry data into production.

---

## 31. Summary

Scout Runtime uses one governed object vocabulary.

One canonical name.

One canonical `object_type`.

One normative owner.

Zero or one authoritative schema.

Explicit producers, consumers, lifecycle, containment, references, integrity, and compatibility.

The registry protects Runtime architecture from silent invention, duplication, renaming, and semantic drift.
