# H-06 Engine Invocation Status Architecture Design

**Document ID:** `SCOUT-RUNTIME-H06-INVOCATION-STATUS-DESIGN`
**Version:** `1.0.0`
**Status:** `Architecture Design — Implemented`
**Owner:** `Scout Runtime`
**Finding:** `H-06`
**Date:** `2026-07-28`

---

## 1. Executive Summary

Engine Invocation Status and Runtime State are separate controlled vocabularies with different owners, scopes, identifiers, and commit rules.

The recommended architecture keeps the existing machine-valid Engine Result status enum:

```text
waiting
succeeded
partial_success
failed
blocked
cancelled
timeout
```

These values are final return dispositions for one Engine invocation. They are not workflow positions and never commit Runtime State. `ready` and `running` are invocation lifecycle phases observed through events, not valid final Engine Result statuses. `success` is a rejected alias for `succeeded`.

Every final Engine Result carries exactly one status and may carry a State Transition proposal. The Runtime Orchestrator independently validates that proposal against `../engines/shared/state_model.md` and commits the Runtime State. A successful invocation may advance one stage without completing the workflow; a failed invocation may lead to retry or recovery without terminating the workflow.

This design resolves H-06 without adding a schema, Runtime Object, or Runtime State.

---

## 2. Current Architecture

The current architecture already establishes several correct boundaries:

- `../engines/shared/engine_contract.md` owns Engine behaviour and Engine Result semantics.
- `../schemas/engine_result.schema.json` requires `status` and machine-validates seven return dispositions.
- `../engines/shared/state_model.md` owns Runtime State names, categories, transitions, and terminal semantics.
- only the Runtime Orchestrator may commit global Runtime State.
- Engine Result and Execution Result are distinct Runtime Objects.
- Engine Result may carry a State Transition proposal or reference.
- Runtime Events are structured observability objects, not State Records.
- Decisions govern material selections and recovery actions but do not commit State.

The Engine Contract currently lists `ready` and `running` beside final return dispositions even though the Engine Result schema excludes them. It also calls the list “terminal or waiting” without clearly separating invocation completion from workflow resumability.

Current status-like values discovered in the Engine Contract and Engine Result schema are:

| Value | Current use | Current machine acceptance |
|---|---|---:|
| `ready` | Engine lifecycle/status prose | No |
| `running` | Engine lifecycle/status prose | No |
| `waiting` | Engine return disposition | Yes |
| `succeeded` | Engine return disposition | Yes |
| `partial_success` | Engine return disposition | Yes |
| `failed` | Engine return disposition | Yes |
| `blocked` | Engine return disposition | Yes |
| `cancelled` | Engine return disposition | Yes |
| `timeout` | Engine return disposition | Yes |
| `success` | Design candidate or informal synonym | No |

Other Runtime Object schemas use their own `status` enums. Those enums describe the lifecycle of their owning objects and MUST NOT be imported into Engine Result.

---

## 3. Problem Statement

The current documents do not make five boundaries explicit enough:

1. lifecycle phase versus final invocation disposition;
2. Engine invocation disposition versus workflow Runtime State;
3. Engine Result reporting versus State Transition proposal;
4. timeout disposition versus timeout cause, event, Decision, and terminal Runtime State;
5. invocation completion versus workflow completion.

Without a protocol, an implementation could commit `blocked` as a generic Runtime State, treat `running` as a final Engine Result, map every `failed` invocation to `failed_terminal`, or treat `succeeded` as `workflow_completed`. Each behaviour would violate the existing ownership architecture.

---

## 4. Design Principles

1. One field has one semantic domain.
2. Existing normative owners remain authoritative.
3. Final return disposition is separate from lifecycle observation.
4. Similar spelling never implies interchangeability.
5. An Engine proposes; the Runtime Orchestrator validates and commits.
6. Invocation completion does not imply workflow completion.
7. Failure, blocking, waiting, cancellation, and timeout preserve evidence and recovery context.
8. The smallest unambiguous model is preferred over parallel status fields.
9. Every machine value is schema-validatable.
10. Cross-domain mappings are conditional, never one-to-one defaults.

---

## 5. Authority and Ownership

