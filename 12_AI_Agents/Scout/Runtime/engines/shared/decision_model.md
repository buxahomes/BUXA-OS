# Decision Model

**Document ID:** `SCOUT-RUNTIME-DECISION-MODEL`
**Version:** `1.0.0`
**Status:** `Approved`
**Owner:** `Scout Runtime`
**Applies To:** Runtime Orchestrator and all Runtime Engines
**Dependencies:** `engine_contract.md`, `engine_context.md` version `1.0.0`, `state_model.md`, Runtime schemas
**Specification Type:** Mandatory Shared Foundation Standard

---

## 1. Purpose

This document defines the authoritative Decision Model for Scout Runtime.

`decision_model.md` is the sole normative authority for Decision identity, required fields, types, confidence, evidence, approval requirements, lifecycle, validity, supersession, and retry, recovery, rollback, revision, approval, and policy Decision semantics. Other Runtime specifications MUST reference this model and MUST NOT create or redefine universal Decision structure or lifecycle.

The Decision Model standardises how the Runtime Orchestrator and all Runtime Engines identify, form, evaluate, record, validate, replay, audit, and govern decisions.

It supplies the sole normative Decision contract used by `engine_contract.md` and aligns with:

- the immutable Engine Context defined in `engine_context.md`;
- the lifecycle and transition controls defined in `state_model.md`;
- the Runtime Object contracts defined in `Runtime/schemas/`;
- the Scout Operating Manual principles of Knowledge Before Generation, Evidence Before Assumption, Reason Before Execution, Quality Before Delivery, Governance Before Autonomy, and Transparency Before Certainty.

The Decision Model exists so that Scout Runtime can transform knowledge into action without relying on hidden reasoning, informal preferences, unrecorded assumptions, or uncontrolled autonomy.

---

## 2. Scope

This specification applies to every material Runtime decision, including decisions made by:

- Runtime Orchestrator;
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
- future registered Runtime Engines.

This specification covers decisions embedded in existing Runtime Objects, including:

- Task;
- Knowledge Package;
- Business Judgement;
- Execution Blueprint;
- Skill Output;
- Quality Report;
- Execution Result;
- Exception Record;
- Learning Candidate;
- Memory Record;
- Engine Result;
- Approval Decision;
- Policy Decision;
- State Transition;
- Context Snapshot;
- State Snapshot.

This document does not create a new required Runtime Object schema. A Decision is a governed structured payload or record inside an existing Runtime Object, Engine Result, transition record, audit event, or future approved schema.

---

## 3. Normative Language

The following terms are normative:

- **MUST / SHALL** - mandatory requirement.
- **MUST NOT / SHALL NOT** - prohibited behaviour.
- **SHOULD** - recommended unless a documented exception exists.
- **SHOULD NOT** - discouraged unless justified.
- **MAY** - optional behaviour.
- **REQUIRED** - mandatory field, control, status, or validation.

Any deviation from a MUST or MUST NOT requirement requires:

1. a documented exception;
2. an identified owner;
3. explicit approval;
4. a defined review or expiry date;
5. an audit record;
6. a compatibility assessment where Runtime behaviour is affected.

---

## 4. Decision Definition

A Decision is a structured, traceable selection, classification, approval, rejection, routing, recovery, or governance outcome produced during Runtime execution.

A Decision MUST state:

- what was decided;
- who or what produced the decision;
- which Runtime Object, Task, execution, state, or Engine invocation it belongs to;
- which alternatives were considered where applicable;
- which evidence, policy, constraints, and assumptions informed it;
- what confidence, risk, materiality, and reversibility apply;
- whether approval or human review is required;
- what state transition, output, retry, recovery, rollback, escalation, or downstream action follows.

A Decision MUST NOT be represented only as unstructured prose, hidden model reasoning, a log line, or an implicit state change.

---

## 5. Decision Principles

### 5.1 Governance Before Autonomy

A Decision MUST operate within approved organisational governance. A high-confidence Decision MUST NOT bypass approval, policy, security, privacy, quality, or authority controls.

### 5.2 Knowledge Before Decision

A Decision SHOULD be formed only after relevant Business Operating System knowledge, approved Runtime Objects, applicable Memory Records, and controlling policies have been retrieved or explicitly marked unavailable.

### 5.3 Evidence Before Assumption

Evidence MUST take priority over assumptions. Where assumptions are used, they MUST be explicit, materiality-assessed, confidence-assessed, and reviewable.

### 5.4 Reason Before Execution

A material execution action MUST be preceded by a Decision that explains why the action is appropriate and under what constraints.

### 5.5 Alternatives Before Recommendation

Where multiple feasible paths exist, the Decision SHOULD compare alternatives before selecting a preferred outcome.

### 5.6 Traceability Before Convenience

A Decision MUST be traceable to Engine Context, input Runtime Objects, evidence references, source references, policies, configuration, and state.

### 5.7 Explicit Unknowns

Unknowns MUST remain visible. A Decision MUST NOT convert unknown information into guessed facts.

### 5.8 Quality Before Commit

A Decision that creates a material output, approval, policy outcome, state transition, memory update, external side effect, or terminal outcome MUST satisfy applicable validation and quality gates before it is committed or acted upon.

### 5.9 Human Authority

Human governance overrides autonomous execution. Human review, approval, denial, or override MUST be recorded with scope, conditions, actor, time, and evidence.

### 5.10 Continuous Learning

Material Decisions SHOULD generate decision history suitable for feedback, learning, replay, and Memory evaluation.

---

## 6. Decision Architecture

The Runtime Decision Architecture has seven layers.

```text
Decision Architecture
├── Decision Trigger
├── Decision Context
├── Evidence and Assumptions
├── Constraints and Policies
├── Alternative Evaluation
├── Decision Outcome
└── Decision Record and Audit
```

### 6.1 Decision Trigger

A Decision begins when an Engine or the Runtime Orchestrator must select, classify, approve, block, retry, recover, rollback, escalate, or commit.

### 6.2 Decision Context

Decision formation MUST use only the authorised Engine Context defined by `engine_context.md` version `1.0.0` and MUST use the minimum authorised information required to decide safely. This Decision Model does not define a separate Runtime context structure.

### 6.3 Evidence and Assumptions

Evidence, assumptions, unknowns, conflicts, freshness, and integrity status form the factual basis of the Decision.

### 6.4 Constraints and Policies

Task constraints, Runtime configuration, approval requirements, policy requirements, security controls, privacy controls, resource limits, and state guards constrain what outcomes are permitted.

### 6.5 Alternative Evaluation

Reasonable alternatives SHOULD be evaluated against defined criteria, weights, mandatory constraints, risk, reversibility, materiality, confidence, and business value.

### 6.6 Decision Outcome

The outcome MUST identify selected action, rejected actions where applicable, conditions, next state proposal, review requirement, and invalidation triggers.

### 6.7 Decision Record and Audit

Every material Decision MUST be stored or referenced in a structured Runtime Object, Engine Result, State Transition, or audit record.

---

## 7. Decision Lifecycle

Every material Decision follows this lifecycle.

```text
decision_required
↓
decision_context_assembled
↓
evidence_evaluated
↓
alternatives_evaluated
↓
outcome_selected
↓
decision_validated
↓
decision_recorded
↓
decision_applied_or_waiting
↓
decision_reviewed
↓
decision_learned_or_archived
```

### 7.1 Lifecycle Rules

