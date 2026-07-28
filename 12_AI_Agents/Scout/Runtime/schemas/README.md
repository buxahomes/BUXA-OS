# Scout Runtime Schema System

## Purpose

The Scout Runtime Schema System defines the structured data contracts used by Runtime components.

Schemas establish how information is created, exchanged, validated, stored and traced across the execution lifecycle.

The purpose of the Schema System is to ensure that Runtime components communicate through consistent, machine-readable and governable objects rather than uncontrolled free-form text.

Schemas make Scout Runtime:

- structured
- interoperable
- traceable
- testable
- auditable
- versionable
- reliable

---

# System Position

The Schema System operates between Runtime components.

```text
Input Engine
    │
    ▼
Task
    │
    ▼
Retrieval Engine
    │
    ▼
Knowledge Package
    │
    ▼
Reasoning Engine
    │
    ▼
Business Judgement
    │
    ▼
Planning Engine
    │
    ▼
Execution Blueprint
    │
    ▼
Skills and Tools
    │
    ▼
Skill Output
    │
    ▼
Quality Engine
    │
    ▼
Quality Report
    │
    ▼
Execution Result
    │
    ▼
Feedback and Learning
    │
    ▼
Learning Candidate
    │
    ▼
Memory Engine
    │
    ▼
Memory Record
```

Schemas define the contracts between these stages.

Every Engine invocation returns an Engine Result envelope around its invocation outcome. The lifecycle diagram shows the Runtime Objects carried or referenced by those envelopes rather than repeating Engine Result between every stage.

They do not define business policy.

Business authority remains with:

```text
Business Operating System
        │
        ▼
Operating Manual
        │
        ▼
Runtime Configuration
        │
        ▼
Schemas and Runtime Components
```

---

# Schema Mission

The mission of the Schema System is to transform Scout Runtime information into reliable operational objects.

Every important Runtime object should clearly express:

- what it represents
- where it originated
- which task it belongs to
- which evidence supports it
- its current status
- its confidence level
- who or what created it
- whether it has been validated
- how it relates to other objects

A valid object should be understandable by both humans and machines.

---

# Schema Directory

The initial Schema System contains the following files.

```text
Runtime/
└── schemas/
    ├── README.md
    ├── common.schema.json
    ├── runtime_object_registry.schema.json
    ├── engine_result.schema.json
    ├── task.schema.json
    ├── knowledge_package.schema.json
    ├── business_judgement.schema.json
    ├── execution_blueprint.schema.json
    ├── skill_output.schema.json
    ├── quality_report.schema.json
    ├── execution_result.schema.json
    ├── learning_candidate.schema.json
    ├── memory_record.schema.json
    └── exception_record.schema.json
```

Future schemas may include:

```text
approval_request.schema.json
feedback_record.schema.json
tool_request.schema.json
tool_result.schema.json
workflow_definition.schema.json
execution_record.schema.json
```

New schemas should only be introduced when a stable operational need has been demonstrated.

---

# Runtime Object Registry Schema

`runtime_object_registry.schema.json` validates the complete machine-readable Runtime Object Registry stored at `../registry/runtime_objects.json`.

Authority is separated as follows:

- `../engines/shared/runtime_objects.md` is the normative governance authority;
- `../registry/runtime_objects.json` is registry data and is not a JSON Schema;
- `runtime_object_registry.schema.json` is the machine validation authority for that data.

The registry schema validates structure and controlled values. Additional semantic validation enforces unique object types and canonical names, existing owner and schema paths, resolved object relationships, recognised authorities, and metadata counts.

This architectural validation schema is not a Runtime Object schema and is therefore outside the lifecycle-based Initial Development Order.

---

# Core Runtime Objects

## Engine Result

Represents the universal response envelope returned by every Runtime Engine invocation.

An Engine Result contains:

- Engine, Task, execution and invocation identity
- Engine invocation status
- canonical `primary_output`
- secondary outputs
- Decision reference and top-level confidence
- warnings and blocking issues
- events, metrics and timing
- State Transition proposal or reference
- side effects, approval, recovery and failure information
- validation and extensions

