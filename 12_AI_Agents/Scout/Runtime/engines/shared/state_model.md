# State Model

**Document ID:** `SCOUT-RUNTIME-STATE-MODEL`  
**Version:** `1.0.0`  
**Status:** `Approved`  
**Owner:** `Scout Runtime`  
**Applies To:** Runtime Orchestrator and all registered Runtime Engines  
**Dependencies:** `engine_contract.md`, `engine_context.md`  
**Specification Type:** Mandatory Shared Foundation Standard  

---

## 1. Purpose

This document defines the authoritative lifecycle and state-transition model for Scout Runtime.

The State Model establishes:

- the canonical Runtime states;
- state categories;
- state ownership;
- entry and exit rules;
- transition rules;
- approval, policy, quality, and security gates;
- waiting, blocking, retry, recovery, rollback, timeout, and cancellation behaviour;
- parallel and composite state behaviour;
- terminal outcomes;
- state history, observability, integrity, and governance.

Every Runtime execution MUST have one current committed state.

Only the Runtime Orchestrator may commit a global state transition.

---

## 2. Scope

This specification applies to:

- Task lifecycle;
- Runtime execution lifecycle;
- Engine invocation lifecycle;
- Runtime Object readiness;
- approval and policy gates;
- quality control;
- exception handling;
- learning and memory processing;
- cancellation, timeout, retry, recovery, and rollback;
- workflow completion and closure.

It does not define the internal algorithm of an Engine or the physical persistence implementation.

---

## 3. Normative Language

- **MUST / SHALL** — mandatory.
- **MUST NOT / SHALL NOT** — prohibited.
- **SHOULD** — recommended unless a documented exception exists.
- **SHOULD NOT** — discouraged unless justified.
- **MAY** — optional.
- **REQUIRED** — mandatory.

A deviation from a mandatory rule requires documented authority, approval, scope, reason, audit record, and review or expiry date.

---

## 4. State Model Principles

### 4.1 Single Committed State

Each execution MUST have one committed top-level state at any point in time.

Composite and parallel substates MAY exist, but they MUST roll up to one authoritative top-level state.

### 4.2 Orchestrator Authority

Engines may propose transitions. Only the Runtime Orchestrator may validate and commit them.

### 4.3 Explicit Transitions

State MUST NOT change implicitly.

Every transition MUST be represented by a transition record.

### 4.4 Guarded Movement

A transition occurs only when its entry guards, policy gates, approval gates, security checks, and required preconditions pass.

### 4.5 Immutable History

Committed state history is append-only.

### 4.6 Recoverability

Non-terminal failures SHOULD provide a defined retry, recovery, rollback, escalation, or cancellation path.

### 4.7 Deterministic Meaning

A state name MUST retain one semantic meaning across Engines and workflows.

### 4.8 No State by Convention

Terms such as `done`, `finished`, or `complete` MUST NOT substitute for registered canonical states.

---

## 5. State Architecture

```text
Runtime State
├── Identity
├── Category
├── Lifecycle Position
├── Owner
├── Entry Conditions
├── Exit Conditions
├── Allowed Transitions
├── Gates
├── Blocking Conditions
├── Retry and Recovery
├── Timeout and Cancellation
├── State Data
├── History
├── Observability
├── Integrity
└── Extensions
```

Canonical logical structure:

```json
{
  "state_id": "state_exec_001_v4",
  "schema_name": "runtime_state",
  "schema_version": "1.0.0",
  "state_version": 4,
  "execution_id": "exec_001",
  "workflow_id": "wf_001",
  "current_state": "knowledge_ready",
  "state_category": "readiness",
  "previous_state": "retrieval_running",
  "entered_at": "2026-07-28T10:00:00Z",
  "state_owner": "runtime_orchestrator",
  "allowed_transitions": [
    "business_judgement_running",
    "blocked",
    "cancelled"
  ],
  "is_terminal": false,
  "is_paused": false,
  "is_cancelled": false,
  "blocking_conditions": [],
  "pending_transition": null,
  "checkpoint": null,
  "integrity": {
    "algorithm": "sha256",
    "checksum": "sha256:example",
    "status": "verified"
  }
}
```