A Decision MUST NOT move to `decision_applied_or_waiting` until mandatory validation has passed or the Decision has intentionally entered an approval, waiting, blocked, exception, retry, recovery, rollback, or escalation path.

A Decision MAY be revised before approval. A material revision after approval MUST create a new version or supersession relationship.

A Decision that affects committed Runtime State MUST be represented through a State Transition proposal and MUST be committed only by the Runtime Orchestrator.

---

## 8. Decision Categories and Types

Decision categories describe where the Decision operates in the Runtime lifecycle.

### 8.1 Canonical Categories

The following categories SHOULD be used where applicable:

- `task_decision`;
- `retrieval_decision`;
- `business_judgement_decision`;
- `planning_decision`;
- `skill_decision`;
- `execution_decision`;
- `quality_decision`;
- `approval_decision`;
- `policy_decision`;
- `exception_decision`;
- `retry_decision`;
- `recovery_decision`;
- `rollback_decision`;
- `escalation_decision`;
- `learning_decision`;
- `memory_decision`;
- `orchestration_decision`;
- `state_transition_decision`.

### 8.2 Canonical Decision Types

The following types SHOULD be used where applicable:

- `classification`;
- `selection`;
- `prioritisation`;
- `routing`;
- `proceed`;
- `proceed_with_conditions`;
- `do_not_proceed`;
- `request_clarification`;
- `request_more_knowledge`;
- `approve`;
- `reject`;
- `revise`;
- `waive`;
- `block`;
- `defer`;
- `retry`;
- `fallback`;
- `recover`;
- `rollback`;
- `cancel`;
- `escalate`;
- `commit_transition`;
- `record_learning`;
- `admit_memory`;
- `reject_memory`.

Decision types MUST NOT replace canonical Runtime State names. They explain why a Runtime action is selected; state names define where execution is.

---

## 9. Decision Identity and Metadata

Every material Decision MUST have stable identity metadata.

Required fields:

- `decision_id`;
- `decision_category`;
- `decision_type`;
- `decision_version`;
- `task_id`;
- `execution_id`;
- `created_at`;
- `created_by`;
- `status`;
- `scope`;
- `owner`;
- `authority_level`;
- `related_object_ids`;
- `current_state`;
- `decision_basis`;
- `selected_outcome`;
- `confidence`;
- `risk`;
- `approval`;
- `validation`.

Recommended identifier pattern:

```text
decision_<uuid>
```

Decision identity MUST be immutable. If a material Decision changes after validation, the Runtime MUST create a new Decision version or a superseding Decision.

---

## 10. Decision Ownership and Authority

### 10.1 Ownership

The Engine that creates the owning Runtime Object owns the Decision payload inside that object unless this specification or an approved Engine capability matrix assigns ownership elsewhere.

Default ownership:

| Decision Location | Primary Owner |
|---|---|
| Knowledge Package retrieval decisions | Retrieval Engine |
| Business Judgement decisions | Business Judgement Engine |
| Execution Blueprint planning decisions | Planning Engine |
| Skill Output local decisions | Execution Engine or owning Skill |
| Quality decisions | Quality Engine |
| Exception decisions | Exception Engine |
| Learning decisions | Learning Engine |
| Memory admission decisions | Memory Engine |
| Approval decisions | Approval Engine |
| Policy decisions | Policy Engine |
| State transition decisions | Runtime Orchestrator |

### 10.2 Authority

Ownership does not grant unlimited authority.

An Engine MAY create and update Decisions within its owned Runtime Object before approval.

An Engine MUST NOT:

- approve its own restricted Decision unless explicitly authorised;
- commit global Runtime State;
- override Approval Engine or Policy Engine outcomes;
- change another Engine's Decision without authority;
- delete Decision history;
- silently broaden Decision scope;
- convert a blocked Decision into success.

### 10.3 Orchestrator Authority

The Runtime Orchestrator owns committed top-level Runtime State and transition commitment. It MAY reject an Engine's proposed Decision outcome if guards, policy, approval, security, privacy, freshness, integrity, or state validation fail.

---

## 11. Decision Inputs and Outputs

### 11.1 Inputs

Decision inputs MAY include:

- Task objective, scope, constraints, success criteria, authority, risk, and approval requirements;
- Engine Context;
- current Runtime State;
- Knowledge Package;
- Business Judgement;
- Execution Blueprint;
- Skill Outputs;
- Quality Reports;
- Execution Results;
- Exception Records;
- Learning Candidates;
- Memory Records;
- applicable policies;
- approval context;
- security and privacy context;
- configuration;
- resource limits;
- source references;
- evidence references;
- state history;
- snapshots.

### 11.2 Outputs

Decision outputs MAY include:

- selected outcome;
- rejected outcomes;
- deferred outcomes;
- conditions;
- required review;
- warnings;
- blocking issues;
- state transition proposal;
- approval request;
- policy decision;
- quality disposition;
- exception creation request;
- retry instruction;
- recovery instruction;
- rollback instruction;
- escalation request;
- learning candidate;
- Memory Record recommendation;
- audit event.

Every output MUST identify whether it is final, proposed, conditional, blocked, waiting, or superseded.

---

## 12. Decision Context

`engine_context.md` version `1.0.0` is the sole normative authority for Runtime Engine Context, including Context structure, metadata, lifecycle, ownership, integrity, and validation.

For Decision formation, an Engine MUST select required authorised information from its Engine Context. This selection does not create a separate Context type and MUST NOT extend, override, or redefine Engine Context.

An Engine MUST NOT use hidden prompt memory, private state, unregistered cache entries, or undocumented external knowledge as authoritative Decision input.

---

## 13. Evidence Model

### 13.1 Evidence Requirements

A material Decision MUST identify supporting evidence or explicitly state why evidence is unavailable.

Evidence SHOULD include:

- source type;
- source location;
- source authority;
- retrieval time;
- evidence role;
- verified status;
- freshness status;
- integrity status;
- applicable claim, criterion, risk, alternative, or condition.

### 13.2 Evidence Roles

Evidence roles SHOULD include:

- `supports`;
- `contradicts`;
- `qualifies`;
- `limits`;
- `contextualises`;
- `establishes_rule`;
- `establishes_constraint`;
- `establishes_precedent`.

### 13.3 Evidence Conflicts

A material evidence conflict MUST be resolved through approved precedence rules or escalated.

The Runtime MUST NOT silently select lower-authority evidence over higher-authority evidence.

---

## 14. Assumptions and Unknowns

### 14.1 Assumptions

Assumptions are permitted only when:

- evidence is unavailable or incomplete;
- the assumption is necessary to proceed;
- the assumption is explicitly recorded;
- the assumption does not contradict approved knowledge;
- risk and impact are assessed;
- review or validation is defined where material.

Material assumptions MUST include:

- statement;
- type;
- status;
- basis;
- materiality;
- confidence;
- impact if false;
- validation method;
- affected alternatives or outcomes.

### 14.2 Unknowns

Unknowns MUST remain explicit. An unknown may produce:

- lower confidence;
- a condition;
- a request for clarification;
- a request for more knowledge;
- a blocked Decision;
- escalation.

Unknowns MUST NOT be converted into facts for convenience.

---

## 15. Constraints and Preconditions

A Decision MUST evaluate applicable constraints and preconditions before outcome selection.

Constraint categories SHOULD include:

- governance;
- brand;
- product;
- content;
- quality;
- legal;
- financial;
- technical;
- time;
- format;
- platform;
- language;
- source;
- approval;
- privacy;
- security;
- scope;
- resource;
- state;
- dependency.