| Concept | Sole normative owner | Machine authority | Boundary |
|---|---|---|---|
| Engine Invocation Status | `../engines/shared/engine_contract.md` | `../schemas/engine_result.schema.json` | Final disposition of one invocation. |
| Runtime State | `../engines/shared/state_model.md` | Planned or deferred State schema; State Model remains normative | Globally committed workflow lifecycle position. |
| State Transition | `../engines/shared/state_model.md` | Planned or deferred transition schema; State Model remains normative | Proposed or committed movement between registered Runtime States. |
| Engine Result | `../engines/shared/engine_contract.md` | `../schemas/engine_result.schema.json` | Universal response envelope for one Engine invocation. |
| Runtime Event | `../engines/shared/engine_contract.md` | Schema deferred | Observable fact about an invocation or transition. |
| Decision | `../engines/shared/decision_model.md` | Schema deferred | Governed selection or outcome supporting action. |
| Runtime Object identity and registration | `../engines/shared/runtime_objects.md` | `../registry/runtime_objects.json` and its registry schema | Canonical object identity, owner, and relationship registration. |

The semantic owner defines meaning. A schema validates representation. The Runtime Object Registry records ownership but does not take it over.

---

## 6. Engine Invocation Lifecycle

The lifecycle is an ordered set of observable phases, not a second Engine Result status enum:

```text
invocation created
        ↓
context received and validated
        ↓
invocation ready
        ↓
invocation running
        ↓
result finalising
        ↓
Engine Result returned with one canonical status
```

Conditional signals may occur during processing:

```text
waiting condition detected
blocking condition detected
cancellation requested
timeout detected
exception detected
```

Once an Engine Result is returned, that invocation is complete. A waiting or blocked workflow may later resume through a new `invocation_id` and revalidated Engine Context. Whether `attempt_number` increments on a non-retry resumption remains a Runtime execution-policy decision.

Candidate names such as `invocation_created`, `invocation_ready`, `invocation_running`, and `invocation_completed` are lifecycle/event concepts. They MUST NOT be added to the Engine Result `status` enum. `invocation_failed`, `invocation_cancelled`, and `invocation_timed_out` are event concepts whose corresponding final dispositions are `failed`, `cancelled`, and `timeout`.

---

## 7. Canonical Invocation Status Model

The canonical field is Engine Result `status`. It is mandatory and owned semantically by the Engine Contract and mechanically by `engine_result.schema.json`.

| Status | Classification | Meaning |
|---|---|---|
| `waiting` | Final invocation disposition; workflow resumable | The invocation returned because an external or scheduled condition must be satisfied. |
| `succeeded` | Final invocation disposition | Required local outputs and mandatory local gates passed. |
| `partial_success` | Final invocation disposition | Valid output exists, but explicitly identified non-optional scope remains incomplete. |
| `failed` | Final invocation disposition | The invocation did not produce the required valid result. |
| `blocked` | Final invocation disposition; workflow resumability governed | A policy, approval, security, privacy, evidence, dependency, or quality condition prevented execution. |
| `cancelled` | Final invocation disposition | Authorised cancellation and required cleanup or cleanup handoff have been recorded. |
| `timeout` | Final invocation disposition | The invocation exceeded its approved time boundary and returned a timeout outcome. |

Values evaluated but not adopted:

| Value | Decision |
|---|---|
| `ready` | Invalid as Engine Result status; retain as lifecycle phase/event meaning. |
| `running` | Invalid as Engine Result status; retain as lifecycle phase/event meaning. |
| `success` | Reject as an alias; migrate to `succeeded`. |
| `invocation_completed` | Event/lifecycle concept, not result status. |
| `timed_out` | Reserved as the canonical Runtime State; do not use as Engine Result status. |

No separate persistent `invocation_phase` field is recommended for Engine Result. Structured events already represent progress, while Engine Result represents the returned disposition. An implementation may maintain an internal phase, but it is non-authoritative and MUST NOT be accepted as Engine Result status or Runtime State.

---

## 8. Terminal and Resumable Semantics

All seven canonical Engine Result statuses are final for the identified `invocation_id`: after returning the envelope, that invocation does not resume in place.

Finality of an invocation is independent of resumability of its execution or workflow:

- `waiting` is final for the invocation and normally resumable through a new invocation.
- `blocked` is final for the invocation; workflow resumability depends on policy, remediation, approval, or escalation.
- `partial_success` is final for the invocation; follow-up work requires a new invocation or planned downstream stage.
- `failed` is final for the invocation but does not imply terminal Runtime failure.
- `timeout` is final for the invocation but may lead to retry, recovery, cancellation, or `timed_out` State.
- `cancelled` is final only after cleanup completes or ownership of remaining cleanup is explicitly handed off and recorded.
- `succeeded` is final for the invocation but does not imply successful completion of the workflow.

