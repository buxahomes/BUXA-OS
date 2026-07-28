# Engine Context

**Document ID:** `SCOUT-RUNTIME-ENGINE-CONTEXT`  
**Version:** `1.0.0`  
**Status:** `Approved`  
**Owner:** `Scout Runtime`  
**Applies To:** All Runtime Engines  
**Contract Dependency:** `engine_contract.md`  
**Specification Type:** Mandatory Shared Foundation Standard  

---

## 1. Purpose

This document defines the universal **Engine Context** used by every Scout Runtime Engine.

The Engine Context is the only authorised execution envelope delivered to an Engine. It provides a governed, immutable, versioned, traceable, and replayable view of the Runtime world required for one Engine invocation.

It standardises context identity, receiving Engine identity, execution identity, Task information, Runtime State, visible Runtime Objects, Knowledge Package access, Memory Record access, configuration, policy, approval, security, privacy, actor, environment, resource limits, observability, references, snapshots, freshness, integrity, priority, evolution, and visibility.

Every Runtime Engine MUST receive exactly one Engine Context.

No Engine may require hidden parameters, undocumented prompt state, unregistered global state, or implicit conversational memory outside the Engine Context.

---

## 2. Scope

This specification applies to the Retrieval, Business Judgement, Planning, Execution, Quality, Exception, Learning, Memory, Approval, Policy, and Orchestration Engines, together with all future Engines registered in the Engine Registry.

It defines the logical content and governance of Engine Context. It does not prescribe the physical database, transport protocol, model provider, implementation language, or storage engine.

---

## 3. Normative Language

- **MUST / SHALL** — mandatory.
- **MUST NOT / SHALL NOT** — prohibited.
- **SHOULD** — recommended unless a documented exception exists.
- **SHOULD NOT** — discouraged unless justified.
- **MAY** — optional.
- **REQUIRED** — mandatory field, control, state, or action.

Any deviation from a MUST or MUST NOT requirement requires a documented exception, owner, explicit approval, review or expiry date, audit record, and compatibility assessment.

---

## 4. Design Goals

Engine Context is designed to be:

- immutable;
- complete but minimal;
- traceable;
- governed;
- serializable;
- replayable;
- versioned;
- composable;
- reference-aware;
- integrity-protected;
- freshness-aware;
- observable;
- Engine-specific.

---

## 5. Design Principles

### 5.1 Single Authorised View

From an Engine's perspective, Engine Context is the authoritative execution view for that invocation.

The Engine MUST NOT rely on hidden prompt memory, undocumented conversational state, global mutable variables, private cross-Engine memory, unregistered caches as authority, undeclared environment variables, or unstated assumptions about previous executions.

### 5.2 Immutability

Engine Context is read-only. All proposed changes MUST be returned through Engine Result.

### 5.3 Least Privilege

The Context Builder MUST expose only the information the receiving Engine is authorised and required to use.

### 5.4 Explicit Absence

Critical missing information MUST be represented explicitly and MUST NOT be replaced with guessed defaults.

### 5.5 Evidence-Bound Context

Authoritative claims MUST be linked to evidence, policy, approved memory, configuration, or controlling Runtime Objects.

### 5.6 Reference Before Duplication

Large, immutable, repeated, or sensitive objects SHOULD be referenced rather than copied.

### 5.7 Context Is Not Memory

Engine Context may contain Memory Records, but it is not itself long-term memory.

### 5.8 Context Is Not Global State

Engine Context is a point-in-time view. Runtime Store and Runtime Orchestrator remain authoritative for committed global state.

---

## 6. Context Architecture

```text
Engine Context
├── Context Metadata
├── Receiving Engine
├── Execution Identity
├── Runtime State
├── Task Context
├── Runtime Objects
├── Knowledge Layer
├── Memory Layer
├── Configuration Layer
├── Policy Layer
├── Approval Layer
├── Security Layer
├── Privacy Layer
├── Actor Context
├── Environment
├── Resource Limits
├── Observability Context
├── References
├── Freshness
├── Integrity
└── Extensions
```

Canonical logical structure:

```json
{
  "context_id": "ctx_01",
  "schema_name": "engine_context",
  "schema_version": "1.0.0",
  "context_version": 1,
  "created_at": "2026-07-28T10:00:00Z",
  "receiving_engine": {},
  "execution": {},
  "runtime_state": {},
  "task": {},
  "runtime_objects": {},
  "knowledge": {},
  "memory": {},
  "configuration": {},
  "policies": {},
  "approval": {},
  "security": {},
  "privacy": {},
  "actor": {},
  "environment": {},
  "limits": {},
  "observability": {},
  "references": [],
  "freshness": {},
  "integrity": {},
  "extensions": {}
}
```

---

## 7. Context Metadata

Required fields:

- `context_id`
- `schema_name`
- `schema_version`
- `context_version`
- `created_at`
- `created_by`
- `context_status`
- `source`
- `checksum`
- `classification`
- `retention_class`

Allowed `context_status` values:

```text
building
validated
ready
delivered
archived
invalid
expired
revoked
superseded
```

A Context MUST NOT be delivered unless its status is `ready`.

---

## 8. Receiving Engine Identity

Required fields:

- `engine_id`
- `engine_name`
- `engine_version`
- `contract_version`
- `capability_profile`
- `configuration_version`
- `implementation_id`
- `environment`

A Context assembled for one Engine MUST NOT be reused for another Engine without rebuilding or revalidating visibility, authority, and compatibility.

---

## 9. Execution Identity

Required fields:

- `execution_id`
- `task_id`
- `trace_id`
- `attempt_number`
- `invocation_id`

Conditionally required fields include `workflow_id`, `parent_execution_id`, `correlation_id`, `checkpoint_id`, `resume_from_id`, `tenant_id`, and `project_id`.

Execution identifiers are immutable. Retries preserve `execution_id` while incrementing `attempt_number`, unless the State Model explicitly creates a new execution.

---

## 10. Runtime State

Required fields:

- `current_state`
- `state_version`
- `entered_at`
- `allowed_transitions`
- `state_owner`
- `is_terminal`
- `is_cancelled`
- `is_paused`

An Engine may propose a transition through Engine Result. Only the Runtime Orchestrator may commit the global transition.

---

## 11. Task Context

Task Context SHOULD include:

- Task object or reference;
- task type;
- objective;
- requested outputs;
- scope;
- constraints;
- priority;
- deadline;
- requester;
- business domain;
- target audience;
- success criteria;
- prohibited outcomes;
- approved assumptions.

The Engine MUST distinguish user requirements, Runtime inferences, policy constraints, and configuration defaults.

---

## 12. Runtime Objects

The section may contain:

- Task;
- Knowledge Package;
- Business Judgement;
- Execution Blueprint;
- Skill Output;
- Quality Report;
- Execution Result;
- Learning Candidate;
- Memory Record;
- Exception Record;
- Approval Record;
- Policy Decision;
- Engine Result;
- Context Snapshot.

Each entry MUST identify object ID, object type, schema version, object version, lifecycle status, owner, source Engine, creation time, integrity metadata, visibility classification, and embedded value or reference.

Draft, expired, superseded, or restricted objects MUST be excluded unless explicitly required and authorised.

---

## 13. Knowledge Layer

The Knowledge Layer may include authoritative documents, Product Passports, Product Standards, Brand Standards, Content Standards, Business Standards, policies, evidence records, source excerpts, source references, structured facts, unresolved conflicts, evidence gaps, and retrieval metadata.

Every item SHOULD identify source, authority, retrieval time, effective period, freshness, applicability, confidence, evidence reference, conflict status, and transformation history.

Knowledge is read-only.

---

## 14. Memory Layer

The Memory Layer contains approved or explicitly authorised Memory Records selected for the invocation.

Each record MUST preserve memory ID, type, authority, scope, status, version, effective period, provenance, confidence, applicability, retrieval score, conflict status, and replacement relationship.

Draft Learning Candidates MUST NOT be represented as approved memory.

Expired, deprecated, or superseded memory MUST be excluded unless the receiving Engine is handling lifecycle, conflict, migration, audit, or historical analysis.

---

## 15. Configuration Layer

Configuration may include thresholds, weights, ranking settings, timeouts, retry policy, fallback policy, quality gates, feature flags, model settings, tool settings, output limits, and environment overrides.

Configuration MUST be schema-valid, versioned, owned, traceable, compatible with the receiving Engine, and free of exposed secrets.

Critical missing configuration MUST cause validation failure.

---

## 16. Policy Layer