If a mandatory precondition fails, the Decision MUST select one of:

- `request_clarification`;
- `request_more_knowledge`;
- `waiting`;
- `block`;
- `retry`;
- `recover`;
- `rollback`;
- `escalate`;
- `cancel`;
- `fail`.

The Engine MUST NOT continue normal execution after a failed mandatory precondition.

---

## 16. Alternatives

Alternative evaluation is REQUIRED when:

- the Task asks for selection or recommendation;
- multiple viable execution paths exist;
- governance or policy requires comparison;
- risk is material;
- an irreversible or externally visible side effect is possible;
- confidence is below the configured threshold;
- a human reviewer needs options.

Each alternative SHOULD include:

- alternative ID;
- name;
- description;
- status;
- criterion evaluations;
- advantages;
- disadvantages;
- overall score;
- mandatory criteria result;
- feasibility;
- risk level;
- confidence;
- assumptions;
- evidence references;
- rejection reasons where applicable.

---

## 17. Rules and Policies

Policy evaluation MUST occur before a governed action is selected or committed.

A Policy Decision MUST identify:

- applicable policies;
- policy version;
- authority;
- scope;
- applicability reason;
- precedence;
- enforcement mode;
- conflicts;
- required enforcement action;
- review or escalation requirement.

Allowed enforcement modes are:

- `advisory`;
- `warning`;
- `mandatory`;
- `blocking`.

A blocking policy outcome MUST prevent normal execution unless a formally approved waiver exists and the controlling standard permits waiver.

---

## 18. Evaluation Process

The standard Decision evaluation process is:

```text
Frame Decision Question
↓
Confirm Authority and Scope
↓
Load Decision Context
↓
Evaluate Evidence
↓
Identify Assumptions and Unknowns
↓
Identify Mandatory Constraints
↓
Generate Alternatives
↓
Score Alternatives
↓
Assess Risk, Materiality, Reversibility, and Confidence
↓
Select Outcome
↓
Validate Decision
↓
Record Decision
↓
Apply, Wait, Block, or Escalate
```

The Decision summary MUST be sufficient for an authorised reviewer to understand the basis of the Decision without exposing private internal chain-of-thought.

---

## 19. Scoring

### 19.1 Scoring Purpose

Scoring provides structured comparison. It does not replace policy, approval, evidence, or human judgement.

### 19.2 Score Range

Scores SHOULD use a normalised range:

```text
0.00-1.00
```

Where weighted criteria are used:

- each criterion MUST identify weight;
- total weight SHOULD equal `1.0`;
- mandatory criteria MUST be evaluated separately from weighted score;
- failure of a mandatory criterion SHOULD block or reject the alternative regardless of score;
- scoring rationale MUST be recorded.

### 19.3 Scoring Prohibitions

An Engine MUST NOT:

- select an alternative solely because it has the highest score if mandatory constraints fail;
- hide low evidence quality behind numeric precision;
- use unregistered weights for material Decisions;
- treat a score as approval.

---

## 20. Confidence

Every material Decision MUST include confidence.

Confidence MUST use the common structured confidence model where practical:

- `level`;
- `score`;
- `basis`;
- `limitations`.

Recommended levels:

- `very_low`;
- `low`;
- `medium`;
- `high`;
- `very_high`.

When a score is used, it MUST be between `0.0` and `1.0`.

Confidence MUST reflect:

- evidence quality;
- evidence completeness;
- source authority;
- source freshness;
- source integrity;
- unresolved conflicts;
- assumption materiality;
- policy clarity;
- model or tool uncertainty;
- operational experience.

High confidence MUST NOT compensate for missing approval, failed validation, policy conflict, security or privacy violation, corrupt state, or missing authoritative evidence.

---

## 21. Risk

Material Decisions MUST assess risk.

Risk SHOULD use common Runtime levels:

- `negligible`;
- `low`;
- `medium`;
- `high`;
- `critical`.

Risk assessment SHOULD include:

- risk categories;
- likelihood or probability;
- impact;
- inherent risk;
- mitigations;
- residual risk;
- risk owner;
- escalation trigger;
- acceptance requirement.

Critical risk MUST block autonomous execution unless authorised human approval and policy permit continuation.

---

## 22. Reversibility

Reversibility determines how safely a Decision can be undone.

Recommended reversibility values:

- `fully_reversible`;
- `reversible_with_compensation`;
- `partially_reversible`;
- `not_reversible`;
- `unknown`.

Irreversible or partially reversible Decisions SHOULD require stronger evidence, lower risk, explicit approval, rollback or compensation analysis, and expanded audit.

Rollback availability MUST NOT be claimed unless a valid checkpoint, compensation action, or restoration path exists.

---

## 23. Materiality

Materiality determines how much governance, evidence, review, and audit a Decision requires.

Recommended materiality values:

- `immaterial`;
- `minor`;
- `moderate`;
- `major`;
- `critical`.

Materiality SHOULD consider:

- impact on Business Knowledge;
- impact on brand positioning;
- impact on product facts;
- impact on governance;
- customer visibility;
- financial or legal implications;
- external publication;
- persistent repository change;
- security or privacy impact;
- reversibility;
- operational blast radius.

Major and critical Decisions MUST include approval analysis and human-review determination.

---

## 24. Approval Decisions

Approval Decisions are owned by the Approval Engine or authorised human authority.

An Approval Decision is a specialised Decision. Its canonical identity is `decision_id` plus `decision_version`. It expresses the approval outcome, authority, scope, conditions, validity, and governing rationale. It is the only approval object that a governed State Transition Proposal or Engine Result `approval_reference` may identify.

An Approval Decision is distinct from the immutable Approval Record governed by `engine_context.md`. An Approval Record references exactly one Approval Decision for audit and replay; it MUST NOT duplicate this model's Decision fields. Decision supersession follows this model and preserves earlier versions for audit.

An Approval Decision MUST record:

- approval ID;
- required flag;
- status;
- required role or authority;
- requested from;
- requested at;
- decided by;
- decided at;
- scope;
- conditions;
- expiry;
- evidence;
- related Decision IDs;
- resume target.

Allowed approval statuses SHOULD align with common schema values:

- `not_required`;
- `pending`;
- `approved`;
- `rejected`;
- `revision_required`;
- `expired`.

Approval MUST NOT be inferred from silence, confidence, task completion, or prior unrelated approval.

---

## 25. Policy Decisions

Policy Decisions are owned by the Policy Engine.

A Policy Decision MUST determine:

- applicable policy set;
- policy precedence;
- enforcement mode;
- policy outcome;
- blocking issues;
- waiver availability;
- required approval;
- audit requirement;
- next allowed actions.

A Policy Decision MUST NOT redefine policy. It applies existing policy to the current Engine Context.

---

## 26. Quality Decisions

Quality Decisions are owned by the Quality Engine or by local Engine validation for Engine-level correctness.

A Quality Decision MUST distinguish:

- output produced;
- output valid;
- output quality-reviewed;
- output approved for downstream use;
- output requiring revision;
- output rejected.

Quality outcomes SHOULD align with:

- `passed`;
- `passed_with_conditions`;
- `failed`;
- `revision_required`;
- `quality_waiver_pending`;
- `quality_waived`.

An Engine MUST NOT mark its own output as globally approved unless explicitly authorised.

---

## 27. Exception Decisions

Exception Decisions determine how an abnormal condition changes normal execution.