---

## 6. State Categories

Canonical categories:

```text
initialisation
readiness
active
waiting
blocked
approval
quality
exception
recovery
learning
memory
terminal
administrative
```

Category meaning:

- `initialisation` — execution is being created or validated;
- `readiness` — a required Runtime Object or stage is ready;
- `active` — an Engine or Runtime operation is executing;
- `waiting` — progress is paused pending a non-error dependency;
- `blocked` — progress cannot continue because a mandatory condition failed;
- `approval` — progress depends on an authority decision;
- `quality` — output is under quality evaluation;
- `exception` — an error or abnormal condition is being handled;
- `recovery` — Runtime is retrying, resuming, or restoring;
- `learning` — learning evaluation is active or complete;
- `memory` — memory eligibility or persistence is active;
- `terminal` — execution has ended;
- `administrative` — archived, superseded, or migrated state.

---

## 7. Canonical Runtime Lifecycle

```text
task_received
↓
context_building
↓
context_ready
↓
retrieval_running
↓
knowledge_ready
↓
business_judgement_running
↓
business_judgement_ready
↓
planning_running
↓
execution_blueprint_ready
↓
execution_running
↓
execution_result_ready
↓
quality_running
↓
quality_passed
↓
learning_running
↓
learning_evaluated
↓
memory_running
↓
memory_updated
↓
workflow_completed
↓
workflow_closed
```

Alternative controlled paths may include waiting, approval, blocked, exception, retry, recovery, rollback, cancellation, timeout, quality failure, and memory rejection.

---

## 8. Core State Registry

### 8.1 Initialisation States

- `task_received`
- `task_validating`
- `task_validated`
- `context_building`
- `context_validating`
- `context_ready`

### 8.2 Retrieval States

- `retrieval_pending`
- `retrieval_running`
- `retrieval_waiting`
- `knowledge_ready`
- `knowledge_incomplete`
- `retrieval_failed`

### 8.3 Business Judgement States

- `business_judgement_pending`
- `business_judgement_running`
- `business_judgement_ready`
- `business_judgement_blocked`
- `business_judgement_failed`

### 8.4 Planning States

- `planning_pending`
- `planning_running`
- `execution_blueprint_ready`
- `planning_blocked`
- `planning_failed`

### 8.5 Execution States

- `execution_pending`
- `execution_running`
- `execution_waiting`
- `execution_paused`
- `execution_result_ready`
- `execution_failed`

### 8.6 Quality States

- `quality_pending`
- `quality_running`
- `quality_passed`
- `quality_failed`
- `quality_revision_required`
- `quality_waiver_pending`
- `quality_waived`

### 8.7 Learning and Memory States

- `learning_pending`
- `learning_running`
- `learning_evaluated`
- `learning_rejected`
- `memory_pending`
- `memory_running`
- `memory_updated`
- `memory_rejected`
- `memory_deferred`

### 8.8 Governance States

- `approval_pending`
- `approval_granted`
- `approval_denied`
- `approval_expired`
- `policy_evaluation_pending`
- `policy_evaluation_running`
- `policy_passed`
- `policy_blocked`

### 8.9 Exception and Recovery States

- `exception_detected`
- `exception_classifying`
- `retry_pending`
- `retrying`
- `recovery_pending`
- `recovering`
- `rollback_pending`
- `rolling_back`
- `rollback_completed`
- `manual_intervention_required`

### 8.10 Terminal States

- `workflow_completed`
- `workflow_closed`
- `cancelled`
- `timed_out`
- `failed_terminal`
- `rejected_terminal`
- `superseded`
- `archived`

---

## 9. State Identity

A State Record MUST contain:

- `state_id`;
- `execution_id`;
- `state_version`;
- `current_state`;
- `entered_at`;
- `state_owner`;
- `schema_version`.

Recommended identity pattern:

```text
state_<execution_id>_v<state_version>
```

`state_version` MUST increase monotonically for each committed transition.

---

## 10. State Metadata

Recommended metadata:

- workflow ID;
- Task ID;
- Context ID;
- current Engine;
- previous state;
- transition ID;
- entered at;
- expected exit time;
- priority;
- classification;
- state reason;
- state annotations;
- checkpoint ID;
- parent execution ID;
- correlation IDs.

Metadata MUST NOT alter the semantic meaning of the canonical state.

---

## 11. State Ownership

The Runtime Orchestrator owns committed top-level state.

Specific Engines may own execution responsibility while a state is active:

| State Family | Responsible Engine |
|---|---|
| Retrieval | Retrieval Engine |
| Business Judgement | Business Judgement Engine |
| Planning | Planning Engine |
| Execution | Execution Engine |
| Quality | Quality Engine |
| Exception | Exception Engine |
| Learning | Learning Engine |
| Memory | Memory Engine |
| Approval | Approval Engine |
| Policy | Policy Engine |

Responsibility does not grant transition-commit authority.

---

## 12. Entry Rules

A state entry MUST define:

- permitted predecessor states;
- mandatory Runtime Objects;
- required Engine Context;
- policy outcome;
- approval outcome;
- security and privacy status;
- resource availability;
- freshness requirements;
- integrity requirements;
- entry actor;
- entry reason.

Entry is invalid if any mandatory guard fails.

---

## 13. Exit Rules

A state exit MUST define:

- success criteria;
- required Engine Result;
- produced Runtime Objects;
- quality conditions;
- approval conditions;
- cleanup actions;
- checkpoint requirements;
- observability events;
- next-state candidates.

An active state MUST NOT be exited merely because an Engine stopped responding.

---

## 14. Transition Model

A transition is an immutable record representing a proposed or committed move between states.

```json
{
  "transition_id": "tr_001",
  "schema_name": "state_transition",
  "schema_version": "1.0.0",
  "execution_id": "exec_001",
  "from_state": "retrieval_running",
  "to_state": "knowledge_ready",
  "proposed_by": {
    "actor_type": "engine",
    "actor_id": "retrieval_engine"
  },
  "proposed_at": "2026-07-28T10:00:00Z",
  "reason_code": "knowledge_package_created",
  "guards": [
    {
      "guard_id": "knowledge_package_valid",
      "status": "passed"
    },
    {
      "guard_id": "policy_gate",
      "status": "passed"
    }
  ],
  "validation_status": "passed",
  "commit_status": "committed",
  "committed_by": "runtime_orchestrator",
  "committed_at": "2026-07-28T10:00:01Z"
}
```

---

## 15. Transition Validation

Validation MUST check:

1. source state equals the current committed state;
2. target state is registered;
3. transition is allowed;
4. proposer has authority;
5. required Runtime Objects exist;
6. entry guards pass;
7. policy gates pass;
8. approval gates pass;
9. security and privacy controls pass;
10. integrity checks pass;
11. freshness requirements pass;
12. resource conditions are acceptable;
13. no conflicting transition is already committed;
14. version concurrency is valid.

A failed validation MUST prevent commit.

---

## 16. Allowed Transition Registry

A registry entry SHOULD define:

```json
{
  "from_state": "planning_running",
  "to_states": [
    "execution_blueprint_ready",
    "planning_blocked",
    "planning_failed",
    "approval_pending",
    "cancelled",
    "timed_out"
  ],
  "owner": "runtime_orchestrator",
  "requires_engine_result": true,
  "requires_checkpoint": false,
  "policy_gate": true,
  "approval_gate": "conditional"
}
```

Unregistered transitions are prohibited unless an approved migration or emergency rule applies.

---

## 17. Composite States

A composite state contains substates while preserving one top-level lifecycle position.

Example:

```text
execution_running
├── tool_preparation
├── skill_execution
├── output_assembly
└── result_validation
```

Composite substate changes MUST NOT imply a top-level transition unless the Orchestrator commits it.