Policy entries MUST identify policy ID, version, authority, scope, applicability reason, effective date, precedence, enforcement mode, and conflict status.

Allowed enforcement modes:

```text
advisory
warning
mandatory
blocking
```

A material unresolved controlling policy conflict MUST block execution.

---

## 17. Approval Context

Approval Context SHOULD include existing, required, pending, denied, expired, and revoked approvals, together with scope, authority level, approver, conditions, evidence, and resume information.

The Engine MUST NOT infer approval from silence or broaden approval beyond recorded scope.

---

## 18. Security Context

Security Context SHOULD include classification, tenant boundary, organisation boundary, data-access scope, allowed and restricted resources, permitted operations, allowed and prohibited tools, authentication state, authorisation claims, secret references, audit requirements, and network restrictions.

Secrets SHOULD be represented by secure references, not raw values.

---

## 19. Privacy Context

Privacy Context SHOULD include data categories, personal-data indicators, sensitive-data indicators, jurisdiction, purpose limitation, retention, deletion, redaction, disclosure, cross-border, and minimisation rules.

Transformed data, summaries, embeddings, classifications, and Memory Records may still contain personal data.

---

## 20. Resource Limits

Supported limits include token budget, wall-clock time, CPU time, memory, storage, API calls, tool calls, model calls, retry count, parallelism, estimated cost, output size, retrieval count, and Context size.

A hard-limit breach MUST stop or block execution. A soft-limit breach MUST emit a warning and follow configured degradation behaviour.

---

## 21. Environment

Environment SHOULD identify name, deployment, region, locale, timezone, Runtime version, model provider, model identifier, model version, tool versions, network mode, feature flags, repository revision, and operating mode.

Recommended environment names:

```text
development
test
staging
production
sandbox
replay
```

---

## 22. Actor Context

Actor Context SHOULD include actor ID, actor type, organisation, role, authority level, permissions, delegation, authentication status, request origin, locale, and timezone.

Actor type may be:

```text
human
runtime
engine
service
organisation
external_system
```

The original requester MUST be distinguished from the executing Engine.

---

## 23. Observability Context

Observability Context SHOULD include trace ID, span ID, parent span ID, correlation IDs, logging level, metrics namespace, event namespace, sampling policy, audit level, redaction policy, performance baseline, and alert routing.

Every Engine event MUST be correlatable to the Context.

---

## 24. Extension Model

Extensions:

- MUST be namespaced;
- MUST be schema-valid;
- MUST declare an owner and version;
- MUST NOT override core semantics;
- MUST NOT weaken security, privacy, policy, or approval controls;
- MUST NOT become a hidden required dependency;
- SHOULD define migration behaviour.

```json
{
  "extensions": {
    "com.buxa.product_content": {
      "version": "1.0.0",
      "market": "AU",
      "channel": "xiaohongshu"
    }
  }
}
```

---

## 25. Context Lifecycle

```text
requested
↓
building
↓
composed
↓
validated
↓
integrity_checked
↓
visibility_filtered
↓
ready
↓
delivered
↓
consumed
↓
archived
```

Exceptional terminal states are `invalid`, `expired`, `revoked`, and `superseded`.

Delivered Context remains immutable. Any change creates a new Context version.

---

## 26. Context Validation

Validation MUST cover:

1. schema;
2. required fields;
3. references;
4. version compatibility;
5. receiving Engine compatibility;
6. Runtime State;
7. authority;
8. visibility;
9. policy;
10. approval;
11. security;
12. privacy;
13. freshness;
14. integrity;
15. resource limits;
16. semantic consistency.

Allowed outcomes:

```text
passed
passed_with_warning
failed
blocked
```

A failed mandatory validation prevents delivery.

---

## 27. Serialization

Canonical execution serialization MUST use JSON.

Rules:

- timestamps use ISO 8601;
- identifiers are strings;
- confidence uses a normalized range;
- maps SHOULD be canonically ordered before hashing;
- binary data SHOULD be referenced;
- secrets MUST NOT be serialized in plain text;
- circular references MUST NOT occur.

---

## 28. Replay

A replayable Context MUST preserve or reference Task, Engine version, Contract version, configuration, policy, Runtime State, input objects, Knowledge Package, Memory Records, approvals, actor, environment, model settings, tool versions, limits, checksums, and required source snapshots.