An Exception Decision MUST:

- create or reference an Exception Record when material;
- classify the exception;
- identify severity;
- assess impact;
- identify containment;
- determine retry, recovery, rollback, escalation, cancellation, or terminal failure;
- preserve learning potential;
- propose a state transition where applicable.

Exceptions MUST NOT be represented only as logs.

---

## 28. Retry Decisions

A Retry Decision MUST be bounded and policy-compliant.

It MUST define:

- retryable failure category;
- current attempt;
- maximum attempts;
- delay;
- backoff;
- jitter where applicable;
- idempotency key or idempotency assessment;
- context changes allowed;
- resource budget;
- escalation threshold;
- stop condition.

Retries MUST preserve the original `execution_id` and create a new attempt identity.

An Engine MUST NOT retry indefinitely.

---

## 29. Recovery Decisions

A Recovery Decision determines how execution can continue after disruption.

Allowed recovery actions include:

- `retry`;
- `retry_with_modified_context`;
- `fallback`;
- `request_input`;
- `request_approval`;
- `escalate`;
- `suspend`;
- `cancel`;
- `abort`.

Recovery MUST record:

- failure origin;
- recovery strategy;
- changed dependencies;
- compatibility impact;
- security impact;
- privacy impact;
- data integrity impact;
- recovery result;
- next state.

Recovery SHOULD return to the earliest appropriate lifecycle stage rather than restarting unnecessary work.

---

## 30. Rollback Decisions

A Rollback Decision restores a prior valid checkpoint or committed state without deleting history.

It MUST identify:

- rollback trigger;
- target checkpoint;
- target state;
- reversible side effects;
- irreversible side effects;
- compensation actions;
- required approval;
- integrity verification;
- post-rollback state;
- audit record.

Rollback MUST NOT erase state history, Engine Results, audit events, or Exception Records.

---

## 31. Escalation Decisions

An Escalation Decision MUST occur when autonomous execution cannot proceed safely.

Escalation SHOULD occur when:

- evidence is insufficient;
- multiple alternatives are equally viable and material;
- business risk is significant;
- authority is unclear;
- policy conflict is unresolved;
- approval is required;
- security or privacy issue exists;
- an exception cannot be recovered;
- a requested action may change approved Business Knowledge, governance, product data, brand positioning, legal commitments, financial commitments, or public publication status.

Escalation MUST include:

- issue;
- decision needed;
- supporting evidence;
- alternatives;
- risks;
- recommendation;
- required authority;
- current state;
- resume target.

---

## 32. Human Review and Override

Human review MAY approve, reject, revise, defer, or override a Runtime Decision within human authority.

Human override MUST record:

- original Decision;
- override Decision;
- human actor;
- authority basis;
- timestamp;
- scope;
- conditions;
- rationale;
- affected Runtime Objects;
- affected State Transition;
- audit event.

Human override MUST NOT delete the original autonomous Decision. It supersedes or constrains it.

---

## 33. Conflict Resolution

Decision conflicts may occur between:

- evidence sources;
- policies;
- approval conditions;
- Runtime State;
- Task constraints;
- Engine outputs;
- Memory Records;
- user instruction and BOS governance;
- security and execution objective;
- quality and delivery speed.

Resolution priority SHOULD follow:

```text
mandatory security and privacy controls
↓
controlling policy
↓
valid approval conditions
↓
committed Runtime State
↓
explicit Task constraints
↓
authoritative current knowledge
↓
approved applicable Memory Records
↓
Engine configuration
↓
non-controlling sources
↓
heuristic or inferred context
```

Material unresolved conflicts MUST block execution or escalate.

---

## 34. Decision Status

Recommended Decision statuses:

- `draft`;
- `pending`;
- `in_progress`;
- `waiting`;
- `validation_required`;
- `approval_required`;
- `approved`;
- `rejected`;
- `revision_required`;
- `blocked`;
- `deferred`;
- `completed`;
- `failed`;
- `cancelled`;
- `superseded`;
- `archived`.

Decision status MUST NOT be confused with Runtime State. A Decision may be `completed` while the workflow remains active.

---

## 35. State Model Integration

Decisions interact with the State Model in three ways.

### 35.1 State Guard Decisions

Policy, approval, quality, security, privacy, freshness, integrity, dependency, and resource checks are Decisions that determine whether a transition guard passes.

### 35.2 Transition Proposal Decisions

An Engine may propose a transition through Engine Result. The Decision MUST identify current state, proposed next state, reason code, guard outcomes, and rollback state where applicable.

### 35.3 Transition Commit Decisions

Only the Runtime Orchestrator may commit top-level Runtime State. Commit Decisions MUST validate source state, target state, authority, guards, policy, approval, security, privacy, freshness, integrity, and concurrency.

---

## 36. Engine Contract Integration

Every Engine Result MUST include a structured Decision summary for material output.

The Decision MUST satisfy the Engine Contract minimum fields:

- decision ID;
- decision type;
- selected outcome;
- alternatives considered where applicable;
- evidence references;
- policy references;
- assumptions;
- confidence;
- risk;
- reason summary;
- review requirement.

An Engine MUST validate its Decision before returning success.

---

## 37. Engine Context Integration

The Engine Context defined by `engine_context.md` version `1.0.0` is the only authorised Decision input envelope.

A Decision MUST record or reference the Context identity and authorised dependencies required by `engine_context.md` and this Decision Model's traceability rules. Context validity, freshness, integrity, lifecycle, and rebuilding requirements are governed exclusively by `engine_context.md`.

If Engine Context fails any mandatory Context validation or use requirement, material Decision formation MUST follow the blocking or rebuilding behaviour required by `engine_context.md`.

---

## 38. Decision History

Decision history is append-only.

Each material history entry SHOULD include:

- Decision ID;
- Decision version;
- prior status;
- new status;
- actor;
- timestamp;
- reason code;
- changed fields;
- affected Runtime Objects;
- affected state;
- validation result;
- approval reference;
- exception reference where applicable.

History MUST NOT be erased during retry, recovery, rollback, cancellation, supersession, or human override.

---

## 39. Versioning

Decision versions SHOULD use monotonic integer versions for Decision payload revisions and semantic versioning for Decision Model specification versions.

A new Decision version is REQUIRED when:

- selected outcome changes;
- approval status changes materially;
- policy basis changes materially;
- evidence basis changes materially;
- risk level changes materially;
- confidence changes materially enough to affect execution;
- scope changes;
- state transition proposal changes;
- human override supersedes an autonomous Decision.

Historical versions MUST remain audit-accessible.

---

## 40. Integrity

Decision integrity controls SHOULD include:

- canonical JSON serialization where practical;
- checksum of material Decision payload;
- source evidence checksums or references;
- Context checksum;
- Runtime Object version references;
- transition hash chain where state is affected;
- immutable audit event references.

Integrity statuses SHOULD align with shared Runtime values:

- `verified`;
- `verified_with_warning`;
- `unverified`;
- `mismatch`;
- `corrupt`.

A mismatch or corrupt integrity status for a material Decision MUST block normal execution.

---

## 41. Freshness

Decision freshness indicates whether the Decision remains valid in the current Runtime context.

Freshness MAY become stale because:

- source knowledge changed;
- policy changed;
- approval expired;
- security or privacy claim expired;
- Runtime State changed;
- Context expired;
- configuration changed materially;
- dependency changed;
- Task scope changed.