`engine_result.schema.json` is the machine-readable authority for this envelope. Engine invocation status is not Runtime State.

The mandatory Engine Result `status` field reports one final invocation disposition:

```text
waiting
succeeded
partial_success
failed
blocked
cancelled
timeout
```

All seven values are final for the returned `invocation_id`. Workflow resumability is governed separately through recovery information, Decisions, Runtime Events, and State Model-authorised transition proposals.

`ready` and `running` are lifecycle phases, not Engine Result statuses. `success` is an invalid alias for `succeeded`, and Runtime State `timed_out` MUST NOT be used as Engine Result status.

`state_transition` accepts a strict State Transition proposal, a transition reference, or `null`. A proposal records source and target states, triggering Engine and Engine Result, proposer, time, reason, validation outcome, approval requirement, and rollback state. The proposal does not commit Runtime State; only the Runtime Orchestrator may validate and commit it under `../engines/shared/state_model.md`.

Engine Result and Execution Result are distinct. An Engine Result may reference an Execution Result through `primary_output.object_ref` or carry a schema-valid Execution Result through an embedded `primary_output.value`; an Execution Result does not replace the Engine Result envelope.

---

## Task

Represents a structured business request accepted by Scout Runtime.

A Task defines:

- objective
- scope
- constraints
- expected deliverables
- success criteria
- authority
- risk
- approval requirements

The Task is the root object of an execution lifecycle.

---

## Knowledge Package

Represents verified and task-specific organisational context assembled by the Retrieval Engine.

A Knowledge Package contains:

- knowledge requirements
- retrieved sources
- evidence
- standards
- relevant memory
- identified gaps
- conflicts
- retrieval confidence

A Knowledge Package is not an unfiltered collection of documents.

---

## Business Judgement

Represents the structured conclusion formed by the Reasoning Engine.

A Business Judgement contains:

- problem interpretation
- evidence analysis
- assumptions
- alternatives
- risks
- trade-offs
- recommendation
- confidence
- unresolved questions

A Business Judgement is not merely a summary.

---

## Execution Blueprint

Represents the actionable plan produced by the Planning Engine.

An Execution Blueprint defines:

- objectives
- activities
- sequence
- dependencies
- Skills
- Tools
- quality gates
- approval gates
- deliverables
- success criteria

An Execution Blueprint connects judgement to execution.

---

## Skill Output

Represents the structured result returned by a Skill.

A Skill Output contains:

- requested capability
- completed work
- generated assets
- evidence references
- limitations
- status
- confidence
- validation information

A Skill Output is an intermediate execution object and may require quality review before delivery.

---

## Quality Report

Represents the result of a formal Runtime quality review.

A Quality Report records:

- object reviewed
- standards applied
- checks performed
- findings
- severity
- required revisions
- approval decision
- reviewer
- evidence

A Quality Report may approve, conditionally approve, reject or return an object for revision.

---

## Execution Result

Represents the final Runtime Object produced by the Execution Engine for an execution cycle or execution step.

An Execution Result contains:

- completed deliverables
- execution status
- business outcome
- approvals
- quality status
- exceptions
- limitations
- follow-up actions

An Execution Result records what was actually completed rather than what was planned. It is governed by `execution_result.schema.json` and is not the universal invocation envelope governed by `engine_result.schema.json`.

---

## Learning Candidate

Represents a potential organisational lesson identified from execution experience.

A Learning Candidate contains:

- observed experience
- pattern or insight
- supporting evidence
- proposed learning
- applicability
- confidence
- validation requirement
- proposed memory destination

A Learning Candidate is not yet approved organisational memory.

---

## Memory Record

Represents validated and reusable organisational learning stored by the Memory Engine.

A Memory Record contains:

- validated knowledge
- evidence
- scope
- applicability
- source executions
- approval status
- review date
- supersession information

Only validated learning may become a Memory Record.

---

## Exception Record