Replay modes:

```text
exact
compatible
diagnostic
historical
simulation
```

Every substitution in compatible replay MUST be recorded.

---

## 29. Compatibility

Compatibility MUST be checked among Context schema, Engine version, Engine Contract, Runtime Object schemas, State Model, policies, configuration, and extensions.

Allowed outcomes:

```text
compatible
compatible_with_warning
migration_required
incompatible
unknown
```

Unknown compatibility for a critical dependency is blocking.

---

## 30. Security Rules

The Context MUST NOT contain raw passwords, private keys, long-lived tokens, session cookies, unrestricted credentials, unnecessary personal data, or confidential data outside Engine authority.

The Context Builder MUST enforce least privilege, redaction, tenant boundaries, access decisions, reference authorization, classification preservation, and policy integrity.

---

## 31. Performance Rules

The Context SHOULD minimize unnecessary duplication through references, selective embedding, summaries with provenance, prioritised inclusion, pagination, chunking, compression, lazy resolution, and immutable caches.

Optimization MUST NOT remove controlling policy, mandatory approval, blocking evidence, provenance, Task objective, current Runtime State, or critical security and privacy constraints.

---

## 32. Context Layer Model

Seven conceptual layers:

1. **Identity Layer** — metadata, receiving Engine, execution identity, actor.
2. **Runtime Layer** — Runtime State, Task, Runtime Objects.
3. **Knowledge Layer** — Knowledge Package, sources, evidence, Memory Records.
4. **Governance Layer** — policy, approval, security, privacy.
5. **Control Layer** — configuration, limits, environment.
6. **Observability Layer** — traces, logs, metrics, audit.
7. **Integrity Layer** — references, checksums, freshness, compatibility, snapshots.

All fields MUST belong to one layer or a namespaced extension.

---

## 33. Context Visibility Rules

Visibility follows least privilege.

| Context Area | Retrieval | Business | Planning | Execution | Quality | Exception | Learning | Memory | Approval | Policy | Orchestrator |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Task | Full | Full | Full | Scoped | Full | Scoped | Full | Full | Scoped | Scoped | Full |
| Runtime State | Full | Full | Full | Full | Full | Full | Full | Full | Full | Full | Full |
| Knowledge | Full | Full | Full | Scoped | Full | Scoped | Full | Full | Scoped | Scoped | Full |
| Memory | Full | Full | Full | Scoped | Full | Scoped | Full | Full | Scoped | Scoped | Full |
| Configuration | Scoped | Scoped | Scoped | Scoped | Scoped | Scoped | Scoped | Scoped | Scoped | Scoped | Full |
| Policies | Applicable | Applicable | Applicable | Applicable | Full applicable | Full applicable | Applicable | Full applicable | Approval-related | Full | Full |
| Approval | Applicable | Applicable | Full | Full | Full | Full | Full | Full | Full | Full | Full |
| Security | Scoped | Scoped | Scoped | Scoped | Audit | Incident | Scoped | Scoped | Relevant | Relevant | Full |
| Privacy | Scoped | Scoped | Scoped | Scoped | Audit | Incident | Scoped | Relevant | Relevant | Relevant | Full |
| Observability | Own trace | Own trace | Own trace | Own trace | Cross-object | Incident | Learning | Memory | Approval | Policy | Full |

Visibility MUST be configured, tested, and auditable.

---

## 34. Context Evolution Rules

Engine Context evolves by replacement, not mutation.

```text
Context v1: Task + initial governance
↓
Context v2: Knowledge Package added
↓
Context v3: Business Judgement added
↓
Context v4: Execution Blueprint added
↓
Context v5: Skill Output + Execution Result added
↓
Context v6: Quality Report added
↓
Context v7: Learning Candidate added
↓
Context v8: Memory Record reference added
```

Each version MUST preserve parent reference, record changes, recompute integrity, and revalidate visibility, compatibility, and freshness.

---

## 35. Context Identity Model

Identity consists of stable Context lineage, monotonic version, optional snapshot ID, execution ID, receiving Engine ID, and checksum.

Recommended pattern:

```text
ctx_<execution>_<engine>_<version>
```

A retry may create a new Context version while preserving the logical execution.

---

## 36. Context Composition Rules

Recommended composition order:

1. Runtime identity;
2. Engine identity;
3. committed Runtime State;
4. Task;
5. authorised Runtime Objects;
6. Knowledge Package;
7. selected Memory Records;
8. configuration;
9. policies;
10. approvals;
11. security and privacy;
12. actor and environment;
13. limits;
14. observability;
15. references;
16. freshness;
17. integrity;
18. extensions.

Controlling policy overrides convenience configuration. Approval conditions override defaults. Higher-authority applicable knowledge overrides lower-authority knowledge. Conflicts MUST remain visible.

---

## 37. Context Reference Model

A Context Reference SHOULD include reference ID, object ID, object type, location, schema version, object version, content hash, classification, access scope, expiry, resolver, availability, retrieval instructions, and immutable flag.

```json
{
  "reference_id": "ref_01",
  "object_id": "kp_01",
  "object_type": "knowledge_package",
  "location": "runtime://knowledge/kp_01",
  "schema_version": "1.0.0",
  "object_version": 3,
  "content_hash": "sha256:example",
  "classification": "internal",
  "availability": "available",
  "immutable": true
}
```

A failed mandatory reference blocks delivery.

---

## 38. Context Snapshot Model

A Context Snapshot is a persistent, immutable representation of a delivered Context.

It MUST record snapshot ID, Context identity, creation time, builder version, Engine identity, execution identity, canonical checksum, included references, dependency versions, classification, retention, encryption status, and integrity proof where required.

A corrected snapshot becomes a new snapshot with a supersession relationship.

---

## 39. Context Integrity Model

Integrity status:

```text
verified
verified_with_warning
unverified
mismatch
corrupt
```

A mismatch or corrupt status for mandatory content blocks delivery.

Integrity controls may include canonical JSON hashes, per-object hashes, signed manifests, immutable versions, reference checks, schema validation, and dependency locks.

---

## 40. Context Freshness Model

Freshness status:

```text
current
aging
stale
expired
unknown
not_applicable
```

Time-sensitive items SHOULD identify observation time, effective period, review date, maximum age, freshness status, and refresh source.

Expired controlling policy, approval, security claim, or critical knowledge blocks execution unless an approved historical or simulation mode applies.

---

## 41. Context Priority Model

Default authority priority:

```text
1. mandatory security and privacy controls
2. controlling policy
3. valid approval conditions
4. committed Runtime State
5. explicit Task constraints
6. authoritative current knowledge
7. approved applicable Memory Records
8. Engine configuration
9. non-controlling sources
10. heuristic or inferred context
```

Priority MUST NOT silently erase conflicts.

---

## 42. Context Size Management

Recommended actions:

1. remove duplicates;
2. replace large immutable objects with references;
3. remove irrelevant optional items;
4. select higher-relevance memory;
5. select higher-authority knowledge;
6. compress metadata;
7. summarize secondary material with provenance;
8. split into staged invocations;
9. request larger approved budget;
10. block if mandatory content cannot fit safely.

The Builder MUST NOT truncate controlling policy, approval conditions, critical security or privacy constraints, blocking evidence, Task objective, current Runtime State, mandatory provenance, or required outputs.

---

## 43. Context Governance

### Runtime Orchestrator

Requests Context creation, selects the receiving Engine, commits Context lineage, delivers validated Context, and records result relationships.

### Context Builder

Composes Context, enforces visibility, resolves references, validates compatibility, records provenance, and calculates integrity.

### Policy Engine

Determines applicable policy, precedence, and conflicts.

### Approval Engine

Provides approval status and conditions.

### Security and Privacy Controls

Enforce access, minimization, redaction, and boundaries.

### Receiving Engine

Treats Context as immutable, uses authorised data only, reports invalid content, and returns proposed changes through Engine Result.

---

## 44. Context Acceptance Criteria

### Identity

- [ ] Context ID is present.
- [ ] Schema version is supported.
- [ ] Receiving Engine is identified.
- [ ] Execution identity is complete.
- [ ] Context version is valid.

### Content

- [ ] Task is present.
- [ ] Runtime State is current.
- [ ] Required Runtime Objects are present.
- [ ] Required knowledge is present or explicitly unavailable.
- [ ] Required Memory Records are present or not applicable.
- [ ] Configuration is complete.
- [ ] Applicable policies are included.
- [ ] Approval status is known.

### Governance

