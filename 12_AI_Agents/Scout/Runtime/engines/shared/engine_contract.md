# Engine Contract

**Document ID:** `SCOUT-RUNTIME-ENGINE-CONTRACT`
**Version:** `1.0.0`
**Status:** `Approved`
**Owner:** `Scout Runtime`
**Applies To:** All Runtime Engines
**Contract Type:** Mandatory Runtime Standard
**Runtime State Dependency:** `state_model.md` version `1.0.0`
**Engine Context Dependency:** `engine_context.md` version `1.0.0`
**Engine Result Schema:** `../../schemas/engine_result.schema.json` version `1.0.0`

---

## 1. Purpose

This document defines the universal contract that every Scout Runtime Engine must follow.

It standardises:

- engine responsibilities;
- input and output interfaces;
- authority boundaries;
- lifecycle behaviour;
- state transitions;
- validation requirements;
- quality obligations;
- exception handling;
- approval handling;
- observability;
- security and privacy;
- versioning;
- testing;
- compatibility;
- interaction between Engines.

Every Engine inside Scout Runtime MUST conform to this contract.

No Engine may introduce incompatible execution semantics, hidden state, undocumented authority, or private interaction rules that bypass Runtime governance.

---

## 2. Scope

This contract applies to all components classified as Runtime Engines, including:

- Input Engine;
- Retrieval Engine;
- Business Judgement Engine;
- Planning Engine;
- Execution Engine;
- Quality Engine;
- Exception Engine;
- Learning Engine;
- Memory Engine;
- Approval Engine;
- Policy Engine;
- Runtime Orchestrator;
- future Engines registered in the Engine Registry.

This contract does not define the internal business logic of any specific Engine.

It defines how every Engine behaves from the Runtime's perspective.

### 2.1 Canonical Engine Taxonomy Authority

`engine_contract.md` is the sole normative authority for canonical Runtime Engine identities. The canonical Engine set is:

- Input Engine;
- Retrieval Engine;
- Business Judgement Engine;
- Planning Engine;
- Execution Engine;
- Quality Engine;
- Learning Engine;
- Memory Engine;
- Exception Engine;
- Approval Engine;
- Policy Engine.

`Reasoning Engine` is a deprecated behavioural alias for Business Judgement Engine. `Feedback Engine` is a deprecated behavioural alias for Learning Engine. Existing prose may use reasoning and feedback as behavioural concepts, but new machine contracts, registry authorities, events, actor identifiers, producer lists, and consumer lists MUST use the canonical Engine identity. Deprecated aliases MUST be rejected as producing or consuming authority identifiers.

Other Runtime and Operating Manual documents MUST reference this taxonomy and MUST NOT create a competing Engine set.

---

## 3. Normative Language

The terms below are normative:

- **MUST / SHALL** — mandatory requirement;
- **MUST NOT / SHALL NOT** — prohibited behaviour;
- **SHOULD** — recommended unless a documented exception exists;
- **SHOULD NOT** — discouraged unless a documented exception exists;
- **MAY** — optional behaviour;
- **REQUIRED** — mandatory field, state, action, or control.

Any deviation from a MUST or MUST NOT requirement requires:

1. a documented exception;
2. an identified owner;
3. explicit approval;
4. a defined expiry or review date;
5. an audit record.

---

## 4. Runtime Engine Definition

A Runtime Engine is a governed decision or operational component that transforms one valid Runtime State into another valid Runtime State.

An Engine:

- receives one Engine Context;
- validates its inputs;
- evaluates preconditions;
- performs one bounded responsibility;
- produces one Engine Result;
- creates or proposes Runtime Objects;
- emits observable events;
- records metrics;
- respects policy, security, privacy, quality, and approval controls;
- never modifies objects outside its declared authority.

An Engine is not:

- an unrestricted prompt;
- an ungoverned agent;
- a hidden memory store;
- a direct database mutator;
- a replacement for the Runtime Orchestrator;
- an approval authority unless explicitly authorised.

---

## 5. Core Runtime Principles

### 5.1 Single Responsibility

Each Engine MUST own one clearly defined primary responsibility.

Examples:

#### Retrieval Engine

Responsible for:

- interpreting retrieval requirements;
- locating candidate knowledge;
- ranking and selecting knowledge;
- constructing the Knowledge Package.

Not responsible for:

- business recommendation;
- execution planning;
- final quality approval;
- memory approval.

#### Planning Engine

Responsible for:

- decomposing work;
- resolving dependencies;
- selecting skills and tools;
- generating the Execution Blueprint.

Not responsible for:

- retrieving authoritative knowledge;
- executing tools;
- approving its own plan;
- converting learning into memory.

An Engine MUST declare:

- responsibilities;
- non-responsibilities;
- owned Runtime Objects;
- readable Runtime Objects;
- writable fields;
- prohibited actions.

### 5.2 Determinism and Reproducibility

Given materially equivalent:

- Engine Context;
- Engine version;
- configuration version;
- policy version;
- knowledge inputs;
- memory inputs;
- model or algorithm settings;

an Engine SHOULD produce materially equivalent decisions and outputs.

Where exact determinism is not technically possible, the Engine MUST provide:

- reproducibility metadata;
- model identifier;
- temperature or equivalent stochastic settings;
- prompt or ruleset version;
- source snapshot references;
- a decision summary;
- confidence;
- known sources of variability.

### 5.3 Observability

No material Engine action may occur silently.

Every Engine MUST emit enough information to determine:

- when it started;
- what it received;
- what it decided;
- what it produced;
- what it changed;
- what it refused;
- what warnings or errors occurred;
- how long it ran;
- what resources it used;
- whether approval was required;
- whether quality gates passed.

### 5.4 Traceability

Every material output MUST be traceable to:

- input Runtime Objects;
- source knowledge;
- memory records;
- policies;
- configuration;
- decision rules;
- actor or Engine identity;
- Engine version.

A Runtime Object created by an Engine MUST contain or reference sufficient provenance to reconstruct why it exists.

### 5.5 Governance

No Engine may bypass:

- Approval Engine;
- Policy Engine;
- Security controls;
- Privacy controls;
- Quality gates;
- authority boundaries;
- mandatory state transitions;
- audit requirements.

Governance controls apply even when the Engine has high confidence.

### 5.6 Recoverability

Every failure MUST result in one of the following explicit outcomes:

- retry;
- fallback;
- pause;
- escalation;
- block;
- cancellation;
- controlled abort;
- partial success with documented limitations.

Failures MUST NOT:

- disappear;
- be converted into success silently;
- be recorded only as free-form log text;
- be hidden from the Orchestrator;
- leave the Runtime in an undefined state.

### 5.7 Explicit State