Decision freshness statuses SHOULD align with the applicable values and rules owned by `engine_context.md`; this Decision Model does not enumerate or redefine Engine Context freshness.

Stale material Decisions MUST be revalidated before execution continues.

---

## 42. Observability

Every material Decision MUST emit or be linked to observable events.

Recommended events:

- `decision_required`;
- `decision_context_loaded`;
- `decision_evidence_evaluated`;
- `decision_alternatives_evaluated`;
- `decision_selected`;
- `decision_validated`;
- `decision_blocked`;
- `decision_approval_required`;
- `decision_policy_blocked`;
- `decision_state_transition_proposed`;
- `decision_committed`;
- `decision_superseded`;
- `decision_overridden`;
- `decision_replayed`.

Events MUST include trace ID, execution ID, task ID, Engine ID or Orchestrator ID, Decision ID, timestamp, status, severity where applicable, and related Runtime Object IDs.

---

## 43. Replay and Audit

A replayable Decision MUST preserve or reference:

- Task;
- Engine Context;
- Engine version;
- Runtime configuration;
- policy version;
- approval status;
- Runtime State;
- input Runtime Objects;
- Knowledge Package;
- Memory Records;
- evidence;
- model or algorithm settings where applicable;
- tool versions;
- limits;
- timestamps;
- checksums;
- source snapshots where required.

Decision replay modes SHOULD align with the applicable modes and rules owned by `engine_context.md`; this Decision Model does not enumerate or redefine them.

Every substitution during compatible replay MUST be recorded.

---

## 44. Snapshots

Decision Snapshots are immutable point-in-time representations of material Decisions.

A Decision Snapshot SHOULD record:

- snapshot ID;
- Decision ID;
- Decision version;
- created at;
- Context ID;
- Engine identity;
- execution identity;
- related Runtime Objects;
- selected outcome;
- confidence;
- risk;
- approval status;
- freshness status;
- integrity proof;
- dependency versions.

Snapshots support replay, audit, recovery, rollback, regression testing, and governance review.

---

## 45. Governance

Decision governance follows the BUXA OS authority hierarchy:

```text
People
↓
Official Business Knowledge
↓
Business Governance
↓
Artificial Intelligence
↓
Automation
```

Decisions MUST preserve this hierarchy.

Scout Runtime MAY make autonomous operational Decisions inside delegated boundaries.

Scout Runtime MUST request approval before Decisions that may:

- change approved Business Knowledge;
- change governance;
- change product specifications;
- change brand positioning;
- create legal or financial commitments;
- publish external content;
- delete or overwrite approved organisational assets;
- perform irreversible external side effects;
- exceed approved risk tolerance.

---

## 46. Validation

Decision validation MUST include:

1. structural validation;
2. semantic validation;
3. authority validation;
4. evidence validation;
5. policy validation;
6. approval validation;
7. security and privacy validation;
8. risk validation;
9. confidence validation;
10. state transition validation where applicable;
11. freshness validation;
12. integrity validation;
13. cross-object validation;
14. observability validation.

Validation outcomes SHOULD include:

- `passed`;
- `passed_with_warning`;
- `failed`;
- `blocked`.

Failed mandatory validation MUST prevent Decision application.

---

## 47. Canonical JSON Examples

Every fenced JSON example in this document has one explicit conformance category:

| Example | Category | Conformance meaning |
|---|---|---|
| Business Judgement Decision | `complete_decision` | Complete material Decision containing every field required by Sections 9 and 36. |
| Alternative Evaluation | `specialised_payload` | Embedded under `alternative_evaluation` in a complete Decision; not independently valid. |
| Approval Decision | `complete_decision` | Complete Approval Decision; it is not an Approval Record. |
| Retry Decision with Recovery Instruction | `complete_decision` | Complete Retry Decision containing a governed recovery instruction. |
| State Transition Decision | `complete_decision` | Complete Orchestrator-owned transition commit Decision. |
| Decision Snapshot | `snapshot_or_history_object` | Immutable snapshot of a Decision; not a Decision object. |
| JSON Validation Certificate | `validation_result` | Syntax-validation result; not a Decision object. |

For example conformance, the universal Decision contract combines the identity and governance fields required by Section 9 with the Engine Contract integration fields required by Section 36. An example classified as `complete_decision` MUST contain all of those fields. A specialised payload inherits the universal fields from its containing Decision and MUST NOT be validated or used as a standalone Decision.

### 47.1 Complete Business Judgement Decision

```json
{
  "decision_id": "decision_550e8400-e29b-41d4-a716-446655440000",
  "decision_category": "business_judgement_decision",
  "decision_type": "selection",
  "decision_version": 1,
  "task_id": "task_550e8400-e29b-41d4-a716-446655440001",
  "execution_id": "exec_550e8400-e29b-41d4-a716-446655440002",
  "created_at": "2026-07-28T12:00:00Z",
  "created_by": {
    "actor_type": "engine",
    "actor_id": "business_judgement_engine",
    "display_name": "Business Judgement Engine",
    "role": "reasoning",
    "version": "1.0.0"
  },
  "status": "completed",
  "scope": "task_specific",
  "owner": "business_judgement_engine",
  "authority_level": "engine_recommendation",
  "current_state": "business_judgement_running",
  "related_object_ids": [
    "kp_550e8400-e29b-41d4-a716-446655440003"
  ],
  "decision_question": "Which execution direction best satisfies the task objective and governance constraints?",
  "selected_outcome": "proceed_with_conditions",
  "selected_option_ids": [
    "option_traceable_runtime_spec"
  ],
  "rejected_option_ids": [
    "option_unstructured_guidance"
  ],
  "decision_basis": "The selected option preserves existing Runtime architecture, references mandatory source files, and produces a reviewable shared foundation specification.",
  "reason_summary": "Select the traceable Runtime specification because it satisfies governance and maintainability requirements.",
  "review_requirement": {
    "required": false,
    "reason": null
  },
  "evidence_refs": [
    "evidence_engine_contract_decision_contract",
    "evidence_state_model_orchestrator_authority"
  ],
  "policy_refs": [
    "policy_governance_before_autonomy"
  ],
  "assumptions": [
    {
      "assumption_id": "assumption_no_new_schema_required",
      "statement": "A separate decision schema is not required for this specification version.",
      "status": "accepted",
      "materiality": "moderate",
      "confidence": {
        "level": "high",
        "score": 0.86,
        "basis": "Existing schemas already embed decision fields in Business Judgement, Skill Output, Learning Candidate, Engine Result, Approval Decision, and Policy Decision contexts.",
        "limitations": [
          "A future implementation may introduce a dedicated decision schema after operational need is demonstrated."
        ]
      }
    }
  ],
  "alternatives_considered": [
    {
      "option_id": "option_traceable_runtime_spec",
      "status": "preferred",
      "score": 0.92
    },
    {
      "option_id": "option_unstructured_guidance",
      "status": "rejected",
      "score": 0.31
    }
  ],
  "confidence": {
    "level": "high",
    "score": 0.9,
    "basis": "The decision is supported by approved shared Runtime specifications and schemas.",
    "limitations": [
      "No dedicated decision schema currently exists."
    ]
  },
  "risk": {
    "level": "low",
    "summary": "Low architectural risk because the Decision Model extends existing contracts without changing schemas.",
    "risk_categories": [
      "runtime",
      "governance"
    ],
    "mitigations": [
      "Use existing Runtime Object names and state names."
    ],
    "residual_risk_level": "low"
  },
  "approval": {
    "required": false,
    "status": "not_required",
    "required_role": null,
    "requested_from": null,
    "requested_at": null,
    "decided_by": null,
    "decided_at": null,
    "expires_at": null,
    "notes": "This is an internal specification addition requested by an authorised user and does not alter approved business knowledge."
  },
  "validation": {
    "status": "valid",
    "validated_at": "2026-07-28T12:00:00Z",
    "validator": "decision_model.semantic_review",
    "schema_version_validated": "1.0.0",
    "errors": [],
    "warnings": []
  }
}
```