The Engine Contract phrase “terminate in one terminal or waiting status” should be replaced during implementation with “return exactly one final invocation disposition.”

---

## 9. Runtime State Boundary

Runtime State is the single globally committed workflow position governed by the State Model. Only the Runtime Orchestrator commits it.

Engine Invocation Status:

- is scoped by `invocation_id`;
- appears in Engine Result `status`;
- describes why one invocation returned;
- does not require its spelling to exist in the State Registry;
- cannot change global State.

Runtime State:

- is scoped by `execution_id` and workflow identity;
- appears in State Records and State Transitions;
- must use a registered State Model value;
- changes only through Orchestrator validation and commit.

Similarly named values may coexist only because their field paths and authorities are explicit. For example, Engine Result `status: cancelled` and Runtime State `current_state: cancelled` are separate facts. The first may support a proposal for the second, but does not commit it.

Generic invocation `blocked` MUST NOT become a generic Runtime State. A proposal must select a registered state justified by the lifecycle position, such as `business_judgement_blocked`, `planning_blocked`, or `policy_blocked`, or propose an exception/recovery path.

---

## 10. Engine Result Integration

A final Engine Result MUST:

- include mandatory `status` using the canonical seven-value enum;
- identify `engine_id`, `execution_id`, and `invocation_id`;
- contain or reference produced Runtime Objects through `primary_output` and `secondary_outputs`;
- preserve the distinction between Engine Result and Execution Result;
- include `state_transition` as a proposal or reference, or `null` when no proposal is appropriate;
- include structured `failure`, `recovery`, `blocking_issues`, and Exception Record reference data when required;
- include relevant Decisions by reference;
- emit or reference the required Runtime Events;
- pass Engine Result schema validation before it is accepted.

`succeeded` and `partial_success` require a non-empty valid primary output. `waiting`, `cancelled`, and `timeout` may use `output_kind: none`. `failed` and `blocked` require structured failure and blocking information under the existing contract. Later implementation should add explicit schema conditions for timeout, cancellation, and waiting evidence where the current schema is less strict than the prose contract.

Execution Result remains the Execution Engine's produced Runtime Object. Its own `status` describes that object's lifecycle and MUST NOT replace Engine Result `status`.

---

## 11. State Transition Proposal Protocol

```text
Engine performs bounded work
        ↓
Engine returns Engine Result with invocation status
        ↓
Engine Result optionally carries State Transition proposal
        ↓
Runtime Orchestrator validates source State, target State, authority and guards
        ↓
Runtime Orchestrator commits or rejects the State Transition
        ↓
Committed Runtime State and Runtime Event are recorded
```

Protocol rules:

1. Engine status does not commit Runtime State.
2. A proposal must name registered source and target Runtime States.
3. A proposal must comply with the State Model transition registry and gates.
4. The Orchestrator may accept, reject, defer, or replace a proposal only within its authority and with a recorded Decision.
5. `succeeded` does not automatically propose `workflow_completed` or `workflow_closed`.
6. `failed` does not automatically propose `failed_terminal`.
7. `waiting`, `blocked`, `partial_success`, `cancelled`, and `timeout` require transition handling appropriate to current State, policy, evidence, side effects, and recovery eligibility.
8. A null proposal is valid when the invocation does not justify a State change, but the reason must remain observable.

---

## 12. Decision Integration

The Decision Model remains authoritative. This protocol requires a Decision when selecting or authorising a material:

- retry, including eligibility, attempt limit, delay, and idempotency;
- recovery strategy and return State;
- rollback target and compensation action;
- escalation authority and resume target;
- cancellation response where work or side effects exist;
- timeout response, including retry, recovery, cancellation, or terminal proposal;
- approval outcome or approval-dependent resume;
- policy-blocking outcome or waiver path;
- quality revision, rejection, or waiver.

Routine successful stage advancement may use the material Decision already referenced by Engine Result. This design does not create new Decision types or redefine Decision fields.

---

## 13. Exception and Timeout Integration

Timeout has five distinct representations:

| Representation | Canonical term | Meaning |
|---|---|---|
| Engine Result status | `timeout` | Final disposition of the invocation that exceeded its limit. |
| Runtime State | `timed_out` | Terminal workflow State only when no recovery path remains and the Orchestrator commits it. |
| Failure or exception category | `timeout` | Cause classification recorded in failure or Exception Record data. |
| Runtime Event | `engine_timeout` | Observable fact that the invocation time boundary was exceeded. |
| Decision | timeout-handling Decision | Selection of retry, recovery, cancellation, escalation, or terminal transition. |