Represents an abnormal condition that interrupts, redirects or limits normal Runtime execution.

An Exception Record contains:

- exception category
- affected component
- cause
- impact
- severity
- recovery action
- escalation
- resolution
- learning potential

Exceptions must be explicitly recorded rather than silently bypassed.

---

# Common Object Model

All major Runtime objects should use a shared structural model where applicable.

```text
Identity
Context
Content
Governance
Evidence
Status
Lifecycle
Relationships
```

Not every object requires every common field.

Fields should only be included where they support a meaningful operational purpose.

---

# Common Metadata

Common metadata may include:

| Field | Purpose |
|---|---|
| `id` | Unique identifier of the object |
| `schema_name` | Name of the schema used |
| `schema_version` | Version of the schema |
| `task_id` | Related Task identifier |
| `execution_id` | Related execution lifecycle |
| `workflow_id` | Related Workflow |
| `parent_id` | Direct parent object |
| `created_at` | Object creation timestamp |
| `updated_at` | Most recent update timestamp |
| `created_by` | Human, Engine, Skill or Tool that created the object |
| `status` | Current lifecycle state |
| `confidence` | Confidence assessment |
| `source_refs` | References to source material |
| `evidence_refs` | References to supporting evidence |
| `tags` | Controlled categorisation |
| `metadata` | Limited extensible metadata |

Common fields should be defined centrally in `common.schema.json`.

Individual schemas should reuse common definitions through JSON Schema references.

---

# Identifier Standards

Every major Runtime object must have a unique identifier.

Identifiers should be:

- globally unique within the Runtime
- immutable
- machine-readable
- non-semantic where possible
- safe for logs and file systems

Recommended format:

```text
<object-prefix>_<uuid>
```

Examples:

```text
task_550e8400-e29b-41d4-a716-446655440000
kp_550e8400-e29b-41d4-a716-446655440000
judgement_550e8400-e29b-41d4-a716-446655440000
blueprint_550e8400-e29b-41d4-a716-446655440000
skillout_550e8400-e29b-41d4-a716-446655440000
quality_550e8400-e29b-41d4-a716-446655440000
result_550e8400-e29b-41d4-a716-446655440000
learning_550e8400-e29b-41d4-a716-446655440000
memory_550e8400-e29b-41d4-a716-446655440000
exception_550e8400-e29b-41d4-a716-446655440000
```

Human-readable titles should be stored separately from identifiers.

Identifiers must not change when titles or content change.

---

# Execution Relationships

All objects created during a Runtime cycle should be traceable to an execution.

```text
Task
└── execution_id
    ├── Knowledge Package
    ├── Business Judgement
    ├── Execution Blueprint
    ├── Skill Outputs
    ├── Quality Reports
    ├── Execution Result
    ├── Exception Records
    └── Learning Candidates
```

A single Task may have multiple executions.

Examples include:

- initial execution
- retry
- revision
- alternative plan
- scheduled recurrence
- human-approved continuation

The `task_id` identifies the business request.

The `execution_id` identifies one attempt or execution cycle.

---

# Date and Time Standards

All timestamps must use ISO 8601 format.

Preferred format:

```text
YYYY-MM-DDTHH:MM:SSZ
```

Example:

```text
2026-07-27T09:30:00Z
```

Runtime storage should use UTC wherever practical.

User-facing systems may convert timestamps to local time.

Dates without times should use:

```text
YYYY-MM-DD
```

Relative time expressions such as the following must not be stored as canonical timestamps:

```text
tomorrow
next week
recently
later
```

They should be resolved into explicit dates or recorded as unresolved input.

---

# Naming Conventions

## Schema Files

Schema file names use lowercase snake case.

Examples:

```text
task.schema.json
knowledge_package.schema.json
execution_blueprint.schema.json
```

## Properties

JSON property names use lowercase snake case.

Examples:

```json
{
  "task_id": "...",
  "success_criteria": [],
  "approval_required": false
}
```

## Schema Titles

Human-readable schema titles use title case.