### 47.2 Alternative Evaluation Specialised Payload

This specialised payload is contained in the `alternative_evaluation` field of a complete Decision. It inherits `decision_id`, `decision_category`, `decision_type`, `decision_version`, `task_id`, `execution_id`, `created_at`, `created_by`, `status`, `scope`, `owner`, `authority_level`, `related_object_ids`, `current_state`, `decision_basis`, `selected_outcome`, `confidence`, `risk`, `approval`, `validation`, `alternatives_considered`, `evidence_refs`, `policy_refs`, `assumptions`, `reason_summary`, and `review_requirement` from that containing Decision. The repeated `decision_id` is a correlation reference. This payload is not independently valid as a Decision.

```json
{
  "decision_id": "decision_550e8400-e29b-41d4-a716-446655440010",
  "criteria": [
    {
      "criterion_id": "criterion_governance_alignment",
      "name": "Governance Alignment",
      "mandatory": true,
      "weight": 0.3
    },
    {
      "criterion_id": "criterion_traceability",
      "name": "Traceability",
      "mandatory": true,
      "weight": 0.25
    },
    {
      "criterion_id": "criterion_operational_simplicity",
      "name": "Operational Simplicity",
      "mandatory": false,
      "weight": 0.2
    },
    {
      "criterion_id": "criterion_reversibility",
      "name": "Reversibility",
      "mandatory": false,
      "weight": 0.15
    },
    {
      "criterion_id": "criterion_business_value",
      "name": "Business Value",
      "mandatory": false,
      "weight": 0.1
    }
  ],
  "alternatives": [
    {
      "option_id": "option_create_shared_spec",
      "name": "Create shared Decision Model specification",
      "mandatory_criteria_met": true,
      "criterion_scores": {
        "criterion_governance_alignment": 1.0,
        "criterion_traceability": 0.95,
        "criterion_operational_simplicity": 0.85,
        "criterion_reversibility": 0.9,
        "criterion_business_value": 0.9
      },
      "weighted_score": 0.93,
      "risk_level": "low",
      "confidence": {
        "level": "high",
        "score": 0.91,
        "basis": "The option fits the existing shared Runtime specification pattern.",
        "limitations": []
      }
    },
    {
      "option_id": "option_add_decision_schema_now",
      "name": "Add dedicated decision schema immediately",
      "mandatory_criteria_met": false,
      "criterion_scores": {
        "criterion_governance_alignment": 0.55,
        "criterion_traceability": 0.7,
        "criterion_operational_simplicity": 0.35,
        "criterion_reversibility": 0.55,
        "criterion_business_value": 0.5
      },
      "weighted_score": 0.54,
      "risk_level": "medium",
      "confidence": {
        "level": "medium",
        "score": 0.62,
        "basis": "A schema may be useful later but current source files define decisions inside existing Runtime Objects.",
        "limitations": [
          "Future operational evidence may justify a dedicated schema."
        ]
      }
    }
  ],
  "selected_option_id": "option_create_shared_spec",
  "selection_rationale": "The selected option improves Runtime consistency without changing schema architecture prematurely."
}
```

### 47.3 Approval Decision

This is a complete Approval Decision owned by authorised human authority. It is distinct from the immutable Approval Record that may reference it for audit and replay. `origin_state` and `resume_target.state` preserve the state that entered `approval_pending`; they do not propose a direct transition from `approval_pending` to `planning_running`. The Orchestrator first commits the registered static transition `approval_pending → approval_granted`, then validates `approval_resume_target` from `approval_granted` under the State Model.

```json
{
  "decision_id": "decision_550e8400-e29b-41d4-a716-446655440020",
  "decision_category": "approval_decision",
  "decision_type": "approve",
  "decision_version": 1,
  "approval_id": "approval_550e8400-e29b-41d4-a716-446655440021",
  "task_id": "task_550e8400-e29b-41d4-a716-446655440001",
  "execution_id": "exec_550e8400-e29b-41d4-a716-446655440002",
  "created_at": "2026-07-28T12:06:00Z",
  "created_by": {
    "actor_type": "human",
    "actor_id": "founder",
    "display_name": "Founder",
    "role": "runtime_owner",
    "version": null
  },
  "status": "approved",
  "scope": "non_destructive_repository_spec_creation",
  "owner": "authorised_human_authority",
  "authority_level": "human_approval",
  "related_object_ids": [
    "task_550e8400-e29b-41d4-a716-446655440001",
    "blueprint_550e8400-e29b-41d4-a716-446655440022"
  ],
  "current_state": "approval_pending",
  "dynamic_target_mechanism": "approval_resume_target",
  "origin_state": "planning_running",
  "decision_basis": "The authorised request permits the scoped specification change subject to the recorded conditions.",
  "selected_outcome": "approve",
  "alternatives_considered": [
    "approve",
    "reject",
    "revision_required"
  ],
  "policy_refs": [
    "policy_governance_before_autonomy"
  ],
  "assumptions": [],
  "reason_summary": "Approve the scoped repository specification change with explicit conditions.",
  "review_requirement": {
    "required": false,
    "reason": null
  },
  "required_role": "runtime_owner",
  "requested_at": "2026-07-28T12:05:00Z",
  "decided_at": "2026-07-28T12:06:00Z",
  "decided_by": {
    "actor_type": "human",
    "actor_id": "founder",
    "display_name": "Founder",
    "role": "runtime_owner",
    "version": null
  },
  "conditions": [
    "Do not modify unrelated files.",
    "Validate all fenced JSON blocks with Python json.loads."
  ],
  "expires_at": null,
  "evidence_refs": [
    "evidence_user_task_instruction"
  ],
  "confidence": {
    "level": "high",
    "score": 0.98,
    "basis": "The approval was explicitly issued by the required human authority for the identified scope.",
    "limitations": [
      "Approval applies only to the stated repository change."
    ]
  },
  "risk": {
    "level": "low",
    "summary": "The approved action is limited to a non-destructive specification update.",
    "risk_categories": [
      "runtime",
      "governance"
    ],
    "mitigations": [
      "Enforce the recorded scope and validation conditions."
    ],
    "residual_risk_level": "low"
  },
  "approval": {
    "required": true,
    "status": "approved",
    "approval_id": "approval_550e8400-e29b-41d4-a716-446655440021",
    "required_role": "runtime_owner",
    "decided_by": "founder",
    "decided_at": "2026-07-28T12:06:00Z",
    "conditions": [
      "Do not modify unrelated files.",
      "Validate all fenced JSON blocks with Python json.loads."
    ]
  },
  "validation": {
    "status": "valid",
    "validated_at": "2026-07-28T12:06:00Z",
    "validator": "approval_engine.governance_validation",
    "errors": [],
    "warnings": []
  },
  "resume_target": {
    "state": "planning_running",
    "context_revalidation_required": true
  }
}
```

### 47.4 Complete Retry Decision with Recovery Instruction