`timeout` MUST NOT be silently normalised to `failed`; doing so loses cause and policy semantics. A timeout may additionally satisfy failure conditions, but its primary invocation status remains `timeout`.

An Exception Record is required when a failure, block, timeout, cancellation issue, or partial result is material, needs recovery, affects side effects, or requires audit beyond the Engine Result. Non-material waiting does not automatically require an Exception Record.

---

## 14. Runtime Event Boundary

The minimum event boundary needed for H-06 is:

- lifecycle events identify creation/context receipt, start, and result return;
- condition events identify waiting, blocking, cancellation, timeout, and exception detection;
- a proposal event identifies that an Engine proposed a State Transition;
- a separate Orchestrator event identifies transition commit or rejection.

Existing event names such as `engine_context_received`, `engine_started`, `engine_state_transition_proposed`, `engine_completed`, `engine_blocked`, `engine_cancelled`, and `engine_timeout` should be retained where their semantics match this boundary.

Every event must carry `event_type`, `invocation_id`, `engine_id`, `execution_id`, timestamp, correlation or trace identity, and the relevant invocation status or State Transition reference. An event is evidence of an occurrence; it is not a status, State, Decision, or Exception Record.

This section does not define the complete observability contract.

---

## 15. Mapping Matrix

“Typical proposed Runtime State” means examples selected according to current lifecycle position. It is never an automatic mapping.

| Engine invocation status | Invocation terminal? | Resumable? | Engine Result allowed? | State transition proposal allowed? | Typical proposed Runtime State | Decision required? | Exception Record required? | Runtime Event required? | Notes |
|---|---:|---:|---:|---:|---|---|---|---:|---|
| `waiting` | Yes | Yes, normally | Yes | Yes | `approval_pending`, `retrieval_waiting`, `execution_waiting`, or no change | Conditional; yes for approval or governed wait routing | No, unless abnormal or material | Yes | Resume uses a new invocation and revalidated Context. |
| `succeeded` | Yes | Not applicable | Yes | Yes | Stage-specific registered readiness or active State | Yes for material output; may reuse referenced Decision | No | Yes | Never implies `workflow_completed`. |
| `partial_success` | Yes | Yes, through follow-up | Yes | Yes | `quality_revision_required`, a waiting State, recovery path, or no change | Yes | Conditional; required when material | Yes | Must identify completed and incomplete scope. |
| `failed` | Yes | Conditional | Yes | Yes | `exception_detected`, `retry_pending`, `recovery_pending`, or `failed_terminal` | Yes for response selection | Yes when material; normally requested or referenced | Yes | Terminal Runtime failure requires independent validation. |
| `blocked` | Yes | Policy-dependent | Yes | Yes | A registered specialised blocked State, `manual_intervention_required`, `exception_detected`, or no change | Yes | Yes when material; normally requested or referenced | Yes | No generic `blocked` Runtime State exists. |
| `cancelled` | Yes, after cleanup/handoff | No for this invocation | Yes | Yes | `cancelled` when the Orchestrator validates workflow cancellation, otherwise cleanup/recovery path | Yes when side effects, policy, or authority are material | Conditional | Yes | Cancellation preserves partial outputs, side effects, and history. |
| `timeout` | Yes | Policy-dependent | Yes | Yes | `retry_pending`, `recovery_pending`, `exception_detected`, or `timed_out` | Yes | Yes when material or recovery is needed | Yes | `timed_out` is not automatic. |

---

## 16. Prohibited Substitutions

- Engine Invocation Status MUST NOT be committed as Runtime State.
- Runtime State MUST NOT be used as Engine Result `status`.
- An event type MUST NOT be used as status or State.
- A Decision status MUST NOT be used as Engine Invocation Status or Runtime State.
- An Execution Result status MUST NOT replace Engine Result status.
- Successful Engine completion MUST NOT imply workflow completion or closure.
- Waiting MUST NOT be represented as success.
- Blocked MUST NOT be treated as terminal workflow failure without governing policy and a validated transition.
- Generic `blocked` MUST NOT be proposed as Runtime State.
- Timeout MUST NOT be silently normalised to failed.
- `timeout` MUST NOT be substituted for Runtime State `timed_out`, or vice versa.
- Cancellation MUST NOT erase partial outputs, side effects, Exception Records, Decisions, events, or history.
- A failed invocation MUST NOT directly commit `failed_terminal`.
- Similar spelling MUST NOT be treated as evidence of equivalent meaning.