Engines MUST communicate through declared Runtime Objects and Runtime State.

Engines MUST NOT depend on:

- hidden prompt memory;
- undocumented global variables;
- private mutable state shared across Engines;
- implicit conversation context;
- unregistered caches as sources of authority.

### 5.8 Evidence Before Assertion

An Engine MUST NOT present a factual, policy, product, quality, legal, financial, operational, or security assertion as authoritative unless it is supported by an approved source or declared inference.

Where evidence is incomplete, the Engine MUST:

- lower confidence;
- identify the evidence gap;
- qualify the output;
- request review where required;
- avoid false certainty.

---

## 6. Standard Engine Interface

Conceptually, every Runtime Engine implements:

```text
EngineContext
    ↓
Engine.execute(context)
    ↓
EngineResult
```

The concrete implementation MAY use Python, TypeScript, another language, or a remote service.

The logical interface MUST remain consistent.

### 6.1 Conceptual Interface

```typescript
interface RuntimeEngine {
  readonly engineId: string;
  readonly engineVersion: string;
  readonly contractVersion: string;

  validateContext(context: EngineContext): ValidationResult;

  evaluatePreconditions(context: EngineContext): PreconditionResult;

  execute(context: EngineContext): Promise<EngineResult>;

  validateResult(result: EngineResult): ValidationResult;
}
```

### 6.2 Interface Rules

Every Engine MUST:

- accept exactly one Engine Context;
- return exactly one Engine Result;
- avoid untyped positional parameters;
- avoid returning raw text as the complete result;
- avoid mutating the supplied Engine Context;
- declare all side effects;
- expose version and compatibility metadata.

---

## 7. Engine Context Dependency

`engine_context.md` version `1.0.0` is the sole normative authority for Runtime Engine Context, including Context structure, metadata, lifecycle, ownership, integrity, and validation.

Every Engine MUST receive and use an Engine Context that conforms to that specification. This Engine Contract MUST NOT define, extend, or override Engine Context semantics.

Engine Context acceptance, validation, use, and any proposed changes MUST follow `engine_context.md`.

---

## 8. Universal Engine Result

Every Engine returns one Engine Result.

An Engine Result is the canonical Runtime Execution Evidence envelope for one Engine invocation, not a raw response or a Runtime State. It records the invocation outcome and the evidence needed to correlate the invocation with authoritative Decisions, proposed Runtime State transitions, Runtime Events, collected evidence, and validation results.

This contract is the normative owner of Engine Result semantics. `../../schemas/engine_result.schema.json` version `1.0.0` is the machine-readable authority for the Engine Result payload.

### 8.1 Required Engine Result Fields

The Engine Result MUST include:

- `id`;
- `schema_name`;
- `schema_version`;
- `engine_id`;
- `engine_version`;
- `contract_version`;
- `task_id`;
- `execution_id`;
- `invocation_id`;
- `created_at`;
- `created_by`;
- `status`;
- `primary_output`;
- `secondary_outputs`;
- `decision_ref`;
- `confidence`;
- `warnings`;
- `blocking_issues`;
- `events`;
- `metrics`;
- `timing`;
- `state_transition`;
- `side_effects`;
- `approval`;
- `recovery`;
- `failure`;
- `validation`;
- `extensions`.

`primary_output` is the sole canonical field for the main Engine Result payload. The fields `output` and `primary output` MUST NOT be used as alternate machine-readable property names.

`confidence` is represented exactly once in an Engine Result as a top-level structured Confidence Assessment conforming to `../../schemas/common.schema.json#/$defs/confidence`, or `null` when no material confidence assessment applies. A referenced Decision MUST NOT be redefined inside Engine Result; `decision_ref` identifies the authoritative Decision and carries only its identity, version, and reviewable summary.

For backward compatibility, an Alpha Engine Result MAY omit `evidence_profile` and `runtime_execution_evidence`; all existing required fields and validation rules remain in force. New implementations SHOULD return `evidence_profile: runtime_execution_evidence_v1` and the corresponding `runtime_execution_evidence` object. A result that proposes a governed dynamic target MUST use that profile.

The canonical evidence profile contains six sections:

- `invocation_identity`;
- `decision_evidence`, which references the authoritative Decision and MUST NOT duplicate Decision semantics;
- `runtime_state_transition_evidence`;
- `runtime_events`;
- `evidence_collection`;
- `validation_results`.

Where the profile repeats a compatibility field, both representations MUST identify the same invocation, Decision, transition proposal, or Runtime Events. The Runtime Orchestrator MUST validate that correlation. `runtime_execution_evidence` records evidence only; it does not commit Runtime State.

### 8.2 Canonical Engine Result Example