This is a complete Retry Decision. Its `recovery` field contains the specialised recovery instruction governed by Section 29; the recovery instruction is not a second standalone Decision. The Decision is evaluated after the registered `exception_detected → exception_classifying` transition and selects the immediate static target `retry_pending`. The Orchestrator may then commit `retry_pending → retrying`; only from `retrying` may it validate the registered `retry_target` authorised by this Decision. This example does not represent a direct transition from `exception_detected` to `retrying`.

```json
{
  "decision_id": "decision_550e8400-e29b-41d4-a716-446655440030",
  "decision_category": "retry_decision",
  "decision_type": "retry",
  "decision_version": 1,
  "task_id": "task_550e8400-e29b-41d4-a716-446655440001",
  "execution_id": "exec_550e8400-e29b-41d4-a716-446655440002",
  "created_at": "2026-07-28T12:08:00Z",
  "created_by": {
    "actor_type": "engine",
    "actor_id": "exception_engine",
    "display_name": "Exception Engine",
    "role": "exception_handling",
    "version": "1.0.0"
  },
  "status": "completed",
  "scope": "execution_attempt",
  "owner": "exception_engine",
  "authority_level": "engine_recommendation",
  "related_object_ids": [
    "exception_550e8400-e29b-41d4-a716-446655440031"
  ],
  "current_state": "exception_classifying",
  "decision_basis": "The validation failure is recoverable, bounded retry capacity remains, and the operation is protected by an idempotency key.",
  "selected_outcome": "retry_with_modified_context",
  "alternatives_considered": [
    "retry_with_modified_context",
    "escalate",
    "abort"
  ],
  "policy_refs": [
    "policy_bounded_retry"
  ],
  "assumptions": [],
  "reason_summary": "Retry once with corrected Context while preserving execution identity and enforcing the attempt limit.",
  "review_requirement": {
    "required": false,
    "reason": "Human review is required only if the escalation threshold is reached."
  },
  "exception_id": "exception_550e8400-e29b-41d4-a716-446655440031",
  "failure_category": "validation",
  "dynamic_target_mechanism": "retry_target",
  "origin_state": "business_judgement_running",
  "retry_target": "business_judgement_running",
  "current_attempt": 1,
  "maximum_attempts": 3,
  "retry_eligible": true,
  "delay_seconds": 0,
  "backoff_strategy": "none",
  "idempotency": {
    "required": true,
    "idempotency_key": "idem_decision_model_json_validation_exec_001",
    "side_effect_risk": "low"
  },
  "context_changes_allowed": [
    "correct_invalid_json_block",
    "append_validation_certificate"
  ],
  "resource_budget": {
    "maximum_additional_attempts": 2,
    "maximum_additional_seconds": 300
  },
  "escalation_threshold": "validation_failed_after_maximum_attempts",
  "evidence_refs": [
    "exception_550e8400-e29b-41d4-a716-446655440031"
  ],
  "confidence": {
    "level": "high",
    "score": 0.9,
    "basis": "The failure is deterministic, locally correctable, and has not exhausted the retry limit.",
    "limitations": [
      "A repeated validation failure requires escalation."
    ]
  },
  "risk": {
    "level": "low",
    "summary": "Retry is bounded and does not repeat an unprotected external side effect.",
    "risk_categories": [
      "reliability",
      "runtime"
    ],
    "mitigations": [
      "Use the idempotency key and stop after the configured maximum attempts."
    ],
    "residual_risk_level": "low"
  },
  "approval": {
    "required": false,
    "status": "not_required",
    "required_role": null,
    "conditions": []
  },
  "validation": {
    "status": "valid",
    "validated_at": "2026-07-28T12:08:00Z",
    "validator": "exception_engine.retry_validation",
    "errors": [],
    "warnings": []
  },
  "recovery": {
    "failure_origin": "exception_550e8400-e29b-41d4-a716-446655440031",
    "recovery_strategy": "retry_with_modified_context",
    "changed_dependencies": [
      "corrected_decision_model_source"
    ],
    "compatibility_impact": "none",
    "security_impact": "none",
    "privacy_impact": "none",
    "data_integrity_impact": "output_requires_revalidation",
    "recovery_result": "pending_retry",
    "next_state": "retry_pending"
  }
}
```

### 47.5 State Transition Decision

This is a complete State Transition Decision owned and committed by the Runtime Orchestrator. The `from_state`, `to_state`, guards, and commit metadata are specialised transition fields within the complete Decision.

```json
{
  "decision_id": "decision_550e8400-e29b-41d4-a716-446655440040",
  "decision_category": "state_transition_decision",
  "decision_type": "commit_transition",
  "decision_version": 1,
  "task_id": "task_550e8400-e29b-41d4-a716-446655440001",
  "execution_id": "exec_550e8400-e29b-41d4-a716-446655440002",
  "created_at": "2026-07-28T12:10:00Z",
  "created_by": {
    "actor_type": "runtime",
    "actor_id": "runtime_orchestrator",
    "display_name": "Runtime Orchestrator",
    "role": "orchestration",
    "version": "1.0.0"
  },
  "status": "completed",
  "scope": "execution_state_transition",
  "owner": "runtime_orchestrator",
  "authority_level": "orchestrator_commit",
  "related_object_ids": [
    "judgement_550e8400-e29b-41d4-a716-446655440041",
    "policy_decision_550e8400-e29b-41d4-a716-446655440042"
  ],
  "current_state": "business_judgement_ready",
  "decision_basis": "The Business Judgement is present and all policy, approval, and transition guards passed.",
  "selected_outcome": "commit_transition",
  "alternatives_considered": [
    "commit_transition",
    "reject_transition"
  ],
  "policy_refs": [
    "policy_decision_550e8400-e29b-41d4-a716-446655440042"
  ],
  "assumptions": [],
  "reason_summary": "Commit the registered transition because source state, target state, authority, and guards are valid.",
  "review_requirement": {
    "required": false,
    "reason": null
  },
  "expected_state_version": 8,
  "from_state": "business_judgement_ready",
  "to_state": "planning_pending",
  "proposed_by": {
    "actor_type": "engine",
    "actor_id": "business_judgement_engine",
    "display_name": "Business Judgement Engine",
    "role": "reasoning",
    "version": "1.0.0"
  },
  "committed_by": {
    "actor_type": "runtime",
    "actor_id": "runtime_orchestrator",
    "display_name": "Runtime Orchestrator",
    "role": "orchestration",
    "version": "1.0.0"
  },
  "reason_code": "judgement_completed",
  "evidence_refs": [
    "judgement_550e8400-e29b-41d4-a716-446655440041",
    "policy_decision_550e8400-e29b-41d4-a716-446655440042"
  ],
  "guards": [
    {
      "guard_id": "business_judgement_present",
      "status": "passed",
      "evidence_ref": "judgement_550e8400-e29b-41d4-a716-446655440041"
    },
    {
      "guard_id": "policy_gate",
      "status": "passed",
      "evidence_ref": "policy_decision_550e8400-e29b-41d4-a716-446655440042"
    },
    {
      "guard_id": "approval_gate",
      "status": "passed",
      "evidence_ref": null
    }
  ],
  "confidence": {
    "level": "high",
    "score": 0.97,
    "basis": "The source state, target state, authority, and mandatory guards were validated.",
    "limitations": []
  },
  "risk": {
    "level": "low",
    "summary": "The transition advances to the registered planning state without an external side effect.",
    "risk_categories": [
      "state_integrity"
    ],
    "mitigations": [
      "Use optimistic state-version validation before commit."
    ],
    "residual_risk_level": "low"
  },
  "approval": {
    "required": false,
    "status": "not_required",
    "required_role": null,
    "conditions": []
  },
  "validation": {
    "status": "valid",
    "validated_at": "2026-07-28T12:10:00Z",
    "validator": "runtime_orchestrator.transition_validation",
    "errors": [],
    "warnings": []
  },
  "validation_status": "passed",
  "commit_status": "committed",
  "committed_at": "2026-07-28T12:10:00Z",
  "new_state_version": 9
}
```