---

## 17. Recommended Architecture

The recommended canonical model has one final Engine Result status field and event-based lifecycle observation:

```text
Engine invocation lifecycle
  └── observed through Runtime Events

Engine Result
  ├── status: one of seven final invocation dispositions
  ├── primary_output / secondary_outputs: produced objects or references
  ├── decision_ref: governed decision evidence
  ├── failure / recovery / exception_ref: abnormal-path evidence
  └── state_transition: optional proposal or reference
                         ↓
Runtime Orchestrator validation
                         ↓
Committed State Transition
                         ↓
Runtime State
```

Canonical enum:

```text
waiting | succeeded | partial_success | failed | blocked | cancelled | timeout
```

The Engine Contract owns meaning. The Engine Result schema owns machine enforcement. State Model remains the exclusive owner of State and transition semantics. No status registry or new Runtime Object is required.

---

## 18. Rejected Alternatives

### 18.1 One Combined Status/State Enum

Rejected because invocation scope and workflow scope have different identities, owners, lifecycles, and commit authority. A combined enum would permit Engines to bypass the Orchestrator.

### 18.2 Add `ready` and `running` to Engine Result Status

Rejected because a final Engine Result cannot truthfully report that the same invocation is still ready or running. Progress belongs in Runtime Events.

### 18.3 Add a Mandatory `invocation_phase` Field to Engine Result

Rejected as redundant. Engine Result is returned at the final disposition boundary; lifecycle phases are already observable through events.

### 18.4 Rename Engine Status `timeout` to `timed_out`

Rejected because `timed_out` is already a canonical Runtime State. Keeping `timeout` in the explicitly namespaced Engine Result field reduces accidental substitution and preserves the existing schema.

### 18.5 Map Every Status to One Runtime State

Rejected because the valid target depends on current State, produced objects, Decisions, policy, approval, recovery eligibility, and transition guards.

### 18.6 Treat Waiting or Blocked as a Still-Open Invocation

Rejected because it creates indefinitely open response envelopes and weakens retry, Context freshness, cancellation, and audit boundaries. A future resume uses a new invocation identity.

---

## 19. Compatibility and Migration

| Existing value or construct | Disposition | Migration |
|---|---|---|
| `ready` as Engine status | Rejected for Engine Result | Emit lifecycle event; do not return as `status`. |
| `running` as Engine status | Rejected for Engine Result | Emit `engine_started` and progress events; do not return as `status`. |
| `waiting` | Retained | Clarify as final invocation disposition with resumable workflow semantics. |
| `succeeded` | Retained | Clarify that it is local invocation success only. |
| `success` | Rejected alias | Convert producer code and tests to `succeeded`; reject at validation. |
| `partial_success` | Retained | Require incomplete-scope and follow-up evidence. |
| `failed` | Retained | Require failure evidence; prohibit automatic terminal-State mapping. |
| `blocked` | Retained | Require failure/blocking evidence; map only through registered State proposals. |
| `cancelled` | Retained | Return only after cleanup or cleanup handoff is recorded. |
| `timeout` | Retained | Preserve as invocation disposition and timeout cause. |
| `timed_out` as Engine status | Rejected | Use only as registered Runtime State. |
| `invocation_created`, `invocation_ready`, `invocation_running`, `invocation_completed` | Represented elsewhere | Use lifecycle/event semantics, not Engine Result status. |
| Direct status-to-State assignment | Rejected | Replace with an explicit State Transition proposal and Orchestrator validation. |

Compatibility is mostly clarifying because the current Engine Result schema already enforces the recommended seven values. The principal breaking change is removal of `ready` and `running` from the Engine Contract's purported allowed result statuses; conforming schema-based producers should already reject them.

---

## 20. Required Implementation Changes