- [ ] Visibility is filtered.
- [ ] Actor authority is valid.
- [ ] Security and privacy checks pass.
- [ ] Classification is preserved.
- [ ] Retention is defined.

### Integrity and Freshness

- [ ] References resolve.
- [ ] Checksums validate.
- [ ] Required content is fresh.
- [ ] Compatibility passes.
- [ ] Context is immutable.
- [ ] Snapshot policy is satisfied.

### Operations

- [ ] Limits are present.
- [ ] Observability IDs are present.
- [ ] Status is `ready`.
- [ ] Validation passed.
- [ ] No unresolved blocking issue exists.

---

## 45. Enterprise Example

```json
{
  "context_id": "ctx_exec_001_planning_004",
  "schema_name": "engine_context",
  "schema_version": "1.0.0",
  "context_version": 4,
  "created_at": "2026-07-28T10:00:00Z",
  "created_by": {
    "actor_type": "runtime",
    "actor_id": "context_builder"
  },
  "context_status": "ready",
  "source": "runtime_orchestrator",
  "classification": "internal",
  "retention_class": "runtime_audit",
  "receiving_engine": {
    "engine_id": "planning_engine",
    "engine_name": "Planning Engine",
    "engine_version": "1.0.0",
    "contract_version": "1.0.0",
    "capability_profile": "planning.default",
    "configuration_version": "1.2.0",
    "implementation_id": "planning-python",
    "environment": "production"
  },
  "execution": {
    "execution_id": "exec_001",
    "task_id": "task_001",
    "workflow_id": "wf_content_001",
    "trace_id": "trace_001",
    "invocation_id": "inv_004",
    "attempt_number": 1
  },
  "runtime_state": {
    "current_state": "business_judgement_ready",
    "previous_state": "knowledge_ready",
    "state_version": 3,
    "entered_at": "2026-07-28T09:59:30Z",
    "allowed_transitions": [
      "execution_blueprint_ready",
      "blocked",
      "waiting"
    ],
    "state_owner": "runtime_orchestrator",
    "is_terminal": false,
    "is_cancelled": false,
    "is_paused": false
  },
  "task": {
    "object_id": "task_001",
    "object_type": "task",
    "schema_version": "1.0.0",
    "objective": "Prepare a draft product content package",
    "requested_outputs": [
      "execution_blueprint"
    ],
    "constraints": [
      "Do not publish",
      "Use approved product knowledge only"
    ],
    "priority": "normal"
  },
  "runtime_objects": {
    "knowledge_package": {
      "object_id": "kp_001",
      "schema_version": "1.0.0",
      "object_version": 1,
      "status": "approved",
      "reference_id": "ref_kp_001"
    },
    "business_judgement": {
      "object_id": "bj_001",
      "schema_version": "1.0.0",
      "object_version": 1,
      "status": "approved",
      "reference_id": "ref_bj_001"
    }
  },
  "knowledge": {
    "package_id": "kp_001",
    "authority_level": "approved",
    "freshness_status": "current",
    "source_refs": [
      "source_product_passport_hd03",
      "source_brand_standard",
      "source_content_standard"
    ],
    "conflicts": [],
    "evidence_gaps": []
  },
  "memory": {
    "selected_records": [
      {
        "memory_id": "mem_001",
        "memory_type": "content_quality_rule",
        "status": "active",
        "authority": "approved",
        "retrieval_score": 0.94,
        "freshness_status": "current"
      }
    ]
  },
  "configuration": {
    "configuration_id": "planning_config",
    "version": "1.2.0",
    "timeout_seconds": 120,
    "max_steps": 20,
    "fallback_enabled": true
  },
  "policies": {
    "applicable": [
      {
        "policy_id": "policy_evidence_bound_claims",
        "version": "1.0.0",
        "enforcement_mode": "blocking",
        "precedence": 100
      }
    ],
    "conflicts": []
  },
  "approval": {
    "required": false,
    "existing": [],
    "pending": [],
    "denied": []
  },
  "security": {
    "classification": "internal",
    "allowed_resources": [
      "runtime_objects",
      "approved_memory",
      "approved_knowledge"
    ],
    "restricted_resources": [
      "secrets",
      "unapproved_memory"
    ]
  },
  "privacy": {
    "contains_personal_data": false,
    "retention_policy": "runtime_audit",
    "redaction_required": false
  },
  "actor": {
    "actor_id": "user_001",
    "actor_type": "human",
    "role": "brand_owner",
    "authority_level": "business_owner"
  },
  "environment": {
    "name": "production",
    "region": "ap-northeast",
    "locale": "zh-CN",
    "timezone": "Asia/Tokyo",
    "runtime_version": "1.0.0",
    "model_identifier": "configured_by_runtime"
  },
  "limits": {
    "token_budget": 30000,
    "time_budget_seconds": 120,
    "max_tool_calls": 10,
    "max_retry_count": 2,
    "hard_output_bytes": 500000
  },
  "observability": {
    "trace_id": "trace_001",
    "span_id": "span_004",
    "parent_span_id": "span_003",
    "logging_level": "info",
    "metrics_namespace": "scout.runtime.planning",
    "audit_level": "standard"
  },
  "references": [
    {
      "reference_id": "ref_kp_001",
      "object_id": "kp_001",
      "object_type": "knowledge_package",
      "location": "runtime://knowledge/kp_001",
      "schema_version": "1.0.0",
      "object_version": 1,
      "content_hash": "sha256:example",
      "classification": "internal",
      "availability": "available",
      "immutable": true
    },
    {
      "reference_id": "ref_bj_001",
      "object_id": "bj_001",
      "object_type": "business_judgement",
      "location": "runtime://judgement/bj_001",
      "schema_version": "1.0.0",
      "object_version": 1,
      "content_hash": "sha256:example",
      "classification": "internal",
      "availability": "available",
      "immutable": true
    }
  ],
  "freshness": {
    "overall_status": "current",
    "evaluated_at": "2026-07-28T10:00:00Z",
    "blocking_items": []
  },
  "integrity": {
    "algorithm": "sha256",
    "checksum": "sha256:example-context",
    "status": "verified",
    "verified_at": "2026-07-28T10:00:00Z"
  },
  "extensions": {}
}
```