### 47.6 Decision Snapshot

This is a `snapshot_or_history_object`, not a standalone Decision. Its source Decision supplies the universal Decision fields; the snapshot preserves only the point-in-time fields required by Section 44.

```json
{
  "snapshot_id": "decision_snapshot_550e8400-e29b-41d4-a716-446655440050",
  "decision_id": "decision_550e8400-e29b-41d4-a716-446655440000",
  "decision_version": 1,
  "created_at": "2026-07-28T12:12:00Z",
  "context_id": "ctx_exec_001_business_judgement_003",
  "engine": {
    "engine_id": "business_judgement_engine",
    "engine_version": "1.0.0",
    "contract_version": "1.0.0"
  },
  "execution": {
    "execution_id": "exec_550e8400-e29b-41d4-a716-446655440002",
    "task_id": "task_550e8400-e29b-41d4-a716-446655440001",
    "trace_id": "trace_550e8400-e29b-41d4-a716-446655440052"
  },
  "related_runtime_objects": [
    "kp_550e8400-e29b-41d4-a716-446655440003",
    "judgement_550e8400-e29b-41d4-a716-446655440041"
  ],
  "selected_outcome": "proceed_with_conditions",
  "confidence": {
    "level": "high",
    "score": 0.9,
    "basis": "Supported by current Runtime specifications.",
    "limitations": []
  },
  "risk": {
    "level": "low",
    "summary": "No schema or state architecture changes are introduced.",
    "risk_categories": [
      "runtime"
    ],
    "mitigations": [
      "Keep Decision as structured payload in existing Runtime Objects."
    ],
    "residual_risk_level": "low"
  },
  "approval_status": "not_required",
  "freshness_status": "current",
  "integrity": {
    "algorithm": "sha256",
    "checksum": "sha256:example",
    "status": "verified"
  },
  "dependency_versions": {
    "engine_contract": "1.0.0",
    "engine_context": "1.0.0",
    "state_model": "1.0.0"
  }
}
```

---

## 48. Enterprise Examples

### 48.1 Xiaohongshu Content Topic Selection

For a Xiaohongshu Content Package workflow, the Business Judgement Engine MUST select a topic only after the Retrieval Engine provides approved content memory, relevant Brand knowledge, Product knowledge where applicable, Content Operations rules, and previous content coverage. The Decision MUST identify alternatives, duplication risk, source support, audience fit, confidence, and approval requirements. It MUST NOT invent customer performance data or mark content as published.

### 48.2 Product Documentation Change

For product documentation work, a Decision that would alter a Product Passport MUST classify the action as approval-required. The Engine MAY draft a recommendation, but MUST NOT commit a product specification change without human approval.

### 48.3 Runtime Recovery

If JSON validation fails for an output artifact, the Exception Engine SHOULD create or request an Exception Record, classify the failure as validation-related, determine retry eligibility, return to the responsible upstream stage, and preserve the failed validation evidence for learning.

### 48.4 Memory Admission

The Memory Engine MAY admit validated learning into a Memory Record only when evidence, quality, authority, privacy, lifecycle, and governance requirements pass. A Learning Candidate is not approved memory until the Memory Decision permits admission.

---

## 49. Acceptance Criteria

A production implementation of the Decision Model is acceptable only when:

### Identity and Traceability

- [ ] Every material Decision has a stable Decision ID.
- [ ] Every material Decision references Task and execution identity.
- [ ] Every material Decision identifies creator, owner, scope, status, and authority.
- [ ] Decision history is append-only.

### Evidence and Reasoning

- [ ] Decisions reference evidence where evidence exists.
- [ ] Assumptions are explicit.
- [ ] Unknowns remain visible.
- [ ] Alternatives are evaluated where material.
- [ ] Confidence matches evidence quality.

### Governance

- [ ] Policy evaluation occurs before governed action.
- [ ] Approval requirements are explicit.
- [ ] Human review and override are supported.
- [ ] Security and privacy controls cannot be bypassed.
- [ ] Higher-authority sources override lower-authority sources.

### State and Engine Integration

- [ ] Engine Results include structured Decision summaries.
- [ ] State Transition Decisions follow the State Model.
- [ ] Only the Runtime Orchestrator commits global Runtime State.
- [ ] Engine Context is the only authorised input envelope.

### Recovery and Audit

- [ ] Retry decisions are bounded.
- [ ] Recovery decisions define the return path.
- [ ] Rollback decisions preserve history.
- [ ] Escalation decisions include required authority and resume target.
- [ ] Snapshots support replay and audit.

### Validation

- [ ] Structural validation passes.
- [ ] Semantic validation passes.
- [ ] Governance validation passes.
- [ ] Cross-object validation passes.
- [ ] Every complete Decision example contains all universal required fields.
- [ ] Every specialised payload names its containing Decision field and inherited universal fields.
- [ ] No snapshot, history object, validation result, or explanatory fragment is presented as a complete Decision.
- [ ] JSON examples in this specification parse successfully.

---

## 50. Prohibited Behaviours

The following behaviours are prohibited:

- making a material Decision without Engine Context;
- representing a material Decision only as prose;
- using hidden prompt memory as Decision authority;
- inventing missing authoritative evidence;
- hiding assumptions;
- hiding unknowns;
- skipping alternatives when alternatives are material;
- selecting an outcome that violates mandatory constraints;
- treating confidence as approval;
- treating a score as approval;
- bypassing Policy Engine or Approval Engine outcomes;
- committing global Runtime State outside the Runtime Orchestrator;
- silently resolving controlling policy conflicts;
- lowering a blocking issue to a warning without authority;
- retrying indefinitely;
- rolling back by deleting history;
- marking draft content as approved or published without approval;
- modifying approved Business Knowledge without governance;
- using stale, expired, revoked, corrupt, or mismatched context for material execution;
- treating logs as authoritative Decision Records.

---

## 51. Summary

The Decision Model is the shared decision framework for Scout Runtime.

It defines how Runtime decisions are:

- triggered;
- contextualised;
- evidenced;
- constrained;
- compared;
- scored;
- confidence-assessed;
- risk-assessed;
- approved or blocked;
- applied to Runtime State;
- recorded;
- validated;
- replayed;
- audited;
- learned from.

Engines make bounded Decisions inside their authority.

The Runtime Orchestrator validates and commits global state.

The Approval Engine and Policy Engine govern approval and policy outcomes.

Human authority remains final.

No material Runtime Decision exists unless it is explicit, traceable, validated, governed, and observable.

---

## JSON Validation Certificate

All fenced `json` code blocks in this document were parsed successfully with Python's standard `json` parser.

```json
{
  "document": "decision_model.md",
  "document_version": "1.0.0",
  "validation_timestamp": "2026-07-28T12:42:15Z",
  "validator": "Python json.loads",
  "json_blocks_found": 7,
  "json_blocks_passed": 7,
  "overall_status": "passed"
}
```