---

## 18. Parallel States

Parallel execution MAY be used when independent branches can run safely.

Example:

```text
execution_running
├── branch_a: content_generation
├── branch_b: image_planning
└── branch_c: compliance_precheck
```

A parallel state MUST define:

- branch IDs;
- branch ownership;
- join condition;
- failure policy;
- cancellation propagation;
- resource allocation;
- partial-result rules;
- branch timeout;
- observability correlation.

Join strategies:

```text
all_required
minimum_required
first_success
quorum
policy_defined
```

---

## 19. Waiting States

Waiting is non-terminal and does not imply failure.

Examples:

- external dependency;
- user input;
- approval;
- scheduled availability;
- rate limit;
- resource allocation;
- tool response;
- data refresh.

A waiting state MUST include reason, dependency, owner, entered time, wake condition, maximum wait, timeout action, and resume target.

---

## 20. Blocking States

Blocking means a mandatory condition prevents safe continuation.

A blocked state MUST include:

- blocking reason;
- controlling rule;
- evidence;
- responsible resolver;
- remediation options;
- escalation path;
- re-evaluation condition;
- terminal fallback.

Blocked execution MUST NOT silently continue.

---

## 21. Approval States

Approval flow:

```text
approval_pending
├── approval_granted
├── approval_denied
├── approval_expired
└── cancelled
```

Approval scope, authority, conditions, expiry, and evidence MUST be preserved.

`approval_granted` does not automatically bypass policy, security, privacy, or quality gates unless the controlling standard explicitly permits a waiver.

---

## 22. Policy States

Policy flow:

```text
policy_evaluation_pending
↓
policy_evaluation_running
├── policy_passed
└── policy_blocked
```

Policy evaluation MUST identify applicable policies, precedence, conflicts, enforcement modes, and decision evidence.

A blocking policy outcome cannot be overridden by Engine preference.

---

## 23. Quality States

Quality flow:

```text
quality_pending
↓
quality_running
├── quality_passed
├── quality_failed
├── quality_revision_required
└── quality_waiver_pending
    ├── quality_waived
    └── quality_failed
```

A Quality Report MUST support every quality transition.

`quality_failed` and `quality_revision_required` are distinct:

- failure indicates unacceptable output;
- revision required indicates output may be corrected and re-evaluated.

---

## 24. Retry States

Retry flow:

```text
exception_detected
↓
retry_pending
↓
retrying
├── prior_active_state
├── recovery_pending
└── failed_terminal
```

Retry policy MUST define:

- eligible error classes;
- maximum attempts;
- delay;
- backoff;
- jitter;
- idempotency;
- checkpoint usage;
- resource budget;
- escalation threshold.

A retry MUST create a new attempt identity and preserve the original execution identity.

---

## 25. Recovery States

Recovery is broader than retry and may involve reconfiguration, dependency replacement, Context rebuilding, checkpoint restoration, or controlled degradation.

Recovery MUST record:

- failure origin;
- recovery strategy;
- changed dependencies;
- compatibility impact;
- security impact;
- data integrity impact;
- recovery result;
- next state.

---

## 26. Rollback States

Rollback restores a prior valid checkpoint or committed state.

Flow:

```text
rollback_pending
↓
rolling_back
├── rollback_completed
└── manual_intervention_required
```

Rollback MUST NOT erase history.

Rollback MUST identify target checkpoint, reversible side effects, irreversible effects, compensation actions, integrity verification, and post-rollback state.

---

## 27. Cancellation

Cancellation may be requested by:

- authorised human actor;
- Runtime Orchestrator;
- policy;
- security control;
- timeout policy;
- superseding workflow.

Cancellation MUST distinguish:

- request received;
- cancellation in progress;
- side-effect cleanup;
- cancellation committed.

The canonical terminal state is `cancelled`.

---

## 28. Timeout

Timeout occurs when a configured hard time boundary is exceeded.

Timeout handling MUST identify:

- timed operation;
- configured limit;
- elapsed time;
- partial results;
- retry eligibility;
- recovery eligibility;
- cleanup status;
- terminal or resumable outcome.

The canonical terminal state is `timed_out` when no recovery path remains.

---

## 29. Pause and Resume

Pause is a controlled suspension, not a failure.

A paused execution MUST preserve:

- checkpoint;
- pause reason;
- actor;
- pause time;
- resource disposition;
- side-effect status;
- resume conditions;
- expiry.

Resume MUST rebuild or revalidate Engine Context before returning to active execution.

---

## 30. Exception States

Exception flow:

```text
exception_detected
↓
exception_classifying
├── retry_pending
├── recovery_pending
├── rollback_pending
├── manual_intervention_required
└── failed_terminal
```

Exception classification MUST distinguish transient, permanent, policy, approval, security, privacy, data, integrity, dependency, resource, quality, and unknown exceptions.

---

## 31. Learning States

Learning flow:

```text
learning_pending
↓
learning_running
├── learning_evaluated
└── learning_rejected
```

`learning_evaluated` means a Learning Candidate has been assessed. It does not mean it has become approved memory.

---

## 32. Memory States

Memory flow:

```text
memory_pending
↓
memory_running
├── memory_updated
├── memory_rejected
└── memory_deferred
```

Memory persistence MUST follow Memory Engine policy, authority, privacy, lifecycle, conflict, and evidence requirements.

---

## 33. Terminal States

Terminal states:

- `workflow_closed`;
- `cancelled`;
- `timed_out`;
- `failed_terminal`;
- `rejected_terminal`;
- `superseded`;
- `archived`.

`workflow_completed` indicates successful functional completion but MAY still allow administrative closure actions.

`workflow_closed` is the normal successful terminal state.

No transition may leave a terminal state except an explicitly governed administrative transition such as `workflow_closed → archived`.

---

## 34. Completion Semantics

Completion levels:

```text
stage_completed
workflow_completed
workflow_closed
archived
```

A stage completion MUST NOT be treated as workflow completion.

Workflow completion requires all mandatory stages, quality gates, approvals, and required outputs.

Workflow closure additionally requires final records, observability completion, retention assignment, and unresolved issue review.

---

## 35. State History

State history is append-only and MUST include:

- state version;
- state name;
- category;
- entered time;
- exited time;
- transition ID;
- transition reason;
- responsible Engine;
- committing actor;
- Context ID;
- Runtime Object references;
- checkpoint;
- integrity status.

```json
{
  "execution_id": "exec_001",
  "history": [
    {
      "state_version": 1,
      "state": "task_received",
      "entered_at": "2026-07-28T09:00:00Z",
      "exited_at": "2026-07-28T09:00:01Z",
      "transition_id": "tr_001"
    },
    {
      "state_version": 2,
      "state": "context_building",
      "entered_at": "2026-07-28T09:00:01Z",
      "exited_at": "2026-07-28T09:00:03Z",
      "transition_id": "tr_002"
    },
    {
      "state_version": 3,
      "state": "context_ready",
      "entered_at": "2026-07-28T09:00:03Z",
      "exited_at": null,
      "transition_id": "tr_003"
    }
  ]
}
```

---

## 36. State Versioning

Every committed transition increments `state_version`.

Concurrent transition proposals MUST use optimistic or equivalent concurrency control.

A proposal with a stale expected version MUST be rejected and re-evaluated.

Canonical requirement:

```text
proposed.expected_state_version == committed.state_version
```

---

## 37. State Integrity

Integrity controls may include:

- canonical JSON hash;
- transition hash chain;
- signed transition records;
- immutable event log;
- state snapshot checksum;
- Runtime Object hash verification.

Integrity status:

```text
verified
verified_with_warning
unverified
mismatch
corrupt
```

A mismatch or corrupt state MUST block normal execution.

---

## 38. State Freshness

State freshness indicates whether the current state remains operationally valid.

A state may become stale because:

- approval expired;
- policy changed;
- security claim expired;
- Context expired;
- knowledge became stale;
- dependency changed;
- timeout threshold passed;
- configuration changed materially.

A stale state MUST be revalidated before active execution continues.

---

## 39. State Priority

Priority influences scheduling but does not weaken controls.

Recommended levels:

```text
critical
high
normal
low
deferred
```

Priority MAY affect queue order, resource allocation, retry timing, and escalation.

Priority MUST NOT bypass policy, approval, security, privacy, quality, or integrity gates.

---

## 40. State Governance

### Runtime Orchestrator

- owns committed state;
- validates and commits transitions;
- prevents conflicting commits;
- maintains history;
- coordinates recovery and terminal closure.

### Engines

- execute within current state;
- return Engine Result;
- propose next state;
- report blocking conditions;
- never directly mutate committed state.

### Approval Engine

- governs approval state and evidence.

### Policy Engine

- governs policy outcomes and precedence.

### Quality Engine

- governs quality state transitions.

### Exception Engine

- classifies failures and proposes retry, recovery, rollback, escalation, or termination.

---

## 41. State Observability

Every transition MUST emit a Runtime Event.

Required observability fields:

- trace ID;
- execution ID;
- transition ID;
- from state;
- to state;
- proposed by;
- committed by;
- timestamps;
- duration in prior state;
- reason code;
- guard outcomes;
- retry count;
- error reference;
- Context ID;
- produced Runtime Objects.

Metrics SHOULD include state duration, transition frequency, blocked time, waiting time, retry rate, recovery success, rollback rate, terminal outcomes, and transition validation failures.

---

## 42. Transition Reason Codes

Recommended reason-code families:

```text
task_*
context_*
knowledge_*
judgement_*
planning_*
execution_*
quality_*
approval_*
policy_*
exception_*
retry_*
recovery_*
rollback_*
learning_*
memory_*
cancel_*
timeout_*
administrative_*
```

Reason codes MUST be registered, stable, and machine-readable.

Free-text explanation MAY accompany a reason code but MUST NOT replace it.

---

## 43. State Snapshot

A State Snapshot is an immutable representation of committed state at a point in time.

```json
{
  "snapshot_id": "state_snapshot_001",
  "execution_id": "exec_001",
  "state_version": 8,
  "current_state": "execution_blueprint_ready",
  "created_at": "2026-07-28T10:10:00Z",
  "context_id": "ctx_exec_001_planning_004",
  "runtime_object_refs": [
    "task_001",
    "kp_001",
    "bj_001",
    "blueprint_001"
  ],
  "checkpoint_id": "checkpoint_004",
  "integrity": {
    "algorithm": "sha256",
    "checksum": "sha256:example",
    "status": "verified"
  }
}
```

Snapshots support replay, recovery, rollback, audit, and regression testing.

---

## 44. Enterprise Transition Example

```json
{
  "transition_id": "tr_exec_001_009",
  "schema_name": "state_transition",
  "schema_version": "1.0.0",
  "execution_id": "exec_001",
  "workflow_id": "wf_content_001",
  "expected_state_version": 8,
  "from_state": "execution_blueprint_ready",
  "to_state": "execution_running",
  "proposed_by": {
    "actor_type": "runtime",
    "actor_id": "runtime_orchestrator"
  },
  "proposed_at": "2026-07-28T10:15:00Z",
  "reason_code": "execution_started",
  "context_id": "ctx_exec_001_execution_009",
  "guards": [
    {
      "guard_id": "blueprint_present",
      "status": "passed",
      "evidence_ref": "blueprint_001"
    },
    {
      "guard_id": "policy_gate",
      "status": "passed",
      "evidence_ref": "policy_decision_001"
    },
    {
      "guard_id": "approval_gate",
      "status": "passed",
      "evidence_ref": null
    },
    {
      "guard_id": "security_gate",
      "status": "passed",
      "evidence_ref": "security_check_001"
    },
    {
      "guard_id": "resource_gate",
      "status": "passed",
      "evidence_ref": "resource_check_001"
    }
  ],
  "validation_status": "passed",
  "commit_status": "committed",
  "committed_by": "runtime_orchestrator",
  "committed_at": "2026-07-28T10:15:00Z",
  "new_state_version": 9,
  "observability": {
    "trace_id": "trace_001",
    "span_id": "span_transition_009"
  }
}
```