Examples:

```text
Task
Knowledge Package
Business Judgement
```

## Enumerations

Enumeration values use lowercase snake case.

Examples:

```json
[
  "draft",
  "in_progress",
  "clarification_required",
  "completed"
]
```

---

# Schema Versioning

Every schema must have an explicit version.

Initial schema version:

```text
1.0.0
```

Schema versions follow semantic versioning.

```text
MAJOR.MINOR.PATCH
```

## Major Version

Increase the major version when a change is not backward compatible.

Examples:

- removing a required field
- changing a field type
- changing the meaning of an existing field
- restructuring a core object

## Minor Version

Increase the minor version when adding backward-compatible capability.

Examples:

- adding an optional field
- adding a new enum value where safely supported
- adding a reusable definition

## Patch Version

Increase the patch version for corrections that do not change the data contract.

Examples:

- description improvements
- examples
- spelling corrections
- validation message clarification

Schemas should not be edited without updating their version where the data contract changes.

---

# Object Lifecycle

Runtime objects may move through defined lifecycle states.

A generic status model may include:

```text
draft
pending
in_progress
validation_required
approval_required
approved
completed
failed
cancelled
superseded
archived
```

Individual schemas should define only the statuses relevant to that object.

Status transitions should be explicit.

Example:

```text
draft
  ↓
in_progress
  ↓
validation_required
  ↓
approved
  ↓
completed
```

Invalid transitions should be rejected where practical.

Example:

```text
cancelled
  ↛
completed
```

A cancelled object may require a new version or new execution before work continues.

---

# Confidence Model

Where confidence is meaningful, objects should use a structured confidence model.

Recommended representation:

```json
{
  "level": "high",
  "score": 0.9,
  "basis": "Supported by approved Product Passport and current Product Standards."
}
```

Recommended confidence levels:

```text
very_low
low
medium
high
very_high
```

A confidence score, when used, must be between:

```text
0.0 and 1.0
```

Confidence is not a substitute for evidence.

High confidence without supporting evidence should not be accepted as authoritative.

---

# Evidence Model

Claims, recommendations and decisions should reference supporting evidence where practical.

Evidence references may identify:

- BOS documents
- approved Repository files
- Product Passports
- organisational memory
- previous approved outputs
- execution records
- external authoritative sources
- tool results
- human instructions

An evidence reference should include enough information to locate or identify the source.

Example:

```json
{
  "evidence_id": "evidence_001",
  "source_type": "repository_document",
  "source_location": "02_Products/01_Flooring/HD03_Product_Passport.md",
  "title": "HD03 Product Passport",
  "version": "1.2",
  "retrieved_at": "2026-07-27T09:30:00Z",
  "authority_level": "approved_internal"
}
```

Evidence should not be copied unnecessarily into every object.

Objects should reference evidence records where practical.

---

# Source Authority

Sources should be classified by authority.

Recommended authority levels:

```text
governance
approved_internal
validated_memory
approved_previous_output
authoritative_external
general_external
unverified
```

Priority should normally follow:

```text
governance
        ↓
approved_internal
        ↓
validated_memory
        ↓
approved_previous_output
        ↓
authoritative_external
        ↓
general_external
        ↓
unverified
```

A lower-authority source must not silently override a higher-authority source.

Conflicts should generate an explicit conflict record or Exception Record.

---

# Validation Levels

Schema validation should occur at multiple levels.

## Structural Validation

Checks whether the object conforms to its JSON Schema.

Examples:

- required fields exist
- data types are correct
- enum values are valid
- string formats are correct
- arrays meet minimum requirements

## Semantic Validation

Checks whether the object makes business sense.

Examples:

- objective is understandable
- success criteria relate to the objective
- evidence supports the recommendation
- dates are logically consistent
- dependencies exist
- confidence matches evidence quality

## Governance Validation

Checks whether the object complies with organisational authority.

Examples:

- approval requirements are correct
- prohibited actions are absent
- authoritative knowledge is respected
- permissions are sufficient
- irreversible actions are controlled