```json
{
  "id": "engine_result_550e8400-e29b-41d4-a716-446655440000",
  "schema_name": "engine_result",
  "schema_version": "1.0.0",
  "engine_id": "retrieval_engine",
  "engine_version": "1.0.0",
  "contract_version": "1.0.0",
  "task_id": "task_550e8400-e29b-41d4-a716-446655440001",
  "execution_id": "exec_550e8400-e29b-41d4-a716-446655440002",
  "invocation_id": "invocation_retrieval_001",
  "created_at": "2026-07-28T10:00:01Z",
  "created_by": {
    "actor_type": "engine",
    "actor_id": "retrieval_engine",
    "display_name": "Retrieval Engine",
    "role": "retrieval",
    "version": "1.0.0"
  },
  "status": "succeeded",
  "primary_output": {
    "output_kind": "runtime_object_reference",
    "summary": "Knowledge Package created and validated.",
    "object_ref": {
      "object_id": "knowledge_package_550e8400-e29b-41d4-a716-446655440003",
      "object_type": "knowledge_package",
      "schema_name": "knowledge_package",
      "schema_version": "1.0.0"
    },
    "value": null
  },
  "secondary_outputs": [],
  "decision_ref": {
    "decision_id": "decision_550e8400-e29b-41d4-a716-446655440004",
    "decision_version": 1,
    "summary": "The retrieved package satisfies the required knowledge scope."
  },
  "confidence": {
    "level": "high",
    "score": 0.9,
    "basis": "Required authoritative sources were retrieved and validated.",
    "limitations": []
  },
  "warnings": [],
  "blocking_issues": [],
  "events": [
    {
      "event_id": "event_engine_completed_001",
      "event_type": "engine_completed",
      "timestamp": "2026-07-28T10:00:01Z"
    },
    {
      "event_id": "event_state_transition_proposed_001",
      "event_type": "engine_state_transition_proposed",
      "timestamp": "2026-07-28T10:00:01Z"
    }
  ],
  "metrics": {
    "duration_ms": 1000,
    "output_count": 1
  },
  "timing": {
    "started_at": "2026-07-28T10:00:00Z",
    "returned_at": "2026-07-28T10:00:01Z",
    "duration_ms": 1000
  },
  "state_transition": {
    "transition_id": "transition_retrieval_ready_001",
    "from_state": "retrieval_running",
    "to_state": "knowledge_ready",
    "triggering_engine_id": "retrieval_engine",
    "triggering_result_id": "engine_result_550e8400-e29b-41d4-a716-446655440000",
    "proposed_by": {
      "actor_type": "engine",
      "actor_id": "retrieval_engine",
      "display_name": "Retrieval Engine",
      "role": "retrieval",
      "version": "1.0.0"
    },
    "proposed_at": "2026-07-28T10:00:01Z",
    "reason_code": "knowledge_package_ready",
    "validation_outcome": "passed",
    "approval_required": false,
    "rollback_state": null,
    "transition_mechanism": "static",
    "dynamic_target": null,
    "decision_id": "decision_550e8400-e29b-41d4-a716-446655440004",
    "approval_reference": null,
    "policy_reference": null,
    "checkpoint_reference": null,
    "retry_reference": null,
    "recovery_reference": null,
    "rollback_reference": null,
    "origin_context": null
  },
  "side_effects": [],
  "approval": {
    "required": false,
    "status": "not_required"
  },
  "recovery": null,
  "failure": null,
  "validation": {
    "status": "valid",
    "validated_at": "2026-07-28T10:00:01Z",
    "validator": {
      "actor_type": "engine",
      "actor_id": "retrieval_engine",
      "display_name": "Retrieval Engine",
      "role": "retrieval",
      "version": "1.0.0"
    },
    "schema_version_validated": "1.0.0",
    "errors": [],
    "warnings": []
  },
  "extensions": {},
  "evidence_profile": "runtime_execution_evidence_v1",
  "runtime_execution_evidence": {
    "invocation_identity": {
      "engine_id": "retrieval_engine",
      "engine_version": "1.0.0",
      "contract_version": "1.0.0",
      "task_id": "task_550e8400-e29b-41d4-a716-446655440001",
      "execution_id": "exec_550e8400-e29b-41d4-a716-446655440002",
      "invocation_id": "invocation_retrieval_001"
    },
    "decision_evidence": {
      "authority": "../engines/shared/decision_model.md",
      "decision_ref": {
        "decision_id": "decision_550e8400-e29b-41d4-a716-446655440004",
        "decision_version": 1,
        "summary": "The retrieved package satisfies the required knowledge scope."
      },
      "evidence_refs": [
        {
          "reference_id": "knowledge_package_550e8400-e29b-41d4-a716-446655440003",
          "object_type": "knowledge_package",
          "version": 1
        }
      ]
    },
    "runtime_state_transition_evidence": {
      "authority": "../engines/shared/state_model.md",
      "proposal": {
        "transition_id": "transition_retrieval_ready_001",
        "from_state": "retrieval_running",
        "to_state": "knowledge_ready",
        "triggering_engine_id": "retrieval_engine",
        "triggering_result_id": "engine_result_550e8400-e29b-41d4-a716-446655440000",
        "proposed_by": {
          "actor_type": "engine",
          "actor_id": "retrieval_engine",
          "display_name": "Retrieval Engine",
          "role": "retrieval",
          "version": "1.0.0"
        },
        "proposed_at": "2026-07-28T10:00:01Z",
        "reason_code": "knowledge_package_ready",
        "validation_outcome": "passed",
        "approval_required": false,
        "rollback_state": null,
        "transition_mechanism": "static",
        "dynamic_target": null,
        "decision_id": "decision_550e8400-e29b-41d4-a716-446655440004",
        "approval_reference": null,
        "policy_reference": null,
        "checkpoint_reference": null,
        "retry_reference": null,
        "recovery_reference": null,
        "rollback_reference": null,
        "origin_context": null
      },
      "orchestrator_validation": {
        "status": "valid",
        "validated_at": "2026-07-28T10:00:01Z",
        "validator": {
          "actor_type": "runtime",
          "actor_id": "runtime_orchestrator",
          "display_name": "Runtime Orchestrator",
          "role": "orchestration",
          "version": "1.0.0"
        },
        "schema_version_validated": "1.0.0",
        "errors": [],
        "warnings": []
      },
      "commit_status": "pending"
    },
    "runtime_events": {
      "event_refs": [
        {
          "event_id": "event_engine_completed_001",
          "event_type": "engine_completed",
          "timestamp": "2026-07-28T10:00:01Z"
        },
        {
          "event_id": "event_state_transition_proposed_001",
          "event_type": "engine_state_transition_proposed",
          "timestamp": "2026-07-28T10:00:01Z"
        }
      ],
      "correlation_complete": true
    },
    "evidence_collection": {
      "evidence_refs": [
        {
          "reference_id": "knowledge_package_550e8400-e29b-41d4-a716-446655440003",
          "object_type": "knowledge_package",
          "version": 1
        }
      ],
      "collection_complete": true
    },
    "validation_results": {
      "schema_validation": {
        "status": "valid",
        "validated_at": "2026-07-28T10:00:01Z",
        "validator": {
          "actor_type": "engine",
          "actor_id": "retrieval_engine",
          "display_name": "Retrieval Engine",
          "role": "retrieval",
          "version": "1.0.0"
        },
        "schema_version_validated": "1.0.0",
        "errors": [],
        "warnings": []
      },
      "decision_validation": {
        "status": "valid",
        "validated_at": "2026-07-28T10:00:01Z",
        "validator": {
          "actor_type": "engine",
          "actor_id": "retrieval_engine",
          "display_name": "Retrieval Engine",
          "role": "retrieval",
          "version": "1.0.0"
        },
        "schema_version_validated": "1.0.0",
        "errors": [],
        "warnings": []
      },
      "state_transition_validation": {
        "status": "valid",
        "validated_at": "2026-07-28T10:00:01Z",
        "validator": {
          "actor_type": "engine",
          "actor_id": "retrieval_engine",
          "display_name": "Retrieval Engine",
          "role": "retrieval",
          "version": "1.0.0"
        },
        "schema_version_validated": "1.0.0",
        "errors": [],
        "warnings": []
      },
      "event_validation": {
        "status": "valid",
        "validated_at": "2026-07-28T10:00:01Z",
        "validator": {
          "actor_type": "engine",
          "actor_id": "retrieval_engine",
          "display_name": "Retrieval Engine",
          "role": "retrieval",
          "version": "1.0.0"
        },
        "schema_version_validated": "1.0.0",
        "errors": [],
        "warnings": []
      },
      "evidence_validation": {
        "status": "valid",
        "validated_at": "2026-07-28T10:00:01Z",
        "validator": {
          "actor_type": "engine",
          "actor_id": "retrieval_engine",
          "display_name": "Retrieval Engine",
          "role": "retrieval",
          "version": "1.0.0"
        },
        "schema_version_validated": "1.0.0",
        "errors": [],
        "warnings": []
      }
    }
  }
}
```

