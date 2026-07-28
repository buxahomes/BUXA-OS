# Scout Runtime

## Purpose

Scout Runtime is the executable implementation of Scout's operating behaviour.

It translates the principles, workflows and governance defined by the Scout Operating Manual into a functioning AI-native business operating system.

The Runtime coordinates knowledge retrieval, reasoning, planning, skill invocation, quality verification, execution, feedback and memory.

Its purpose is not simply to generate responses.

Its purpose is to transform organisational knowledge into reliable business outcomes and continuous organisational learning.

---

# System Position

Scout is organised into three primary layers.

```text
Scout
│
├── Business Operating System
│   └── Defines organisational knowledge, standards and governance.
│
├── Operating Manual
│   └── Defines how Scout should behave.
│
└── Runtime
    └── Implements and executes that behaviour.
```

The relationship between these layers is:

```text
BOS
Defines the organisation
        │
        ▼
Operating Manual
Defines operational behaviour
        │
        ▼
Runtime
Implements operational behaviour
```

The Runtime must comply with both the Business Operating System and the Operating Manual.

---

# Runtime Mission

The mission of Scout Runtime is to provide a reliable execution environment in which Scout can:

- receive and structure business tasks
- retrieve approved organisational knowledge
- analyse evidence and form business judgement
- create executable plans
- invoke specialised business skills
- use technical tools
- verify outputs through quality gates
- execute approved activities
- capture outcomes and feedback
- improve organisational memory

Every successful Runtime cycle should create business value and improve future execution capability.

---

# Runtime Scope

Scout Runtime may support business capabilities including:

- market intelligence
- competitor intelligence
- product knowledge
- product documentation
- brand strategy
- content creation
- social media
- SEO
- website optimisation
- sales support
- customer intelligence
- PDMS
- repository management
- organisational knowledge management

New capabilities should normally be introduced as Skills rather than by changing the core Runtime architecture.

---

# Runtime Architecture

Scout Runtime consists of a coordinated set of operational components.

```text
Task
  │
  ▼
Input
  │
  ▼
Retrieval
  │
  ▼
Reasoning
  │
  ▼
Planning
  │
  ▼
Skill Orchestration
  │
  ▼
Quality Verification
  │
  ▼
Execution
  │
  ▼
Feedback
  │
  ▼
Memory
```

This process operates as two connected loops.

## Execution Loop

```text
Task
↓
Retrieval
↓
Reasoning
↓
Planning
↓
Skills
↓
Quality
↓
Execution
↓
Business Outcome
```

The Execution Loop creates business value.

## Learning Loop

```text
Business Outcome
↓
Experience
↓
Feedback
↓
Learning
↓
Validation
↓
Memory
↓
Better Future Retrieval
```

The Learning Loop increases organisational capability.

---

# Runtime Components

The Runtime is organised into the following primary directories.

```text
Runtime/
├── README.md
├── config/
├── core/
├── engines/
├── skills/
├── tools/
├── memory/
├── workflows/
├── schemas/
├── registry/
├── prompts/
├── quality/
├── logs/
├── tests/
└── scripts/
```

---

## Config

`config/` contains environment and Runtime configuration.

Examples include:

- model configuration
- tool availability
- permissions
- approval requirements
- environment settings
- execution limits

Configuration determines how the Runtime operates without redefining Scout's organisational principles.

The Business Operating System and Operating Manual remain the authoritative sources of organisational behaviour.

---

## Core

`core/` contains the fundamental Runtime objects and orchestration logic.

Core objects may include:

- Task
- Context
- State
- Result
- Execution Record
- Exception
- Approval Request

The Core layer should remain independent of individual business Skills.

---

## Engines

`engines/` contains the implementation of Scout's operational engines.

Expected engines include:

```text
engines/
├── input/
├── retrieval/
├── reasoning/
├── planning/
├── execution/
├── quality/
├── feedback/
└── memory/
```

Each Engine should have one primary responsibility.

| Engine | Primary Input | Primary Output |
|---|---|---|
| Input | Raw request or trigger | Structured Task |
| Retrieval | Structured Task | Knowledge Package |
| Reasoning | Knowledge Package | Business Judgement |
| Planning | Business Judgement | Execution Blueprint |
| Execution | Execution Blueprint | Execution Results |
| Quality | Engine or Skill Output | Quality Report |
| Feedback | Execution Results | Learning Candidates |
| Memory | Validated Learning | Memory Record |

Engines determine how work progresses through the Runtime.

They should not contain unnecessary business-specific logic.

---

## Skills

`skills/` contains reusable business capabilities.

Examples include:

```text
skills/
├── content/
├── market_research/
├── competitor_intelligence/
├── product/
├── website/
├── seo/
├── social_media/
├── sales/
├── pdms/
└── repository/
```

A Skill performs a specific business function.

Examples:

- create a Xiaohongshu content package
- analyse a competitor
- prepare a Product Passport
- review a website page
- generate an SEO recommendation
- prepare a PDMS inspection document

Skills should be:

- modular
- reusable
- composable
- governed
- testable
- replaceable

Skills do not independently redefine business objectives.