## Cross-Object Validation

Checks consistency between related objects.

Examples:

- Execution Blueprint addresses the Task objective
- Business Judgement uses the supplied Knowledge Package
- Skill Output matches the Skill Request
- Quality Report references the correct object
- Execution Result reflects actual Skill Outputs
- Memory Record originates from validated learning

JSON Schema primarily supports structural validation.

Semantic, governance and cross-object validation require Runtime validators.

---

# Required and Optional Fields

Fields should be required only when their absence makes the object invalid or unsafe.

A field should normally be required when it is necessary for:

- identity
- traceability
- execution
- validation
- governance
- interpretation

Optional fields should not be used as an excuse for unclear object design.

Where information is unknown, the schema should distinguish between:

```text
not provided
not applicable
unknown
awaiting clarification
```

These states should not be represented indiscriminately by empty strings.

---

# Null and Empty Values

The Runtime should avoid ambiguous empty values.

Preferred rules:

- use `null` only when the schema explicitly permits an unknown or absent value
- use an empty array only when no items exist and the empty state is valid
- do not use empty strings as placeholders
- do not omit required fields
- distinguish `unknown` from `not_applicable` where operationally important

Example:

```json
{
  "approval_status": "not_required",
  "approved_by": null
}
```

This is clearer than:

```json
{
  "approval_status": "",
  "approved_by": ""
}
```

---

# Extensibility

Schemas may include a controlled `metadata` object for non-core extensions.

Example:

```json
{
  "metadata": {
    "campaign_name": "HD03 Xiaohongshu July",
    "channel": "xiaohongshu"
  }
}
```

Core operational fields must not be hidden inside `metadata`.

The following should remain explicit when relevant:

- status
- approval
- confidence
- evidence
- risks
- exceptions
- ownership
- lifecycle relationships

Extension fields should not conflict with defined schema properties.

---

# Immutability and Revision

Some fields should be immutable after object creation.

Typical immutable fields include:

- `id`
- `schema_name`
- `created_at`
- original `task_id`
- original `execution_id`

Objects that require material revision should use one of two approaches.

## In-Place Revision

Used for non-material corrections before approval.

The object retains its identifier and updates:

```text
updated_at
revision_number
revision_notes
```

## New Version

Used when approved or materially significant content changes.

The new object should include:

```text
supersedes_id
version
change_summary
```

Approved organisational records should not be silently overwritten.

---

# Approval Model

Objects requiring human or system approval should use an explicit approval structure.

Example:

```json
{
  "approval": {
    "required": true,
    "status": "pending",
    "required_role": "brand_owner",
    "approved_by": null,
    "approved_at": null,
    "notes": null
  }
}
```

Recommended approval statuses:

```text
not_required
pending
approved
rejected
revision_required
expired
```

Approval must not be inferred from task completion.

---

# Quality Status

Objects subject to quality review should record quality status separately from execution status.

Recommended quality statuses:

```text
not_reviewed
review_pending
passed
passed_with_conditions
failed
revision_required
```

Example:

```text
Execution status: completed
Quality status: revision_required
```

This means the work was produced but is not approved for final delivery.

---

# Error and Exception Representation

Schema validation failures should be represented explicitly.

Example:

```json
{
  "validation_status": "failed",
  "validation_errors": [
    {
      "field": "objective",
      "code": "required_field_missing",
      "message": "Task objective is required."
    }
  ]
}
```

Operational exceptions should use `exception_record.schema.json`.

An exception should not be represented only as a free-form log message.

---

# Backward Compatibility

Schema evolution should preserve existing Runtime data wherever practical.

Compatibility rules include:

- existing valid objects should remain readable
- optional fields may be added without invalidating old data
- renamed fields require migration
- removed fields require a major version change
- changed enum values require compatibility review
- historical objects should retain their original schema version
- validators should identify unsupported versions clearly

Migration scripts should be created when schema changes require data transformation.

---

# Schema Validation

All Runtime objects should be validated:

1. when created
2. before being passed to another Engine
3. before approval
4. before persistence
5. after migration

Validation results should be recorded where operationally significant.

Example:

```text
Task created
        ↓
Task schema validation
        ↓
Task semantic validation
        ↓
Task accepted by Runtime
```

Invalid objects should not silently proceed through the Runtime.

---

# Schema Design Principles

## Meaning Before Structure

A schema should first define the business meaning of an object.

Fields should follow meaning rather than determine it.

## Minimum Necessary Structure

Schemas should contain enough structure to support reliable execution without creating unnecessary complexity.

## Explicit Over Implicit

Important status, authority, evidence and risk information should be represented explicitly.

## Traceability by Default

Important objects should be traceable to their sources, Task and execution.

## Evidence Before Confidence

Confidence should be supported by identifiable evidence.

## Governance by Design

Approval, authority and risk should be embedded in relevant schemas rather than added after execution.

## Stable Core, Extensible Edge

Core contracts should remain stable.

Business-specific extension should occur through controlled fields or specialised schemas.

## Human and Machine Readability

Objects should be understandable to developers, operators and authorised business users.

## Validation Before Transfer

An Engine should validate its output before passing it to the next Runtime component.

---

# Schema Development Process

Every new schema should follow this sequence.

```text
Define Semantic Purpose

↓

Define Object Boundary

↓

Define Required Fields

↓

Define Optional Fields

↓

Define Enumerations

↓

Define Validation Rules

↓

Define Relationships

↓

Create JSON Schema

↓

Create Valid Examples

↓

Create Invalid Examples

↓

Create Tests

↓

Approve Schema Version
```

A schema is not complete until it has:

- semantic documentation
- machine-readable definition
- valid examples
- invalid examples
- validation tests

---

# Schema Completion Standard

A Runtime schema is considered complete when:

- its purpose is unambiguous
- its boundary is defined
- required fields are justified
- property types are specified
- enumerations are controlled
- validation rules are documented
- relationships are traceable
- JSON Schema validation succeeds
- valid example objects are available
- invalid example objects are tested
- schema version is assigned
- ownership is identified

A schema file existing in the Repository does not by itself mean the schema is complete.

---

# Initial Development Order

The Schema System should be implemented in the following order.

```text
1. common.schema.json

2. engine_result.schema.json

3. task.schema.json

4. knowledge_package.schema.json

5. business_judgement.schema.json

6. execution_blueprint.schema.json

7. skill_output.schema.json

8. quality_report.schema.json

9. execution_result.schema.json

10. exception_record.schema.json

11. learning_candidate.schema.json

12. memory_record.schema.json
```

This order follows the Runtime lifecycle and reduces circular dependencies.

`exception_record.schema.json` is introduced before learning and memory because exceptions may occur at every execution stage.

---

# Initial Implementation Scope

Schema System v1 should support the first Scout Runtime Workflow:

```text
Xiaohongshu Content Package
```

The schemas must remain general enough to support future business workflows.

They should not encode Xiaohongshu-specific content directly into core objects.

Platform-specific data should be handled by:

- Skill-specific schemas
- Workflow-specific schemas
- controlled metadata
- deliverable schemas

Core schemas should describe universal Runtime behaviour.

---

# Success Criteria

The Schema System v1 is successful when:

- every core Runtime object has a defined schema
- objects can be validated automatically
- Engine interfaces use structured objects
- failed validation prevents unsafe progression
- objects remain traceable across an execution
- evidence and authority are visible
- approval and quality states are explicit
- schema versions are controlled
- the first Runtime Workflow can execute using these contracts

---

# Philosophy

Scout Runtime should not rely on hidden context or informal assumptions to connect its components.

Knowledge must become Context.

Context must become Judgement.

Judgement must become a Blueprint.

A Blueprint must become Execution.

Execution must become Learning.

Learning must become validated Memory.

The Schema System provides the contracts that make this transformation reliable.

Schemas are not administrative documentation.

They are the operational language of Scout Runtime.