### 8.3 Result Completeness

A `succeeded` Engine Result MUST:

- contain a non-empty `primary_output` whose `output_kind` is not `none`;
- reference the material Decision through `decision_ref` where a material Decision exists;
- contain top-level `confidence` where a material confidence assessment applies;
- pass Engine Result schema validation;
- include timing, events, and the State Model-governed transition proposal or reference where applicable.

A `partial_success` Engine Result MUST contain:

- a non-empty `primary_output` for the completed scope;
- the incomplete scope, limitation, and follow-up action in structured warnings or blocking issues;
- risk and recovery information where applicable;
- schema-valid validation, timing, events, and transition information.

A failed or blocked Engine Result MUST NOT omit:

- `failure.category`;
- `failure.severity`;
- `failure.summary`;
- `failure.recovery_recommendation`;
- `failure.exception_ref`, using `null` only when the Exception Record has been requested but does not yet exist;
- at least one `blocking_issues` entry.

For `waiting`, `cancelled`, or `timeout`, `primary_output.output_kind` MAY be `none`. The status remains an Engine invocation status and MUST NOT be used as Runtime State.

### 8.4 Engine Result and Execution Result

Engine Result and Execution Result are distinct objects.

- **Engine Result** is the universal response envelope returned by every Engine invocation and is governed by this contract and `../../schemas/engine_result.schema.json`.
- **Execution Result** is a Runtime Object produced by the Execution Engine and is governed by `../../schemas/execution_result.schema.json`.

An Engine Result MAY reference an Execution Result through `primary_output.object_ref` or carry a schema-valid Execution Result through an embedded `primary_output.value`. The two objects MUST NOT be treated as interchangeable, and an Execution Result MUST NOT replace the Engine Result envelope.

---

## 9. Engine Lifecycle

Every Engine follows the same logical lifecycle.

```text
Registered
↓
Idle
↓
Context Received
↓
Input Validation
↓
Precondition Evaluation
↓
Policy and Authority Check
↓
Execution
↓
Output Validation
↓
Local Quality Check
↓
Result Construction
↓
State Transition Proposal
↓
Event and Metric Emission
↓
Completed
```

If any mandatory stage fails:

```text
Failure Detected
↓
Failure Classified
↓
Exception Record Created or Requested
↓
Retry / Fallback / Escalate / Block / Abort
```

### 9.1 Lifecycle Rules

An Engine MUST NOT begin execution before:

- context validation passes;
- required inputs are present;
- configuration is available;
- dependencies are available;
- policy checks pass;
- security checks pass;
- required approval already exists or the Engine is authorised to request it.

An Engine MUST NOT report success before:

- output schema validation passes;
- local quality obligations are met;
- required events are emitted;
- required metrics are recorded;
- the proposed state transition is valid.

---

## 10. Engine Status Model

Engine Invocation Status is the final return disposition of one Engine invocation. It is reported only through the mandatory Engine Result `status` field and is scoped by `invocation_id`.

Every Engine invocation MUST return exactly one final invocation disposition. Once its Engine Result is returned, that invocation is complete and MUST NOT resume in place. A resumable workflow continues through a new invocation identity and revalidated Engine Context.

Engine Invocation Status is not Runtime State. It MUST NOT be committed as Runtime State, used as a State Transition target, or interpreted as workflow completion or termination.

### 10.1 Allowed Statuses

```text
waiting
succeeded
partial_success
failed
blocked
cancelled
timeout
```

### 10.2 Status Semantics

#### `waiting`

The invocation returned because it cannot continue until an external or scheduled condition is satisfied, such as:

- approval;
- human input;
- dependency completion;
- source availability;
- scheduled retry.

#### `succeeded`

All required outputs were produced and all mandatory local gates passed.

This is local invocation success. It MUST NOT imply `workflow_completed`, `workflow_closed`, or any other Runtime State.

#### `partial_success`

Some valid outputs were produced, but one or more non-optional objectives were not completed.

A partial success MUST include:

- completed scope;
- incomplete scope;
- limitation;
- risk;
- recovery or follow-up action.

#### `failed`

Execution did not produce a valid required result.

Failure is final for the invocation but MUST NOT automatically imply `failed_terminal` or another terminal Runtime State.

#### `blocked`

Execution was intentionally prevented by:

- policy;
- approval;
- security;
- privacy;
- unresolved dependency;
- missing authoritative evidence;
- critical quality gate.

#### `cancelled`

Execution was stopped by an authorised actor or Runtime control. The status may be returned only after required cleanup is complete or cleanup ownership and remaining side effects are explicitly handed off and recorded.

#### `timeout`

Execution exceeded an approved time limit.

`timeout` is the invocation disposition and failure cause. It MUST NOT be silently normalised to `failed` and MUST NOT be substituted for the Runtime State `timed_out`. The Orchestrator may commit `timed_out` only when the State Model permits it and no recovery path remains.

### 10.3 Invocation Finality and Workflow Resumability

All seven canonical statuses are final for the identified invocation.

- `waiting` is normally resumable through a new invocation.
- `blocked` is resumable only when policy, remediation, approval, or escalation permits it.
- `partial_success`, `failed`, and `timeout` may lead to follow-up, retry, recovery, rollback, escalation, cancellation, or terminal handling through governed Decisions and State Transitions.
- `cancelled` ends the invocation after cleanup or cleanup handoff.
- `succeeded` completes only the local invocation obligation.

Workflow resumability is not encoded by substituting another status value. It is expressed through recovery information, Decisions, Runtime Events, and a State Model-authorised transition proposal.

### 10.4 Lifecycle Phases and Rejected Statuses

`ready` and `running` are lifecycle phases observed through Runtime Events. They are not Engine Result statuses.

The following values are rejected for Engine Result `status`:

- `ready` — lifecycle phase;
- `running` — lifecycle phase;
- `success` — invalid alias for `succeeded`;
- `timed_out` — canonical Runtime State, not invocation status;
- event names, Decision statuses, Runtime Object statuses, and all Runtime State names.

An implementation MAY maintain an internal invocation phase, but that phase is non-authoritative and MUST NOT be returned as Engine Result `status` or committed as Runtime State.

---

## 11. Authority Boundary

Every Engine MUST have a declared authority profile.

The authority profile MUST specify:

- objects it may read;
- objects it may create;
- objects it may propose updates to;
- fields it may update;
- actions it may request;
- actions it may never perform.

### 11.1 Default Authority Rules

Unless explicitly authorised, an Engine MAY:

- read approved Runtime Objects;
- create its owned output object;
- emit events;
- emit metrics;
- request approval;
- request exception handling;
- propose state transitions.

Unless explicitly authorised, an Engine MUST NOT:

- delete Runtime history;
- overwrite an approved Memory Record;
- modify another Engine's output;
- approve its own restricted decision;
- alter audit records;
- change source evidence;
- bypass the Orchestrator;
- change policy;
- suppress a blocking issue;
- mark an invalid output as valid.

### 11.2 Ownership Rule

Each Runtime Object type MUST have one primary owning Engine.

Other Engines may read the object and may propose changes, but MUST NOT directly modify it unless the capability matrix explicitly allows that modification.

---

## 12. Preconditions Contract

Before execution, every Engine MUST evaluate preconditions.

### 12.1 Minimum Preconditions

The Engine MUST verify:

- required Runtime Objects exist;
- input schemas are valid;
- configuration is valid;
- required policies are loaded;
- actor authority is sufficient;
- security access is permitted;
- privacy requirements are satisfied;
- dependencies are available;
- resource limits allow execution;
- execution is not cancelled;
- mandatory approval exists where required.

### 12.2 Precondition Outcomes

A precondition check MUST return one of:

```text
passed
passed_with_warning
waiting
blocked
failed
```

### 12.3 Failed Preconditions

If a mandatory precondition fails:

- execution SHALL NOT begin;
- the Engine MUST identify the failed condition;
- the Engine MUST state whether the failure is recoverable;
- the Engine MUST return `waiting`, `blocked`, or `failed`;
- the Engine MUST emit an event;
- an Exception Record MUST be created or requested where the failure is material.

---

## 13. Postconditions Contract

After successful execution, every Engine MUST guarantee:

- `primary_output` exists;
- all created Runtime Objects validate against their schemas;
- required provenance exists;
- confidence is assigned;
- warnings and blocking issues are explicit;
- side effects are recorded;
- state transition is valid;
- events are emitted;
- metrics are recorded;
- no unauthorised mutation occurred.

A postcondition failure converts the invocation to:

- `failed`; or
- `partial_success` only where partial completion is explicitly permitted.

---

## 14. Runtime State Contract

An Engine may only propose transitions allowed by the Runtime State Model.

### 14.1 State Transition Rules

Every proposed transition MUST include:

- current state;
- proposed next state;
- triggering Engine;
- triggering result;
- timestamp;
- validation outcome;
- approval requirement;
- rollback state where applicable.

A canonical evidence proposal MUST additionally include `transition_mechanism`, `dynamic_target`, `decision_id`, `approval_reference`, `policy_reference`, `checkpoint_reference`, `retry_reference`, `recovery_reference`, `rollback_reference`, and `origin_context`. Inapplicable references MUST be `null`; they MUST NOT be omitted. A static transition MUST use `transition_mechanism: static` and `dynamic_target: null`.

A governed dynamic-target proposal MUST use the mechanism registered by the State Model and MUST provide a non-null `dynamic_target`, authorising `decision_id`, and `origin_context`. Approval, policy, checkpoint resume, retry, recovery, and rollback mechanisms MUST also provide their corresponding non-null governance reference. Rollback additionally requires `checkpoint_reference`. These fields reference State Model and Decision Model evidence and MUST NOT redefine either model.

For `approval_resume_target`, `approval_reference` MUST identify the canonical Approval Decision owned by `decision_model.md`, with `object_type: approval_decision`. It MUST NOT identify an Approval Record. Approval Records are immutable process and audit records carried or referenced by Engine Context under `engine_context.md`.

The canonical machine structure is defined by `../../schemas/engine_result.schema.json#/$defs/state_transition_proposal`. Every `from_state`, `to_state`, and non-null `rollback_state` MUST exist in the State Model registry.

The interaction protocol is:

```text
Engine Invocation Status
↓ reported in
Engine Result
↓ optionally carries
State Transition Proposal
↓ independently validated by
Runtime Orchestrator
↓ commits or rejects
Runtime State change
```

An Engine Result status does not commit, select, or guarantee the proposed target State. A `succeeded` invocation does not imply workflow completion, and a `failed`, `blocked`, `cancelled`, or `timeout` invocation does not automatically imply a terminal Runtime State.

### 14.2 State Model Authority

`state_model.md` version `1.0.0` is the sole normative authority for Runtime state names, state categories, lifecycle paths, allowed transitions, conditional approval and policy paths, and terminal semantics.

This Engine Contract MUST NOT create or redefine Runtime states.

Every Engine MUST use the canonical lifecycle and transition registry defined by the State Model. Success, revision, failure, rejection, deferred, cancellation, timeout, exception, approval, and policy paths MUST follow the applicable State Model rules and MUST NOT be collapsed into invented convenience states.

An Engine MUST NOT skip a mandatory state unless:

- the State Model explicitly permits it;
- the reason is recorded;
- the required policy permits it;
- the Orchestrator validates the transition.

### 14.3 Commit Authority

An Engine proposes a state transition.

The Runtime Orchestrator commits the transition.

No Engine other than the Orchestrator may independently declare the global Runtime State committed.

---

## 15. Decision Contract

`decision_model.md` version `1.0.0` is the sole normative authority for Decision identity, required fields, types, confidence, evidence, approval requirements, lifecycle, validity, supersession, and retry, recovery, rollback, revision, approval, and policy Decision semantics.

This Engine Contract MUST NOT create, duplicate, or redefine universal Decision structure or lifecycle.

### 15.1 Engine Decision Obligations

An Engine MUST form, validate, retain, and reference Decisions exactly as required by `decision_model.md`.

An Engine MUST obtain an authorised Decision before it:

- performs a material execution action;
- selects a governed dynamic target;
- requests or acts on approval or policy outcomes;
- retries, recovers, rolls back, or revises operational work;
- creates a material side effect;
- proposes a terminal or supersession outcome.

Where a material Decision exists, Engine Result MUST reference the canonical Decision through `decision_ref`. Transition evidence MUST carry the authorising `decision_id` and the mechanism-specific references required by the State Model and Engine Result schema.

An Engine MUST NOT act without a required authorised Decision, invent Decision authority, substitute an Approval Record for an Approval Decision, use hidden reasoning as the Decision record, bypass Decision validation, or treat an Engine Result status as a Decision.

Reviewable summaries MUST follow the transparency and protected-reasoning boundaries owned by `decision_model.md`.

---

## 16. Confidence Contract