They execute the responsibilities assigned by the Execution Blueprint.

---

## Tools

`tools/` contains technical integrations used by Skills and Engines.

Examples include:

- language models
- web research
- GitHub
- image generation
- video generation
- analytics platforms
- website platforms
- publishing systems
- databases

The Runtime follows this responsibility model:

```text
Engine
Decides when and why work should happen
        │
        ▼
Skill
Defines what business capability should be performed
        │
        ▼
Tool
Provides the technical method used to perform it
```

Tools are implementation dependencies.

They are not sources of organisational authority.

---

## Memory

`memory/` contains Runtime-managed memory.

Scout uses three primary memory types.

```text
memory/
├── working/
├── episodic/
├── organisational/
├── indexes/
└── archive/
```

### Working Memory

Temporary task context used during active execution.

Working Memory may include:

- current instructions
- retrieved context
- intermediate state
- temporary decisions
- current outputs

Working Memory should normally be released or reduced after task completion.

### Episodic Memory

Historical execution records and operational experiences.

Examples include:

- completed tasks
- campaign outcomes
- customer interactions
- workflow failures
- quality findings
- execution decisions

Episodic Memory records what happened.

It does not automatically become approved organisational knowledge.

### Organisational Memory

Validated and reusable organisational learning.

Examples include:

- confirmed best practices
- reusable decision patterns
- validated customer insights
- improved workflows
- approved operational lessons

Knowledge requiring formal organisational approval may later be promoted into the Business Operating System or approved Repository.

---

## Workflows

`workflows/` defines how Engines and Skills are coordinated for repeatable business processes.

Examples include:

- general task execution
- content creation
- competitor research
- market intelligence
- website review
- daily business brief
- product documentation

A Workflow defines orchestration.

It should not duplicate the detailed logic of Engines or Skills.

---

## Schemas

`schemas/` defines the structured contracts used between Runtime components.

Expected schemas include:

- Task
- Knowledge Package
- Business Judgement
- Execution Blueprint
- Skill Request
- Skill Output
- Quality Report
- Feedback Record
- Learning Candidate
- Memory Record
- Exception Record

Schemas make Runtime behaviour:

- structured
- testable
- traceable
- interoperable
- easier to validate

Runtime components should exchange structured objects rather than ungoverned free-form text whenever practical.

---

## Registry

`registry/` contains governed machine-readable registries used by Runtime components.

For Runtime Objects:

- `engines/shared/runtime_objects.md` is the normative governance authority;
- `registry/runtime_objects.json` is the complete machine-readable registry data;
- `schemas/runtime_object_registry.schema.json` is the machine validation authority.

`runtime_objects.json` is registry data, not a JSON Schema. Registry entries reference their normative owners and authoritative schemas without replacing them.

---

## Prompts

`prompts/` contains model-facing instruction templates used by the Runtime.

Prompt categories may include:

- system
- retrieval
- reasoning
- planning
- execution
- quality
- feedback
- memory

Prompts are implementation assets.

They must not become the sole source of Scout's organisational rules.

Authoritative behaviour originates from:

```text
Business Operating System
+
Operating Manual
```

Prompts translate those rules into model instructions.

---

## Quality

`quality/` contains executable quality gates, validators, checklists and reports.

Quality operates across the entire Runtime.

```text
Input
  └── Task Quality Gate

Retrieval
  └── Knowledge Quality Gate

Reasoning
  └── Judgement Quality Gate

Planning
  └── Blueprint Quality Gate

Skills
  └── Output Quality Gate

Execution
  └── Final Delivery Gate

Memory
  └── Memory Admission Gate
```

Quality is not limited to final output review.

It is a cross-cutting Runtime capability.

---

## Logs

`logs/` records operational activity.

Examples include:

- execution records
- exceptions
- quality reports
- tool activity
- approval events
- learning candidates

Logs support:

- traceability
- debugging
- evaluation
- governance
- continuous improvement

Logs are operational records.

They should not automatically become organisational memory.

---

## Tests

`tests/` verifies that Runtime components behave as intended.

Testing may include:

- unit tests
- schema validation
- Engine tests
- Skill tests
- Workflow tests
- integration tests
- regression tests
- governance tests
- quality-gate tests

A Runtime capability should not be considered reliable until its critical behaviour can be tested.

---

## Scripts

`scripts/` contains executable development and operational entry points.

Examples include:

- run Scout
- run a Workflow
- validate configuration
- validate schemas
- rebuild memory indexes
- generate a daily brief
- run quality checks

Scripts provide controlled access to Runtime capabilities.

---

# Runtime Data Flow

The Runtime should use structured information throughout the execution lifecycle.

```text
Raw Request
    │
    ▼
Task
    │
    ▼
Knowledge Package
    │
    ▼
Business Judgement
    │
    ▼
Execution Blueprint
    │
    ▼
Skill Outputs
    │
    ▼
Quality Report
    │
    ▼
Execution Result
    │
    ▼
Feedback Record
    │
    ▼
Learning Candidate
    │
    ▼
Memory Record
```

Each object should have:

- a unique identifier
- creation time
- source information
- execution relationship
- status
- relevant evidence
- version information where required

This enables end-to-end traceability.

---

# Governance and Approval

Scout Runtime operates under delegated human authority.

The Runtime may autonomously perform approved activities such as:

- retrieval
- analysis
- drafting
- planning
- internal quality review
- non-destructive repository preparation

Human approval should normally be required before activities such as:

- public publishing
- financial commitments
- legal commitments
- major strategic changes
- deletion of approved organisational assets
- modification of governance documents
- modification of approved product facts
- irreversible external actions

Approval requirements should be configurable and auditable.

---

# Exception Handling

Runtime exceptions should be:

1. detected
2. classified
3. assessed
4. recovered where possible
5. escalated where required
6. recorded
7. evaluated for learning

The Runtime should return to the earliest appropriate stage when recovery is possible.

Examples:

```text
Missing knowledge
→ Retrieval

Unsupported judgement
→ Reasoning

Unexecutable plan
→ Planning

Skill failure
→ Execution or alternative Skill

Quality failure
→ Responsible upstream component

Governance uncertainty
→ Human escalation
```

Scout should not silently bypass failed stages.

---

# Development Principles

Runtime development follows the principles below.

## Behaviour Before Technology

Implementation should follow the Operating Manual rather than the preferences of a specific framework.

## Minimum Viable Runtime

The first objective is a small end-to-end working execution chain.

The objective is not to implement every planned component immediately.

## Structured Interfaces

Every Engine and Skill should have clear inputs, outputs and failure conditions.

## Real Business Validation

Runtime capabilities should be tested using real BUXA business tasks.

## Human Review During Early Development

Autonomy should increase only after consistent reliability has been demonstrated.

## Incremental Capability

New Skills and Tools should be introduced without destabilising the core architecture.

---

# Initial Implementation Scope

Scout Runtime v1 should first implement one complete business workflow.

The recommended initial workflow is:

```text
Xiaohongshu Content Package
```

The workflow should perform:

```text
Receive Content Task

↓

Retrieve Approved Content Memory

↓

Retrieve Brand and Product Knowledge

↓

Identify Previously Covered Themes

↓

Select and Justify a New Topic

↓

Create an Execution Blueprint

↓

Invoke Copy and Image Planning Skills

↓

Perform Product, Brand and Content Quality Review

↓

Return a Complete Content Package

↓

Capture Learning Candidates
```

The first version does not need to publish content automatically.

Its purpose is to prove that Scout can complete a governed end-to-end execution cycle.

---

# Initial Success Criteria

Scout Runtime v1 is successful when it can:

- receive a structured task
- retrieve approved internal knowledge
- produce a traceable Knowledge Package
- form a structured Business Judgement
- create an executable Execution Blueprint
- invoke at least one real business Skill
- perform a documented quality review
- return a complete business deliverable
- record execution results
- generate a learning candidate
- avoid modifying approved organisational knowledge without authorisation

Success is measured by reliable end-to-end execution rather than the number of implemented components.

---

# Current Status

Scout Runtime is under initial development.

The current development priority is:

1. establish the minimum Runtime structure
2. define core schemas
3. implement the execution orchestrator
4. register the first business Skill
5. complete one end-to-end Workflow
6. add quality verification
7. record execution and learning outputs
8. expand Skills only after the first Workflow is reliable

Planned directories and modules do not imply that all capabilities have already been implemented.

---

# Relationship with the Operating Manual

The Runtime implements the behaviour defined by the Operating Manual.

| Operating Manual | Runtime Implementation |
|---|---|
| Mission | Runtime objectives and success criteria |
| Operating Principles | Runtime rules and constraints |
| Execution Lifecycle | Workflow orchestration |
| Runtime Architecture | Core architecture |
| Retrieval | Retrieval Engine |
| Reasoning | Reasoning Engine |
| Planning | Planning Engine |
| Skills | Skill Registry and Execution Engine |
| Quality | Quality Gates and Validators |
| Memory | Memory Engine and Memory Stores |
| Feedback | Feedback and Learning Pipeline |
| Human Governance | Permissions and Approval Gates |
| Exception Handling | Runtime Exception Framework |

When Runtime behaviour conflicts with the Operating Manual, the Operating Manual takes precedence.

When the Operating Manual conflicts with the Business Operating System, the Business Operating System takes precedence.

---

# Authority Hierarchy

Scout Runtime follows this authority hierarchy:

```text
Human Governance
        │
        ▼
Business Operating System
        │
        ▼
Operating Manual
        │
        ▼
Runtime Configuration
        │
        ▼
Workflows
        │
        ▼
Engines
        │
        ▼
Skills
        │
        ▼
Tools
```

Lower layers must not override higher layers.

---

# Philosophy

Scout Runtime is not a collection of prompts, models or automation tools.

It is the executable intelligence layer of the organisation.

Its purpose is to connect organisational knowledge, structured judgement, reliable execution and continuous learning into one governed operating system.

The Runtime succeeds when Scout does more than complete a task.

It succeeds when each execution creates business value, preserves organisational integrity and improves the organisation's ability to perform future work.
