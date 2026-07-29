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
- `administrative` — post-closure archival or approved migration administration that does not alter terminal semantics.

---

## 7. Canonical Runtime Lifecycle — Non-Normative Illustration

The following diagram illustrates the normal successful path. It is not the transition registry and does not authorize a transition. Conditional, exceptional, governance, recovery, and administrative paths are defined exclusively by the normative registry in Section 16.

```text
task_received
↓
task_validating
↓
task_validated
↓
context_building
↓
context_validating
↓
context_ready
↓
retrieval_pending
↓
retrieval_running
↓
knowledge_ready
↓
business_judgement_pending
↓
business_judgement_running
↓
business_judgement_ready
↓
planning_pending
↓
planning_running
↓
execution_blueprint_ready
↓
execution_pending
↓
execution_running
↓
execution_result_ready
↓
quality_pending
↓
quality_running
↓
quality_passed
↓
learning_pending
↓
learning_running
↓
learning_evaluated
↓
memory_pending
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

### 8.10 Completion State

- `workflow_completed`

`workflow_completed` is non-terminal. It records successful functional completion and permits only the governed transition to `workflow_closed` after closure requirements pass.

### 8.11 Terminal States

- `workflow_closed`
- `cancelled`
- `timed_out`
- `failed_terminal`
- `rejected_terminal`
- `superseded`

### 8.12 Administrative States

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

The following JSON is a conditional example of a committed transition record. It does not add an allowed transition beyond Section 16.

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

## 16. Normative Allowed Transition Registry

This section is the complete normative allowed-transition registry. It contains exactly one entry for every state in the Core State Registry. The Runtime Orchestrator MUST reject a transition when its source entry is absent, its target is neither listed in `allowed_to_states` nor authorised by that source entry's registered `dynamic_target_mechanism`, or any common, entry-specific, or dynamic-target guard fails.

The registry fields have these controlled meanings:

- `transition_mode`: `conditional_branch`, `waiting_resume`, `blocked_resolution`, `governance_branch`, `exception_routing`, `retry`, `recovery`, `rollback`, `completion`, `terminal`, `administrative_exception`, or `administrative_final`;
- `policy_gate`, `approval_gate`, and `quality_gate`: `required`, `conditional`, or `not_applicable`;
- `checkpoint_requirement`: `required`, `conditional`, or `not_required`;
- `dynamic_target_mechanism`: the sole registered mechanism, if any, through which the source may propose a validated dynamic target; absence means that no dynamic target is permitted;
- boolean capability fields state whether that response may be selected directly from the source state; they do not bypass its listed target or guard requirements;
- `required_guards` are additional to `common_required_guards`;
- `required_runtime_objects` uses canonical `object_type` values from the Runtime Object Registry. An empty array means no additional domain object is required beyond the State Transition record and any Engine Result required by the common guards.

The registry is machine-extractable JSON and normative:

```json
{
  "registry_id": "scout_runtime_allowed_transition_registry",
  "registry_version": "1.1.0",
  "normative": true,
  "entry_count": 70,
  "common_required_guards": [
    "source_state_is_current",
    "target_state_is_registered_and_allowed",
    "proposer_authority_valid",
    "required_engine_result_present_where_applicable",
    "engine_context_valid_where_applicable",
    "policy_approval_quality_security_privacy_guards_pass",
    "freshness_integrity_resource_and_concurrency_guards_pass",
    "transition_reason_and_observability_recorded"
  ],
  "entries": [
    {
      "from_state": "task_received",
      "allowed_to_states": [
        "task_validating",
        "cancelled"
      ],
      "state_category": "initialisation",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "task_identity_valid"
      ],
      "required_runtime_objects": [
        "task"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "task_validating",
      "allowed_to_states": [
        "task_validated",
        "exception_detected",
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "initialisation",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "task_validation_outcome_recorded"
      ],
      "required_runtime_objects": [
        "task"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "task_validated",
      "allowed_to_states": [
        "context_building",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled"
      ],
      "state_category": "initialisation",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "task_validation_passed"
      ],
      "required_runtime_objects": [
        "task"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "context_building",
      "allowed_to_states": [
        "context_validating",
        "exception_detected",
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "initialisation",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "context_build_inputs_available"
      ],
      "required_runtime_objects": [
        "task"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "context_validating",
      "allowed_to_states": [
        "context_ready",
        "exception_detected",
        "recovery_pending",
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "initialisation",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "context_validation_outcome_recorded"
      ],
      "required_runtime_objects": [
        "task"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "context_ready",
      "allowed_to_states": [
        "retrieval_pending",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "context_ready_and_immutable"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "retrieval_pending",
      "allowed_to_states": [
        "retrieval_running",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled",
        "timed_out"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "retrieval_preconditions_passed"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "retrieval_running",
      "allowed_to_states": [
        "retrieval_waiting",
        "knowledge_ready",
        "knowledge_incomplete",
        "retrieval_failed",
        "exception_detected",
        "cancelled",
        "timed_out"
      ],
      "state_category": "active",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "retrieval_outcome_recorded"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "retrieval_waiting",
      "allowed_to_states": [
        "retrieval_running",
        "exception_detected",
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "waiting",
      "is_terminal": false,
      "transition_mode": "waiting_resume",
      "required_guards": [
        "wake_condition_or_escalation_recorded"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Resume only to retrieval_running after the recorded wake condition passes; otherwise escalate, cancel, or time out."
    },
    {
      "from_state": "knowledge_ready",
      "allowed_to_states": [
        "business_judgement_pending",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "knowledge_package_valid"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "knowledge_package"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "knowledge_incomplete",
      "allowed_to_states": [
        "retrieval_pending",
        "approval_pending",
        "exception_detected",
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "knowledge_gap_and_response_recorded"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "knowledge_package"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "retrieval_failed",
      "allowed_to_states": [
        "retry_pending",
        "recovery_pending",
        "rollback_pending",
        "manual_intervention_required",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "exception",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "retrieval_failure_classified"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": true,
      "recovery_allowed": true,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "business_judgement_pending",
      "allowed_to_states": [
        "business_judgement_running",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled",
        "timed_out"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "judgement_preconditions_passed"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "knowledge_package"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "business_judgement_running",
      "allowed_to_states": [
        "business_judgement_ready",
        "business_judgement_blocked",
        "business_judgement_failed",
        "policy_evaluation_pending",
        "approval_pending",
        "exception_detected",
        "cancelled",
        "timed_out"
      ],
      "state_category": "active",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "judgement_outcome_recorded"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "knowledge_package"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "business_judgement_ready",
      "allowed_to_states": [
        "planning_pending",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "business_judgement_valid"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "knowledge_package",
        "business_judgement"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "business_judgement_blocked",
      "allowed_to_states": [
        "business_judgement_pending",
        "policy_evaluation_pending",
        "approval_pending",
        "recovery_pending",
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "blocked",
      "is_terminal": false,
      "transition_mode": "blocked_resolution",
      "required_guards": [
        "blocking_condition_resolution_recorded"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "knowledge_package"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Remediation returns through business_judgement_pending; governance, recovery, manual intervention, cancellation, and timeout are explicit exits."
    },
    {
      "from_state": "business_judgement_failed",
      "allowed_to_states": [
        "retry_pending",
        "recovery_pending",
        "rollback_pending",
        "manual_intervention_required",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "exception",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "judgement_failure_classified"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "knowledge_package"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": true,
      "recovery_allowed": true,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "planning_pending",
      "allowed_to_states": [
        "planning_running",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled",
        "timed_out"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "planning_preconditions_passed"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "business_judgement"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "planning_running",
      "allowed_to_states": [
        "execution_blueprint_ready",
        "planning_blocked",
        "planning_failed",
        "policy_evaluation_pending",
        "approval_pending",
        "exception_detected",
        "cancelled",
        "timed_out"
      ],
      "state_category": "active",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "planning_outcome_recorded"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "business_judgement"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "execution_blueprint_ready",
      "allowed_to_states": [
        "execution_pending",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "execution_blueprint_valid"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "business_judgement",
        "execution_blueprint"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "planning_blocked",
      "allowed_to_states": [
        "planning_pending",
        "policy_evaluation_pending",
        "approval_pending",
        "recovery_pending",
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "blocked",
      "is_terminal": false,
      "transition_mode": "blocked_resolution",
      "required_guards": [
        "blocking_condition_resolution_recorded"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "business_judgement"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Remediation returns through planning_pending; governance, recovery, manual intervention, cancellation, and timeout are explicit exits."
    },
    {
      "from_state": "planning_failed",
      "allowed_to_states": [
        "retry_pending",
        "recovery_pending",
        "rollback_pending",
        "manual_intervention_required",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "exception",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "planning_failure_classified"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "business_judgement"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": true,
      "recovery_allowed": true,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "execution_pending",
      "allowed_to_states": [
        "execution_running",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled",
        "timed_out"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "execution_preconditions_passed"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "execution_blueprint"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "execution_running",
      "allowed_to_states": [
        "execution_waiting",
        "execution_paused",
        "execution_result_ready",
        "execution_failed",
        "policy_evaluation_pending",
        "approval_pending",
        "exception_detected",
        "cancelled",
        "timed_out"
      ],
      "state_category": "active",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "execution_outcome_and_side_effects_recorded"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "execution_blueprint"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "execution_waiting",
      "allowed_to_states": [
        "execution_running",
        "execution_paused",
        "exception_detected",
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "waiting",
      "is_terminal": false,
      "transition_mode": "waiting_resume",
      "required_guards": [
        "wake_condition_or_escalation_recorded"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "execution_blueprint"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Resume only to execution_running after dependency and Context revalidation; pause, escalate, cancel, or time out remain explicit."
    },
    {
      "from_state": "execution_paused",
      "allowed_to_states": [
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "waiting",
      "is_terminal": false,
      "transition_mode": "waiting_resume",
      "required_guards": [
        "checkpoint_and_resume_conditions_valid"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "execution_blueprint"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Resume requires a valid checkpoint and rebuilt or revalidated Engine Context; no in-place implicit resume is allowed.",
      "dynamic_target_mechanism": "checkpoint_resume_target"
    },
    {
      "from_state": "execution_result_ready",
      "allowed_to_states": [
        "quality_pending",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "execution_result_valid"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "execution_blueprint",
        "execution_result"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "execution_failed",
      "allowed_to_states": [
        "retry_pending",
        "recovery_pending",
        "rollback_pending",
        "manual_intervention_required",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "exception",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "execution_failure_and_side_effects_classified"
      ],
      "required_runtime_objects": [
        "task",
        "engine_context",
        "execution_blueprint"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": true,
      "recovery_allowed": true,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "quality_pending",
      "allowed_to_states": [
        "quality_running",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled",
        "timed_out"
      ],
      "state_category": "quality",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "quality_inputs_available"
      ],
      "required_runtime_objects": [
        "execution_result"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "required",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "quality_running",
      "allowed_to_states": [
        "quality_passed",
        "quality_failed",
        "quality_revision_required",
        "quality_waiver_pending",
        "exception_detected",
        "cancelled",
        "timed_out"
      ],
      "state_category": "quality",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "quality_outcome_recorded"
      ],
      "required_runtime_objects": [
        "execution_result"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "required",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "quality_passed",
      "allowed_to_states": [
        "learning_pending",
        "workflow_completed"
      ],
      "state_category": "quality",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "quality_report_passed"
      ],
      "required_runtime_objects": [
        "execution_result",
        "quality_report"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "required",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "quality_failed",
      "allowed_to_states": [
        "quality_revision_required",
        "quality_waiver_pending",
        "recovery_pending",
        "rollback_pending",
        "manual_intervention_required",
        "rejected_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "quality",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "quality_failure_disposition_recorded"
      ],
      "required_runtime_objects": [
        "execution_result"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "required",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "quality_revision_required",
      "allowed_to_states": [
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "quality",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "revision_target_and_scope_recorded"
      ],
      "required_runtime_objects": [
        "execution_result"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "required",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Operational correction requires the registered revision_target authorised by a Quality Decision; static exits are limited to intervention, cancellation, or timeout.",
      "dynamic_target_mechanism": "revision_target"
    },
    {
      "from_state": "quality_waiver_pending",
      "allowed_to_states": [
        "quality_waived",
        "quality_failed",
        "approval_pending",
        "manual_intervention_required",
        "cancelled",
        "timed_out"
      ],
      "state_category": "quality",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "waiver_authority_and_scope_recorded"
      ],
      "required_runtime_objects": [
        "execution_result"
      ],
      "policy_gate": "conditional",
      "approval_gate": "required",
      "quality_gate": "required",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "quality_waived",
      "allowed_to_states": [
        "learning_pending",
        "workflow_completed"
      ],
      "state_category": "quality",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "approved_waiver_valid"
      ],
      "required_runtime_objects": [
        "execution_result",
        "quality_report"
      ],
      "policy_gate": "conditional",
      "approval_gate": "required",
      "quality_gate": "required",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "learning_pending",
      "allowed_to_states": [
        "learning_running",
        "policy_evaluation_pending",
        "approval_pending",
        "cancelled",
        "timed_out"
      ],
      "state_category": "learning",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "learning_eligibility_recorded"
      ],
      "required_runtime_objects": [
        "execution_result"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "learning_running",
      "allowed_to_states": [
        "learning_evaluated",
        "learning_rejected",
        "exception_detected",
        "cancelled",
        "timed_out"
      ],
      "state_category": "learning",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "learning_evaluation_outcome_recorded"
      ],
      "required_runtime_objects": [
        "execution_result"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "learning_evaluated",
      "allowed_to_states": [
        "memory_pending",
        "workflow_completed"
      ],
      "state_category": "learning",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "learning_candidate_valid"
      ],
      "required_runtime_objects": [
        "execution_result",
        "learning_candidate"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "learning_rejected",
      "allowed_to_states": [
        "memory_deferred",
        "workflow_completed",
        "cancelled"
      ],
      "state_category": "learning",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "learning_rejection_reason_recorded"
      ],
      "required_runtime_objects": [
        "execution_result"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "memory_pending",
      "allowed_to_states": [
        "memory_running",
        "policy_evaluation_pending",
        "approval_pending",
        "memory_deferred",
        "cancelled",
        "timed_out"
      ],
      "state_category": "memory",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "memory_eligibility_recorded"
      ],
      "required_runtime_objects": [
        "learning_candidate"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "memory_running",
      "allowed_to_states": [
        "memory_updated",
        "memory_rejected",
        "memory_deferred",
        "exception_detected",
        "cancelled",
        "timed_out"
      ],
      "state_category": "memory",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "memory_decision_recorded"
      ],
      "required_runtime_objects": [
        "learning_candidate"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "memory_updated",
      "allowed_to_states": [
        "workflow_completed"
      ],
      "state_category": "memory",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "memory_record_valid"
      ],
      "required_runtime_objects": [
        "learning_candidate",
        "memory_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "memory_rejected",
      "allowed_to_states": [
        "workflow_completed",
        "cancelled"
      ],
      "state_category": "memory",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "memory_rejection_reason_recorded"
      ],
      "required_runtime_objects": [
        "learning_candidate"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "memory_deferred",
      "allowed_to_states": [
        "workflow_completed"
      ],
      "state_category": "memory",
      "is_terminal": false,
      "transition_mode": "conditional_branch",
      "required_guards": [
        "memory_deferral_reason_recorded"
      ],
      "required_runtime_objects": [
        "learning_candidate"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "approval_pending",
      "allowed_to_states": [
        "approval_granted",
        "approval_denied",
        "approval_expired",
        "cancelled",
        "timed_out"
      ],
      "state_category": "approval",
      "is_terminal": false,
      "transition_mode": "governance_branch",
      "required_guards": [
        "approval_scope_authority_and_expiry_defined"
      ],
      "required_runtime_objects": [
        "decision"
      ],
      "policy_gate": "conditional",
      "approval_gate": "required",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "approval_granted",
      "allowed_to_states": [
        "cancelled"
      ],
      "state_category": "approval",
      "is_terminal": false,
      "transition_mode": "governance_branch",
      "required_guards": [
        "approval_valid_and_resume_target_authorised"
      ],
      "required_runtime_objects": [
        "decision"
      ],
      "policy_gate": "conditional",
      "approval_gate": "required",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "The target must equal the preserved approved resume target; this entry does not authorise arbitrary stage skipping.",
      "dynamic_target_mechanism": "approval_resume_target"
    },
    {
      "from_state": "approval_denied",
      "allowed_to_states": [
        "rejected_terminal",
        "cancelled"
      ],
      "state_category": "approval",
      "is_terminal": false,
      "transition_mode": "governance_branch",
      "required_guards": [
        "denial_scope_and_terminal_decision_recorded"
      ],
      "required_runtime_objects": [
        "decision"
      ],
      "policy_gate": "conditional",
      "approval_gate": "required",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "approval_expired",
      "allowed_to_states": [
        "approval_pending",
        "cancelled",
        "timed_out"
      ],
      "state_category": "approval",
      "is_terminal": false,
      "transition_mode": "governance_branch",
      "required_guards": [
        "expiry_confirmed_and_reapproval_or_exit_selected"
      ],
      "required_runtime_objects": [
        "decision"
      ],
      "policy_gate": "conditional",
      "approval_gate": "required",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "An expired approval may be resubmitted for approval or exit through cancellation or timeout; it does not open a generic intervention return path."
    },
    {
      "from_state": "policy_evaluation_pending",
      "allowed_to_states": [
        "policy_evaluation_running",
        "approval_pending",
        "cancelled",
        "timed_out"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "governance_branch",
      "required_guards": [
        "applicable_policy_set_identified"
      ],
      "required_runtime_objects": [
        "decision"
      ],
      "policy_gate": "required",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "policy_evaluation_running",
      "allowed_to_states": [
        "policy_passed",
        "policy_blocked",
        "cancelled",
        "timed_out"
      ],
      "state_category": "active",
      "is_terminal": false,
      "transition_mode": "governance_branch",
      "required_guards": [
        "policy_outcome_recorded"
      ],
      "required_runtime_objects": [
        "decision"
      ],
      "policy_gate": "required",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "policy_passed",
      "allowed_to_states": [
        "cancelled"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "governance_branch",
      "required_guards": [
        "policy_pass_valid_and_resume_target_authorised"
      ],
      "required_runtime_objects": [
        "decision"
      ],
      "policy_gate": "required",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "The target must equal the preserved policy resume target; all other applicable gates remain mandatory.",
      "dynamic_target_mechanism": "policy_resume_target"
    },
    {
      "from_state": "policy_blocked",
      "allowed_to_states": [
        "policy_evaluation_pending",
        "approval_pending",
        "recovery_pending",
        "rejected_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "blocked",
      "is_terminal": false,
      "transition_mode": "blocked_resolution",
      "required_guards": [
        "blocking_policy_and_permitted_resolution_recorded"
      ],
      "required_runtime_objects": [
        "decision"
      ],
      "policy_gate": "required",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Re-evaluation, an explicitly policy-permitted approval path, recovery, rejection, cancellation, or timeout must be selected by Decision."
    },
    {
      "from_state": "exception_detected",
      "allowed_to_states": [
        "exception_classifying",
        "cancelled",
        "timed_out"
      ],
      "state_category": "exception",
      "is_terminal": false,
      "transition_mode": "exception_routing",
      "required_guards": [
        "exception_record_requested_or_present"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "exception_classifying",
      "allowed_to_states": [
        "retry_pending",
        "recovery_pending",
        "rollback_pending",
        "policy_evaluation_pending",
        "approval_pending",
        "manual_intervention_required",
        "failed_terminal",
        "rejected_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "exception",
      "is_terminal": false,
      "transition_mode": "exception_routing",
      "required_guards": [
        "exception_classification_and_response_decision_recorded"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": true,
      "recovery_allowed": true,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "retry_pending",
      "allowed_to_states": [
        "retrying",
        "recovery_pending",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "recovery",
      "is_terminal": false,
      "transition_mode": "retry",
      "required_guards": [
        "retry_eligibility_budget_and_target_valid"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "conditional",
      "retry_allowed": true,
      "recovery_allowed": true,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "retrying",
      "allowed_to_states": [
        "recovery_pending",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "recovery",
      "is_terminal": false,
      "transition_mode": "retry",
      "required_guards": [
        "new_attempt_and_retry_target_authorised"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "conditional",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "The registered retry_target must identify the recorded failed operation, a Retry Decision must authorise it, and the retry must use a new attempt identity.",
      "dynamic_target_mechanism": "retry_target"
    },
    {
      "from_state": "recovery_pending",
      "allowed_to_states": [
        "recovering",
        "rollback_pending",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "recovery",
      "is_terminal": false,
      "transition_mode": "recovery",
      "required_guards": [
        "recovery_strategy_and_target_valid"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "conditional",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "recovering",
      "allowed_to_states": [
        "rollback_pending",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "recovery",
      "is_terminal": false,
      "transition_mode": "recovery",
      "required_guards": [
        "recovery_result_and_recovery_target_valid"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "conditional",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Successful recovery uses the registered recovery_target authorised by a Recovery Decision; unsuccessful recovery exits through rollback, failure, cancellation, or timeout.",
      "dynamic_target_mechanism": "recovery_target"
    },
    {
      "from_state": "rollback_pending",
      "allowed_to_states": [
        "rolling_back",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "recovery",
      "is_terminal": false,
      "transition_mode": "rollback",
      "required_guards": [
        "checkpoint_compensation_and_authority_valid"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "rolling_back",
      "allowed_to_states": [
        "rollback_completed",
        "failed_terminal",
        "cancelled",
        "timed_out"
      ],
      "state_category": "recovery",
      "is_terminal": false,
      "transition_mode": "rollback",
      "required_guards": [
        "rollback_integrity_outcome_recorded"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "Only the enumerated targets are permitted; the Runtime Orchestrator must validate all common and entry-specific guards before commit."
    },
    {
      "from_state": "rollback_completed",
      "allowed_to_states": [
        "cancelled"
      ],
      "state_category": "recovery",
      "is_terminal": false,
      "transition_mode": "rollback",
      "required_guards": [
        "post_rollback_target_and_integrity_valid"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "required",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": false,
      "cancellation_allowed": true,
      "timeout_allowed": false,
      "notes": "The registered rollback_target must be authorised by a Rollback Decision and match the Runtime State recorded by the referenced valid checkpoint.",
      "dynamic_target_mechanism": "rollback_target"
    },
    {
      "from_state": "manual_intervention_required",
      "allowed_to_states": [
        "retry_pending",
        "recovery_pending",
        "rollback_pending",
        "approval_pending",
        "policy_evaluation_pending",
        "cancelled",
        "failed_terminal",
        "rejected_terminal",
        "superseded"
      ],
      "state_category": "recovery",
      "is_terminal": false,
      "transition_mode": "recovery",
      "required_guards": [
        "authorised_human_decision_and_target_recorded"
      ],
      "required_runtime_objects": [
        "exception_record"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "conditional",
      "retry_allowed": false,
      "recovery_allowed": true,
      "rollback_allowed": true,
      "cancellation_allowed": true,
      "timeout_allowed": true,
      "notes": "An authorised human Decision may route only to a registered governance, retry, recovery, rollback, cancellation, failure, rejection, or supersession state. Operational restart requires the selected governed mechanism."
    },
    {
      "from_state": "workflow_completed",
      "allowed_to_states": [
        "workflow_closed"
      ],
      "state_category": "readiness",
      "is_terminal": false,
      "transition_mode": "completion",
      "required_guards": [
        "closure_requirements_passed"
      ],
      "required_runtime_objects": [
        "execution_result",
        "quality_report"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "required",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Successful functional completion is non-terminal and permits only governed closure."
    },
    {
      "from_state": "workflow_closed",
      "allowed_to_states": [
        "archived"
      ],
      "state_category": "terminal",
      "is_terminal": true,
      "transition_mode": "administrative_exception",
      "required_guards": [
        "retention_and_archival_authority_valid"
      ],
      "required_runtime_objects": [
        "execution_result",
        "quality_report"
      ],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "required",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Normal successful terminal state; the sole outgoing edge is the separately governed administrative transition to archived."
    },
    {
      "from_state": "cancelled",
      "allowed_to_states": [],
      "state_category": "terminal",
      "is_terminal": true,
      "transition_mode": "terminal",
      "required_guards": [
        "no_outgoing_transition"
      ],
      "required_runtime_objects": [],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Terminal; no ordinary or administrative transition is permitted."
    },
    {
      "from_state": "timed_out",
      "allowed_to_states": [],
      "state_category": "terminal",
      "is_terminal": true,
      "transition_mode": "terminal",
      "required_guards": [
        "no_outgoing_transition"
      ],
      "required_runtime_objects": [],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Terminal only when no recovery path remains; no outgoing transition is permitted."
    },
    {
      "from_state": "failed_terminal",
      "allowed_to_states": [],
      "state_category": "terminal",
      "is_terminal": true,
      "transition_mode": "terminal",
      "required_guards": [
        "no_outgoing_transition"
      ],
      "required_runtime_objects": [],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Terminal failure; no outgoing transition is permitted."
    },
    {
      "from_state": "rejected_terminal",
      "allowed_to_states": [],
      "state_category": "terminal",
      "is_terminal": true,
      "transition_mode": "terminal",
      "required_guards": [
        "no_outgoing_transition"
      ],
      "required_runtime_objects": [],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Terminal rejection; no outgoing transition is permitted."
    },
    {
      "from_state": "superseded",
      "allowed_to_states": [],
      "state_category": "terminal",
      "is_terminal": true,
      "transition_mode": "terminal",
      "required_guards": [
        "no_outgoing_transition"
      ],
      "required_runtime_objects": [],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Terminal supersession; replacement work uses a different execution identity."
    },
    {
      "from_state": "archived",
      "allowed_to_states": [],
      "state_category": "administrative",
      "is_terminal": false,
      "transition_mode": "administrative_final",
      "required_guards": [
        "no_outgoing_transition"
      ],
      "required_runtime_objects": [],
      "policy_gate": "conditional",
      "approval_gate": "conditional",
      "quality_gate": "conditional",
      "checkpoint_requirement": "not_required",
      "retry_allowed": false,
      "recovery_allowed": false,
      "rollback_allowed": false,
      "cancellation_allowed": false,
      "timeout_allowed": false,
      "notes": "Final administrative state entered only from workflow_closed; it is an intentional administrative dead end, not a normal terminal lifecycle state."
    }
  ],
  "dynamic_target_model": {
    "normative": true,
    "static_graph_semantics": "allowed_to_states contains only literal static transition targets. Governed dynamic targets are validated separately and MUST NOT be expanded into static fan-out edges.",
    "universal_validation_rules": [
      "The dynamic target MUST name a state registered in the Core State Registry.",
      "The dynamic target MUST satisfy the permitted_target_rule and, where present, belong to permitted_target_states.",
      "The dynamic target MUST have been recorded before entry into the detour or be derived from the approved Decision required by the mechanism.",
      "The Runtime Orchestrator MUST validate the target, authorisation, origin context, and transition data before committing Runtime State.",
      "The selected mechanism, target state, origin state, authorising Decision identifier, and applicable checkpoint identifier MUST be recorded in the State Transition Proposal and resulting Runtime Event.",
      "A dynamic target MUST NOT be treated as an implicit transition from any source state other than the source_states registered for its mechanism."
    ],
    "mechanisms": {
      "approval_resume_target": {
        "target_field": "resume_target",
        "source_states": [
          "approval_granted"
        ],
        "detour_entry_state": "approval_pending",
        "permitted_target_rule": "registered_origin_state_with_static_edge_to_detour_entry",
        "permitted_target_states": [],
        "required_decision_type": "Approval Decision",
        "recording_requirement": "recorded_before_detour",
        "origin_context_required": true,
        "checkpoint_required": false
      },
      "policy_resume_target": {
        "target_field": "resume_target",
        "source_states": [
          "policy_passed"
        ],
        "detour_entry_state": "policy_evaluation_pending",
        "permitted_target_rule": "registered_origin_state_with_static_edge_to_detour_entry",
        "permitted_target_states": [],
        "required_decision_type": "Policy Decision",
        "recording_requirement": "recorded_before_detour",
        "origin_context_required": true,
        "checkpoint_required": false
      },
      "checkpoint_resume_target": {
        "target_field": "resume_target",
        "source_states": [
          "execution_paused"
        ],
        "permitted_target_rule": "explicit_set",
        "permitted_target_states": [
          "context_building",
          "execution_pending"
        ],
        "required_decision_type": "Resume Decision",
        "recording_requirement": "derived_from_approved_decision",
        "origin_context_required": true,
        "checkpoint_required": true
      },
      "retry_target": {
        "target_field": "retry_target",
        "source_states": [
          "retrying"
        ],
        "permitted_target_rule": "explicit_set",
        "permitted_target_states": [
          "task_validating",
          "context_building",
          "context_validating",
          "retrieval_running",
          "business_judgement_running",
          "planning_running",
          "execution_running",
          "quality_running",
          "learning_running",
          "memory_running",
          "policy_evaluation_running"
        ],
        "required_decision_type": "Retry Decision",
        "recording_requirement": "derived_from_approved_decision",
        "origin_context_required": true,
        "checkpoint_required": false
      },
      "recovery_target": {
        "target_field": "recovery_target",
        "source_states": [
          "recovering"
        ],
        "permitted_target_rule": "explicit_set",
        "permitted_target_states": [
          "context_building",
          "retrieval_pending",
          "business_judgement_pending",
          "planning_pending",
          "execution_pending",
          "quality_pending",
          "learning_pending",
          "memory_pending",
          "policy_evaluation_pending",
          "approval_pending"
        ],
        "required_decision_type": "Recovery Decision",
        "recording_requirement": "derived_from_approved_decision",
        "origin_context_required": true,
        "checkpoint_required": false
      },
      "rollback_target": {
        "target_field": "rollback_target",
        "source_states": [
          "rollback_completed"
        ],
        "permitted_target_rule": "explicit_set_and_checkpoint_state_match",
        "permitted_target_states": [
          "context_building",
          "retrieval_pending",
          "business_judgement_pending",
          "planning_pending",
          "execution_pending",
          "quality_pending",
          "learning_pending",
          "memory_pending",
          "policy_evaluation_pending",
          "approval_pending"
        ],
        "required_decision_type": "Rollback Decision",
        "recording_requirement": "derived_from_approved_decision",
        "origin_context_required": true,
        "checkpoint_required": true
      },
      "revision_target": {
        "target_field": "revision_target",
        "source_states": [
          "quality_revision_required"
        ],
        "permitted_target_rule": "explicit_set",
        "permitted_target_states": [
          "retrieval_pending",
          "business_judgement_pending",
          "planning_pending",
          "execution_pending"
        ],
        "required_decision_type": "Quality Decision",
        "recording_requirement": "derived_from_approved_decision",
        "origin_context_required": true,
        "checkpoint_required": false
      }
    }
  }
}
```

Unregistered transitions are prohibited unless an approved migration or emergency rule applies. Such a rule MUST be recorded, approved, time-bounded, audited, and validated against registered source and target states.

The `workflow_closed → archived` entry is an administrative exception, not an ordinary lifecycle transition. `workflow_completed` permits only `workflow_closed`; `archived` cannot be entered directly from `workflow_completed` or used to bypass closure.

### 16.1 Governed Dynamic-Target Rules

The `dynamic_target_model` is normative. A dynamic transition is authorised only when its source state declares the named `dynamic_target_mechanism`; the mechanism's target field, target rule, Decision type, origin requirements, and checkpoint requirements all validate. Dynamic targets MUST NOT be expanded into literal `allowed_to_states` fan-outs.

- Approval and policy detours use `resume_target`. The target MUST be the registered origin state recorded before the detour, that origin MUST have a static transition to the applicable detour entry state, and the approved Decision MUST preserve the origin context.
- An execution pause uses `resume_target` only through `checkpoint_resume_target`. The approved Resume Decision and valid checkpoint MUST authorise either `context_building` or `execution_pending`.
- `retry_target` MUST be authorised by a bounded Retry Decision, identify the recorded failed operation, and use a new attempt identity.
- `recovery_target` MUST be authorised by a Recovery Decision and belong to the registered recovery target set.
- `rollback_target` MUST be authorised by a Rollback Decision, belong to the registered rollback target set, and equal the Runtime State recorded by the referenced valid checkpoint.
- `revision_target` MUST be authorised by a Quality Decision and identify a registered correction stage in the permitted revision target set.
- Every target MUST be registered, belong to the mechanism's permitted set or satisfy its registered origin rule, be recorded before the detour or derived from the required approved Decision, and be validated by the Runtime Orchestrator.
- `quality_passed → workflow_completed` and `quality_waived → workflow_completed` are permitted only when learning and memory processing are explicitly not required by controlling policy.
- `learning_evaluated → workflow_completed` is permitted only when memory evaluation is explicitly not required; otherwise it MUST enter `memory_pending`.

`manual_intervention_required` is a routing state, not a generic resume hub. It may propose only `retry_pending`, `recovery_pending`, `rollback_pending`, `approval_pending`, `policy_evaluation_pending`, `cancelled`, `failed_terminal`, `rejected_terminal`, or `superseded`. Any return to an operational lifecycle stage MUST pass through the selected governed mechanism and its typed Decision or checkpoint validation.

### 16.2 Cycle and Exit Policy

The ordinary lifecycle is directional. Static cycles are permitted only as bounded local components for retrieval waiting, execution waiting, approval expiry, policy re-evaluation, blocked-stage remediation, or quality waiver review. Retry, recovery, rollback, revision, checkpoint resume, and governance resume paths use the registered dynamic-target mechanisms rather than broad static return edges.

Every local loop MUST be bounded by its applicable maximum wait, approval expiry, retry budget, recovery limit, rollback policy, resource budget, or authorised Decision. Every loop has an explicit exit to forward progress or a registered cancellation, timeout, failure, rejection, or supersession path. No administrative cycle exists. A transition implementation MUST reject an unbounded loop or a loop without a recorded exit policy.

---

## 17. Composite States

A composite state contains substates while preserving one top-level lifecycle position.

Non-normative composite illustration:

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

Non-normative parallel-execution illustration:

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

Conditional approval illustration:

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

Conditional policy illustration:

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

Conditional quality illustration:

```text
quality_pending
↓
quality_running
├── quality_passed
├── quality_revision_required
└── quality_failed
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

Conditional retry illustration:

```text
exception_detected
↓
exception_classifying
↓
retry_pending
↓
retrying
├── registered retry target selected by Retry Decision
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

Conditional rollback illustration:

```text
rollback_pending
↓
rolling_back
↓
rollback_completed
└── registered rollback target selected by Rollback Decision and valid checkpoint
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

Conditional exception-routing illustration:

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

Conditional learning illustration:

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

Conditional memory illustration:

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
- `superseded`.

`workflow_completed` is non-terminal. It indicates successful functional completion while mandatory administrative closure actions remain.

`workflow_closed` is the normal successful terminal state.

`archived` is an administrative state entered only after closure. It is not a substitute for `workflow_closed`.

No transition may leave a terminal state except the explicitly governed administrative transition `workflow_closed → archived`.

---

## 34. Completion Semantics

Non-normative completion-level illustration; the `workflow_closed → archived` edge remains the administrative exception registered in Section 16:

```text
stage_completed
workflow_completed
workflow_closed
archived
```

`stage_completed` is a conceptual completion level, not a registered Runtime State.

A stage completion MUST NOT be treated as workflow completion.

Workflow completion requires all mandatory stages, quality gates, approvals, and required outputs.

Workflow closure additionally requires final records, observability completion, retention assignment, and unresolved issue review.

Therefore, `workflow_completed` is non-terminal and may transition only to `workflow_closed`. Archival MUST occur only after `workflow_closed` through the governed administrative transition.

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

The following JSON is a non-normative history illustration.

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
      "state": "task_validating",
      "entered_at": "2026-07-28T09:00:01Z",
      "exited_at": "2026-07-28T09:00:02Z",
      "transition_id": "tr_002"
    },
    {
      "state_version": 3,
      "state": "task_validated",
      "entered_at": "2026-07-28T09:00:02Z",
      "exited_at": "2026-07-28T09:00:03Z",
      "transition_id": "tr_003"
    },
    {
      "state_version": 4,
      "state": "context_building",
      "entered_at": "2026-07-28T09:00:03Z",
      "exited_at": "2026-07-28T09:00:05Z",
      "transition_id": "tr_004"
    },
    {
      "state_version": 5,
      "state": "context_validating",
      "entered_at": "2026-07-28T09:00:05Z",
      "exited_at": "2026-07-28T09:00:07Z",
      "transition_id": "tr_005"
    },
    {
      "state_version": 6,
      "state": "context_ready",
      "entered_at": "2026-07-28T09:00:07Z",
      "exited_at": null,
      "transition_id": "tr_006"
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

The following JSON is a non-normative snapshot illustration.

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

The following JSON is a conditional example whose source and target are authorized by Section 16.

```json
{
  "transition_id": "tr_exec_001_009",
  "schema_name": "state_transition",
  "schema_version": "1.0.0",
  "execution_id": "exec_001",
  "workflow_id": "wf_content_001",
  "expected_state_version": 8,
  "from_state": "execution_blueprint_ready",
  "to_state": "execution_pending",
  "proposed_by": {
    "actor_type": "runtime",
    "actor_id": "runtime_orchestrator"
  },
  "proposed_at": "2026-07-28T10:15:00Z",
  "reason_code": "execution_queued",
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

The following JSON is a non-normative validation-result illustration.

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

- [ ] The Core State Registry contains exactly 70 unique state names unless an approved State Model version changes that count.
- [ ] Every state has one semantic definition.
- [ ] Every registered state has exactly one normative allowed-transition entry.
- [ ] Every `from_state` is registered and no duplicate source entry exists.
- [ ] Every `allowed_to_states` target is registered.
- [ ] Every transition is registered and machine-extractable.
- [ ] Every dynamic-target mechanism identifies its source states, target field, permitted target rule or set, required Decision type, origin-context requirement, and checkpoint requirement.
- [ ] Every dynamic target is registered, authorised, recorded, and validated by the Runtime Orchestrator without being expanded into static graph fan-out.
- [ ] Every terminal state is identified.
- [ ] Every terminal state has an empty target list except the governed `workflow_closed → archived` administrative exception.
- [ ] Every intended lifecycle state is reachable from `task_received`.
- [ ] No non-terminal state is unintentionally dead-ended.
- [ ] Every cycle is classified, bounded, and has an explicit exit policy.
- [ ] No broad strongly connected component is created by a generic resume, retry, recovery, rollback, revision, or manual-intervention hub.
- [ ] Approval, policy, quality, waiting, blocked, exception, retry, recovery, rollback, cancellation, and timeout branches are explicit.
- [ ] `workflow_completed` is treated as non-terminal and transitions only to `workflow_closed`.
- [ ] `workflow_closed` remains the normal successful terminal state.
- [ ] Archival cannot bypass workflow closure.

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
- treating `workflow_completed` as terminal;
- transitioning directly from `workflow_completed` to `archived`;
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
  "validated_at": "2026-07-29T07:27:13Z",
  "validator": "Python json.loads",
  "json_code_blocks_found": 8,
  "json_code_blocks_valid": 8,
  "json_syntax_status": "passed"
}
```