---

## 46. Validation Example

```json
{
  "context_id": "ctx_exec_001_planning_004",
  "validation_status": "passed",
  "validated_at": "2026-07-28T10:00:00Z",
  "validator_version": "1.0.0",
  "checks": [
    {
      "check": "schema",
      "status": "passed"
    },
    {
      "check": "compatibility",
      "status": "passed"
    },
    {
      "check": "visibility",
      "status": "passed"
    },
    {
      "check": "security",
      "status": "passed"
    },
    {
      "check": "privacy",
      "status": "passed"
    },
    {
      "check": "freshness",
      "status": "passed"
    },
    {
      "check": "integrity",
      "status": "passed"
    }
  ],
  "warnings": [],
  "blocking_issues": []
}
```

---

## 47. Prohibited Context Behaviours

Prohibited behaviours include:

- delivering Context with failed schema validation;
- allowing an Engine to mutate Context;
- exposing data outside Engine authority;
- embedding raw secrets unnecessarily;
- omitting controlling policy or approval conditions;
- replacing missing knowledge with invented facts;
- treating draft learning as approved memory;
- reusing a Context for another Engine without revalidation;
- ignoring stale controlling data;
- truncating blocking evidence;
- resolving controlling conflicts without authority;
- changing Context without a new version;
- storing global Runtime State only inside Context;
- using logs as the authoritative Context record;
- using unregistered extensions to alter core semantics.

---

## 48. Summary

Engine Context is the Runtime's governed execution universe for one Engine invocation.

It defines what the Engine may see, rely on, and do; what constraints apply; what authority exists; what information is current and trusted; what resources remain; how the invocation is traced; and how it may be replayed.

Every Engine sees the Runtime through Engine Context.

Nothing outside Engine Context may be treated as authoritative execution state unless explicitly resolved through an authorised Runtime reference.

Engine Context is immutable. Runtime change occurs through Engine Result and Orchestrator-controlled state transition.


---

## Appendix A — JSON Syntax Validation Certificate

All fenced `json` code blocks were parsed successfully with Python's standard `json` parser.

```json
{
  "document": "engine_context.md",
  "document_version": "1.0.0",
  "validated_at": "2026-07-28T10:32:51.328473+00:00",
  "json_code_blocks_found": 5,
  "json_code_blocks_valid": 5,
  "json_syntax_status": "passed"
}
```