Decision confidence is governed exclusively by `decision_model.md`. This Engine Contract does not define its scale, fields, thresholds, lifecycle, or validity.

Where Engine Result requires a top-level `confidence` assessment, the Engine MUST return the structure defined by `../../schemas/engine_result.schema.json` and `../../schemas/common.schema.json#/$defs/confidence`. Engine Result confidence describes the invocation output assessment and MUST NOT replace, weaken, or redefine the referenced Decision's confidence.

An Engine MUST NOT use either Decision or Engine Result confidence to bypass evidence, approval, policy, security, privacy, quality, authority, or validation requirements.

---

## 17. Quality Contract

Every Engine owns the basic correctness of its own output.

An Engine MUST NOT delegate all output quality responsibility to the Quality Engine.

### 17.1 Local Quality Obligations

Every Engine MUST:

- validate output schema;
- check required fields;
- check internal consistency;
- verify provenance;
- assign confidence;
- emit warnings;
- emit blocking issues;
- confirm authority boundaries;
- confirm no prohibited side effect occurred.

### 17.2 Quality Engine Relationship

The local quality check verifies Engine-level correctness.

The Quality Engine performs independent, cross-object, business, policy, product, content, and release-level evaluation.

An Engine MUST NOT mark its output as globally approved unless it owns that approval authority.

### 17.3 Blocking Issues

An Engine MUST treat the following as blocking unless policy states otherwise:

- invalid output schema;
- missing required authoritative evidence;
- unauthorised action;
- security violation;
- privacy violation;
- unresolved mandatory approval;
- critical policy conflict;
- corrupt Runtime State;
- missing required dependency;
- invalid state transition.

---

## 18. Exception Contract

Material failures are Runtime Objects, not log messages.

### 18.1 Exception Trigger

An Engine MUST create or request an Exception Record when:

- execution fails;
- a mandatory gate blocks execution;
- repeated retry occurs;
- a dependency produces invalid data;
- an unauthorised state mutation is attempted;
- a critical quality issue is detected;
- a security or privacy issue occurs;
- a material conflict cannot be resolved;
- data integrity is uncertain;
- a partial success creates material risk.

### 18.2 Required Exception Information

The exception must identify:

- source Engine;
- execution ID;
- failure stage;
- failure category;
- severity;
- impact;
- evidence;
- suspected cause;
- containment;
- recovery action;
- retry eligibility;
- escalation requirement;
- owner.

### 18.3 Recovery Actions

Allowed recovery actions include:

```text
retry
retry_with_modified_context
fallback
request_input
request_approval
escalate
suspend
cancel
abort
```

### 18.4 Retry Rules

Retry MUST be bounded.

Every Engine MUST define:

- maximum attempts;
- retryable failure categories;
- delay policy;
- backoff policy;
- context changes allowed between attempts;
- escalation threshold.

An Engine MUST NOT retry indefinitely.

---

## 19. Approval Contract

Whenever approval is required, the Engine MUST stop at the approval boundary.

### 19.1 Required Behaviour

The Engine MUST:

1. identify the decision requiring approval;
2. identify the required authority;
3. produce or request an approval object;
4. return `waiting` or `blocked`;
5. preserve resumable state;
6. emit an approval-required event;
7. resume only after valid approval is supplied.

### 19.2 Prohibited Behaviour

An Engine MUST NOT:

- self-approve unless explicitly authorised;
- infer approval from silence;
- reuse expired approval;
- broaden approval beyond its approved scope;
- continue because confidence is high;
- substitute a warning for required approval.

### 19.3 Conditional Approval

Where approval includes conditions, the Engine MUST:

- load the conditions;
- apply them as execution constraints;
- validate compliance before completion;
- record whether each condition was satisfied.

---

## 20. Policy Contract

Every Engine MUST evaluate applicable policy before performing a governed action.

### 20.1 Policy Evaluation

The Engine MUST identify:

- applicable policies;
- policy version;
- match rationale;
- precedence;
- conflicts;
- required enforcement action.

### 20.2 Policy Conflict

An Engine MUST NOT silently choose between conflicting controlling policies.

It MUST:

- identify the conflict;
- apply the approved precedence rule;
- request Policy Engine resolution where necessary;
- block execution if the conflict remains unresolved and material.

---

## 21. Observability Contract

Every Engine invocation MUST be observable through structured events and metrics.

### 21.1 Required Events

At minimum, an Engine MUST emit:

```text
engine_context_received
engine_started
engine_preconditions_evaluated
engine_execution_completed
engine_output_validated
engine_state_transition_proposed
engine_completed
```

Where applicable:

```text
engine_warning_emitted
engine_blocked
engine_waiting_for_approval
engine_retry_scheduled
engine_fallback_started
engine_exception_created
engine_cancelled
engine_timeout
```

`engine_context_received`, `engine_started`, and other progress events represent invocation lifecycle phases. `engine_completed` records that an Engine Result was returned and MUST carry or correlate to its final invocation status. Status-specific events record the relevant condition but MUST NOT substitute for Engine Result `status`.

`engine_state_transition_proposed` records a proposal only. A separate Orchestrator-controlled Runtime Event MUST record whether the State Transition was committed or rejected. Neither event commits State by itself.

### 21.2 Required Event Fields

Each event MUST include:

- event ID;
- event type;
- Engine ID;
- Engine version;
- execution ID;
- task ID;
- workflow ID where applicable;
- trace ID;
- timestamp;
- status;
- summary;
- severity where applicable;
- related object IDs.

### 21.3 Logging Rules

Logs MUST:

- be structured where possible;
- avoid secrets;
- avoid unnecessary personal data;
- include trace correlation;
- distinguish warnings from failures;
- preserve causal ordering.

Logs MUST NOT be treated as the authoritative source of Runtime State.

---

## 22. Metrics Contract

Every Engine MUST measure performance and reliability.

### 22.1 Core Metrics

At minimum:

- invocation count;
- success count;
- partial success count;
- failure count;
- blocked count;
- cancellation count;
- timeout count;
- latency;
- retry count;
- warning count;
- blocking issue count;
- output validation failure count;
- average confidence;
- quality score where applicable.

### 22.2 Resource Metrics

Where measurable:

- token usage;
- model calls;
- tool calls;
- API calls;
- memory usage;
- storage usage;
- compute time;
- estimated cost;
- cache usage.

### 22.3 Business Metrics

An Engine MAY record domain-specific metrics, but MUST:

- define the metric;
- define the unit;
- define whether higher or lower is better;
- define the measurement source;
- avoid mixing estimated and measured values without labels.

---

## 23. Security Contract

Every Engine MUST respect the Runtime Security Context.

### 23.1 Required Security Controls