| File | Classification | Later implementation change |
|---|---|---|
| `../engines/shared/engine_contract.md` | Required change | Define status as final invocation disposition; remove `ready` and `running` from allowed statuses; clarify finality, resumability, timeout, cancellation, events, and State proposal boundary. |
| `../schemas/engine_result.schema.json` | Required change | Preserve the seven-value enum; improve descriptions and conditional validation for waiting, cancellation, timeout, recovery, failure, and event evidence. |
| `../engines/shared/state_model.md` | Optional clarification | Add an explicit prohibition on treating Engine Result status as Runtime State; no State names or transitions need change for H-06. |
| `../engines/shared/decision_model.md` | Optional clarification | Cross-reference the status-to-transition protocol for abnormal-path Decisions; do not redefine status. |
| `../engines/shared/runtime_objects.md` | Optional clarification | Strengthen the Engine Result, State Transition, Event, and Runtime State boundary in the human-readable view. |
| `../registry/runtime_objects.json` | No change | Existing owners and relationships already support this design. |
| `../schemas/README.md` | Required change | Document the canonical seven-value Engine Result enum and distinguish it from object status and Runtime State. |
| `../README.md` | Optional clarification | Add a short status/State boundary note or link after implementation. |
| `../schemas/execution_result.schema.json` | No change | Execution Result status is a separate object lifecycle enum. |
| `../schemas/exception_record.schema.json` | No change | Existing Exception Record lifecycle remains separate. |

No new schema, Runtime State, Runtime Object, Decision type, or event registry is required to implement H-06.

---

## 21. Validation Strategy

Implementation validation must include:

1. schema validation that accepts exactly the seven canonical Engine Result statuses;
2. negative tests rejecting `ready`, `running`, `success`, `timed_out`, Runtime State names, event names, and object statuses in Engine Result `status`;
3. contract tests proving every returned Engine Result contains one final disposition;
4. tests that waiting and blocked returns use a new invocation identity on resume;
5. tests that cancellation records cleanup or cleanup handoff;
6. tests that timeout remains distinguishable from generic failure;
7. tests that State proposals contain only registered Runtime States;
8. tests that no Engine directly commits Runtime State;
9. tests showing `succeeded` does not imply `workflow_completed`;
10. tests showing `failed` and `timeout` do not imply terminal State;
11. Decision and Exception Record checks for governed abnormal paths;
12. required Runtime Event and correlation checks;
13. strict JSON parsing and schema validation for all examples;
14. cross-document terminology and ownership checks.

---

## 22. H-06 Acceptance Criteria

H-06 is closed only when:

- [ ] the Engine Contract is the sole semantic owner of one canonical Engine Invocation Status enum;
- [ ] `engine_result.schema.json` machine-validates exactly that enum;
- [ ] `ready` and `running` are lifecycle/event concepts, not final statuses;
- [ ] every canonical status has unambiguous invocation-final semantics;
- [ ] workflow resumability is explicit for waiting, blocked, failure, and timeout paths;
- [ ] Engine Result status cannot be substituted for Runtime State;
- [ ] Runtime State cannot be substituted for Engine Result status;
- [ ] the status-to-Engine Result-to-transition-to-State protocol is normative;
- [ ] timeout status, cause, event, Decision, and `timed_out` State are distinguished;
- [ ] successful invocation does not imply workflow completion;
- [ ] failed invocation does not imply terminal workflow failure;
- [ ] cancellation preserves cleanup, partial effects, and history;
- [ ] Engine Result schema conditions align with prose requirements;
- [ ] all status values and negative cases are machine-tested;
- [ ] all referenced Runtime States are registered;
- [ ] all referenced Runtime Objects are registered;
- [ ] all JSON examples in changed specifications are valid;
- [ ] no High-severity status/State ambiguity remains.

---

## 23. Open Questions

The following implementation details do not alter the H-06 boundary but require policy decisions during implementation:

1. Does resuming from `waiting` increment `attempt_number`, or only create a new `invocation_id`?
2. Which materiality threshold makes an Exception Record mandatory for `partial_success`, `cancelled`, or `timeout`?
3. Which Engine capabilities may return `partial_success`?
4. What expiry policy converts an unresolved blocked workflow into escalation, cancellation, or terminal failure?
5. Should later observability work standardise `engine_completed` as a single return event with status, or retain status-specific terminal events alongside it?

These are operational-policy questions, not unresolved ownership or substitution ambiguities.

---

## 24. Recommendation

Approve the seven-value Engine Result status enum already enforced by `engine_result.schema.json`, remove `ready` and `running` from the Engine Contract's allowed final statuses, and make the following protocol normative in the later implementation task:

```text
Invocation lifecycle is observed through events.
Engine Result reports one final invocation disposition.
Engine Result may propose a registered State Transition.
The Runtime Orchestrator validates and commits Runtime State.
```

This is the smallest architecture that resolves H-06, preserves existing owners and schemas, prevents silent substitution, and avoids creating a parallel lifecycle system.