---

## 45. Transition Validation Result

```json
{
  "transition_id": "tr_exec_001_009",
  "validation_status": "passed",
  "validated_at": "2026-07-28T10:15:00Z",
  "validator_version": "1.0.0",
  "checks": [
    {
      "check": "source_state_matches",
      "status": "passed"
    },
    {
      "check": "target_state_registered",
      "status": "passed"
    },
    {
      "check": "transition_allowed",
      "status": "passed"
    },
    {
      "check": "entry_guards",
      "status": "passed"
    },
    {
      "check": "policy_gate",
      "status": "passed"
    },
    {
      "check": "approval_gate",
      "status": "passed"
    },
    {
      "check": "security_privacy",
      "status": "passed"
    },
    {
      "check": "state_version_concurrency",
      "status": "passed"
    }
  ],
  "warnings": [],
  "blocking_issues": []
}
```

---

## 46. Acceptance Criteria

A production State Model implementation is acceptable only when:

### Registry

- [ ] Every state is registered.
- [ ] Every state has one semantic definition.
- [ ] Every transition is registered.
- [ ] Every terminal state is identified.

### Authority

- [ ] Only the Orchestrator commits top-level state.
- [ ] Engines propose transitions through Engine Result.
- [ ] Actor authority is validated.

### Transition Safety

- [ ] Source state is current.
- [ ] Target state is permitted.
- [ ] Entry and exit rules pass.
- [ ] Policy and approval gates pass.
- [ ] Security and privacy checks pass.
- [ ] Integrity and freshness checks pass.
- [ ] Concurrency is controlled.

### Recovery

- [ ] Retry rules are defined.
- [ ] Recovery rules are defined.
- [ ] Rollback rules are defined.
- [ ] Cancellation and timeout are defined.
- [ ] Manual intervention is supported.

### Auditability

- [ ] History is append-only.
- [ ] Every transition has a reason code.
- [ ] Every transition is traceable.
- [ ] Snapshots are supported.
- [ ] State and transition JSON are schema-valid.

---

## 47. Prohibited Behaviours

The following are prohibited:

- direct Engine mutation of committed state;
- unregistered state names;
- unregistered transitions;
- silent transition without a record;
- transition commit without validation;
- bypassing policy or approval gates;
- treating waiting as success;
- treating blocked as terminal without policy;
- treating stage completion as workflow closure;
- deleting state history;
- reusing stale state without validation;
- retrying non-idempotent side effects without protection;
- rollback without compensation analysis;
- leaving active work after terminal cancellation;
- leaving a terminal state through an ungoverned transition;
- using log text as the authoritative State Record.

---

## 48. Summary

The State Model is the authoritative lifecycle control system for Scout Runtime.

It defines:

- where an execution is;
- how it arrived there;
- what may happen next;
- who may propose and commit change;
- what conditions must pass;
- how failure, waiting, retry, recovery, rollback, cancellation, and timeout are handled;
- when work is complete;
- how every transition is recorded and audited.

Engines perform work.

Engine Result proposes change.

The Runtime Orchestrator validates and commits state.

No Runtime lifecycle change exists without an explicit, governed, versioned, and observable State Transition.


---

## Appendix A — JSON Syntax Validation Certificate

All fenced `json` code blocks in this document were parsed successfully with Python's standard `json` parser.

```json
{
  "document": "state_model.md",
  "document_version": "1.0.0",
  "validated_at": "2026-07-28T10:42:28.328498+00:00",
  "validator": "Python json.loads",
  "json_code_blocks_found": 7,
  "json_code_blocks_valid": 7,
  "json_syntax_status": "passed"
}
```