Every Engine MUST:

- validate actor access;
- validate object access;
- enforce least privilege;
- preserve confidentiality;
- preserve integrity;
- record restricted operations;
- avoid secret exposure;
- respect environment boundaries;
- reject unauthorised instructions.

### 23.2 Secret Handling

Secrets MUST NOT be:

- placed in prompts unless explicitly required and protected;
- written to logs;
- copied into Runtime Objects;
- included in error messages;
- exposed in metrics.

### 23.3 Integrity

Where integrity protection is required, the Engine MUST:

- preserve content hashes;
- verify signed or hashed inputs;
- report integrity mismatches;
- block use of corrupted controlling inputs.

---

## 24. Privacy Contract

Every Engine MUST respect the Runtime Privacy Context.

### 24.1 Required Privacy Controls

Every Engine MUST:

- identify personal data;
- minimise data use;
- respect purpose limitation;
- respect retention rules;
- support redaction;
- support deletion where required;
- avoid unnecessary replication;
- record lawful or approved retention basis where applicable.

### 24.2 Derived Data

Derived summaries, embeddings, classifications, and memory records may still contain personal data.

An Engine MUST NOT assume transformed data is non-personal.

---

## 25. Side-Effect Contract

Any operation that changes an external or persistent system is a side effect.

Examples:

- writing a file;
- updating a repository;
- sending a message;
- publishing content;
- modifying memory;
- creating an approval request;
- calling an external tool;
- changing a database record.

### 25.1 Side-Effect Rules

Every side effect MUST be:

- declared;
- authorised;
- idempotent where possible;
- attributable;
- observable;
- reversible where practical;
- recorded in Engine Result.

### 25.2 Idempotency

An Engine performing repeatable side effects SHOULD use an idempotency key.

Retries MUST NOT create duplicate external actions unless duplication is explicitly acceptable.

---

## 26. Cancellation, Timeout, and Resume

### 26.1 Cancellation

An Engine MUST periodically respect cancellation state for long-running work.

On cancellation, it MUST:

- stop safely;
- preserve completed valid work;
- avoid further side effects;
- emit a cancellation event;
- return `cancelled`.

### 26.2 Timeout

Every Engine invocation MUST have an approved timeout or inherit one from Runtime configuration.

On timeout, it MUST:

- stop or be terminated;
- record incomplete work;
- identify resumability;
- return `timeout`;
- request an Exception Record where material.

### 26.3 Resume

A resumable Engine MUST store:

- checkpoint ID;
- completed stage;
- pending stage;
- required inputs;
- side effects already committed;
- retry count;
- approval state;
- Engine version.

Resume MUST NOT assume that prior context is still current.

The Engine MUST revalidate:

- policy;
- approval;
- security;
- time-sensitive sources;
- dependencies;
- configuration.

---

## 27. Versioning and Compatibility

Every Engine MUST declare:

- Engine ID;
- Engine version;
- Engine Contract version;
- supported input schema versions;
- supported output schema versions;
- configuration version;
- policy compatibility;
- deprecation status.

### 27.1 Semantic Versioning

Engine versions SHOULD use:

```text
MAJOR.MINOR.PATCH
```

- **MAJOR** — breaking interface or behaviour change;
- **MINOR** — backward-compatible capability;
- **PATCH** — backward-compatible correction.

### 27.2 Compatibility Check

Before execution, the Runtime MUST verify:

- Engine Contract compatibility;
- input schema compatibility;
- configuration compatibility;
- Runtime State compatibility;
- policy compatibility.

### 27.3 Deprecation

A deprecated Engine MUST define:

- replacement Engine;
- deprecation date;
- removal date where known;
- migration guidance;
- affected workflows;
- compatibility limits.

---

## 28. Engine Capability Matrix

The capability matrix defines the default authority of each Engine.

`R` = Read
`C` = Create
`U` = Update within owned authority
`P` = Propose
`A` = Approve
`X` = Prohibited by default

| Engine | Primary Object | Read Runtime Objects | Create Primary Object | Update Primary Object | Approve | Commit Global State |
|---|---|---:|---:|---:|---:|---:|
| Retrieval Engine | Knowledge Package | R | C | U | X | X |
| Business Judgement Engine | Business Judgement | R | C | U | X | X |
| Planning Engine | Execution Blueprint | R | C | U | X | X |
| Execution Engine | Skill Output / Execution Result | R | C | U | X | X |
| Quality Engine | Quality Report | R | C | U | P only | X |
| Exception Engine | Exception Record | R | C | U | X | X |
| Learning Engine | Learning Candidate | R | C | U | P only | X |
| Memory Engine | Memory Record | R | C | U | X by default | X |
| Approval Engine | Approval Decision | R | C | U | A within policy | X |
| Policy Engine | Policy Decision | R | C | U | A within policy | X |
| Runtime Orchestrator | Runtime State | R | C | U | X | A |

### 28.1 Capability Rules

The matrix defines defaults.

Any expanded capability requires:

- explicit configuration;
- documented reason;
- owner;
- approval;
- auditability;
- tests.

### 28.2 Memory Protection

Approved or active Memory Records MUST NOT be overwritten.

Changes MUST occur through:

- new revision;
- new version;
- supersession;
- deprecation;
- controlled lifecycle update.

---

## 29. Engine Interaction Rules

Engines communicate through the Runtime Orchestrator and Runtime Objects.

### 29.1 Standard Interaction Pattern

```text
Engine A
↓
Engine Result
↓
Runtime Store
↓
Orchestrator validates and commits
↓
Engine B receives updated Engine Context
```

### 29.2 Prohibited Interaction

An Engine MUST NOT:

- call another Engine's private method;
- mutate another Engine's in-memory state;
- bypass Runtime Store for material outputs;
- pass undocumented hidden context;
- rely on another Engine's prompt history;
- directly commit global Runtime State;
- suppress another Engine's blocking result.

### 29.3 Direct Calls

A direct Engine-to-Engine call MAY be permitted only when:

- the interaction is registered;
- the called interface is public;
- the Orchestrator authorises the call;
- the call is traceable;
- results are returned as Runtime Objects or Engine Results;
- no authority boundary is bypassed.

### 29.4 Dependency Declaration

Every Engine MUST declare:

- required Engines;
- optional Engines;
- required tools;
- required schemas;
- required policies;
- required registries;
- fallback dependencies.

Circular Engine dependencies MUST be rejected unless explicitly designed and bounded.

---

## 30. Engine Registration

Every Engine MUST be registered before production use.

### 30.1 Required Registry Fields

The Engine Registry MUST include:

- Engine ID;
- display name;
- purpose;
- owner;
- version;
- contract version;
- implementation location;
- enabled status;
- supported workflows;
- input object types;
- output object types;
- capability profile;
- timeout;
- retry policy;
- security classification;
- configuration reference;
- health status;
- deprecation status.

### 30.2 Unregistered Engines

An unregistered Engine MUST NOT:

- execute in production;
- create authoritative Runtime Objects;
- perform persistent side effects;
- access restricted Runtime data;
- participate in approved workflows.

---

## 31. Configuration Contract

Engine behaviour MUST be controlled through versioned configuration where appropriate.

Configuration SHOULD include:

- thresholds;
- weights;
- limits;
- routing rules;
- retry policy;
- timeout;
- quality gates;
- model settings;
- feature flags;
- fallback rules.

### 31.1 Configuration Rules

Configuration MUST:

- be schema validated;
- be versioned;
- be traceable to an execution;
- have an owner;
- distinguish environment-specific values;
- avoid secrets in plain text;
- define safe defaults.

An Engine MUST NOT silently invent missing critical configuration.

---

## 32. Testing Requirements

Every Engine MUST have a test suite appropriate to its risk.

### 32.1 Minimum Test Categories

- contract compliance tests;
- schema validation tests;
- unit tests;
- boundary tests;
- failure tests;
- blocked-state tests;
- retry tests;
- fallback tests;
- timeout tests;
- cancellation tests;
- recovery tests;
- replay tests;
- regression tests;
- policy tests;
- approval tests;
- security tests;
- privacy tests;
- observability tests;
- state transition tests;
- compatibility tests;
- performance tests where applicable.

### 32.2 Golden Cases

Each Engine SHOULD maintain approved golden cases containing:

- context fixture;
- expected decision;
- expected Runtime Object;
- expected status;
- expected events;
- expected state transition;
- expected warnings;
- prohibited outputs.

### 32.3 Test Reproducibility

Tests involving stochastic models MUST define:

- model;
- version;
- deterministic settings where supported;
- tolerance;
- evaluation method;
- pass threshold.

---

## 33. Acceptance Criteria

An Engine is production-ready only when all mandatory criteria are met.

### 33.1 Contract Compliance

- [ ] Conforms to this Engine Contract.
- [ ] Declares responsibilities and non-responsibilities.
- [ ] Declares authority boundaries.
- [ ] Uses Engine Context and Engine Result.
- [ ] Produces schema-valid Runtime Objects.
- [ ] Uses approved state transitions.

### 33.2 Governance

- [ ] Policy evaluation is implemented.
- [ ] Approval boundaries are implemented.
- [ ] Security controls are implemented.
- [ ] Privacy controls are implemented.
- [ ] Audit events are emitted.

### 33.3 Reliability

- [ ] Failure behaviour is explicit.
- [ ] Retry is bounded.
- [ ] Timeout is configured.
- [ ] Cancellation is supported where applicable.
- [ ] Recovery or fallback is defined.
- [ ] Idempotency is addressed for side effects.

### 33.4 Quality

- [ ] Local output validation exists.
- [ ] Confidence is assigned.
- [ ] Warnings and blocking issues are structured.
- [ ] Provenance is recorded.
- [ ] Required quality gates pass.

### 33.5 Operations

- [ ] Engine is registered.
- [ ] Metrics are emitted.
- [ ] Logs are traceable.
- [ ] Version compatibility is declared.
- [ ] Tests pass.
- [ ] Documentation is complete.
- [ ] Owner is assigned.

---

## 34. Required Engine Documentation

Every Engine MUST provide a primary specification using the following structure:

```markdown
# Engine Name

## 1. Purpose
## 2. Responsibilities
## 3. Non-Responsibilities
## 4. Owned Runtime Objects
## 5. Authority Boundary
## 6. Inputs
## 7. Outputs
## 8. Dependencies
## 9. Preconditions
## 10. Processing Stages
## 11. Decision Logic
## 12. State Transitions
## 13. Quality Gates
## 14. Failure Modes
## 15. Exception Handling
## 16. Approval Requirements
## 17. Policy Requirements
## 18. Observability
## 19. Metrics
## 20. Security and Privacy
## 21. Configuration
## 22. Versioning
## 23. Examples
## 24. Acceptance Tests
```

An Engine specification MUST reference this contract rather than redefine universal rules inconsistently.

---

## 35. Prohibited Engine Behaviours

The following behaviours are prohibited:

- returning raw text as the only Engine Result;
- mutating Engine Context;
- modifying another Engine's owned object without authority;
- bypassing mandatory approval;
- bypassing policy;
- hiding material failure;
- creating unaudited side effects;
- using hidden prompt memory as authoritative state;
- inventing missing authoritative evidence;
- committing global Runtime State outside the Orchestrator;
- silently resolving controlling policy conflicts;
- silently lowering a blocking issue to a warning;
- retrying indefinitely;
- overwriting approved Memory Records;
- exposing secrets in outputs or logs;
- claiming success after failed output validation;
- using an unregistered Engine in production.

---

## 36. Compliance and Audit

Engine compliance MUST be reviewable.

### 36.1 Compliance Evidence

The Runtime SHOULD retain:

- Engine registration;
- version history;
- configuration snapshots;
- policy snapshots;
- test reports;
- execution traces;
- validation results;
- exception history;
- approval history;
- state transition history;
- audit logs.

### 36.2 Non-Compliance

If an Engine is found non-compliant, the Runtime MAY:

- warn;
- restrict capability;
- suspend the Engine;
- block production execution;
- require remediation;
- deprecate the Engine;
- replace the Engine.

Critical security, privacy, authority, integrity, or state violations SHOULD cause immediate suspension.

---

## 37. Contract Change Management

Changes to this contract affect every Runtime Engine.

### 37.1 Change Requirements

A contract change MUST include:

- change summary;
- rationale;
- compatibility assessment;
- affected Engines;
- migration plan;
- test updates;
- approval;
- effective date;
- deprecation period where required.

### 37.2 Breaking Changes

A breaking change requires:

- major version increment;
- migration guidance;
- compatibility testing;
- Engine Registry update;
- Runtime Orchestrator compatibility update;
- explicit production rollout approval.

---

## 38. Summary

Every Runtime Engine must appear consistent from the Runtime's perspective.

Each Engine may have different internal decision logic, but all Engines share:

- one context contract;
- one result contract;
- one lifecycle model;
- one status model;
- explicit authority;
- explicit state transitions;
- structured decisions;
- quality obligations;
- exception obligations;
- approval obligations;
- observability;
- security;
- privacy;
- versioning;
- testing;
- governance.

The Engine Contract is the constitutional standard of the Scout Runtime Engine Layer.

No Engine is production-ready until it complies with this document.
