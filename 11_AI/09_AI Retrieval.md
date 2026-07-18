# AI Retrieval

Version: 1.0

Status: Approved

Owner: Founder

Last Updated: July 2026

Document Type: AI Retrieval Standard

Module: 11_AI

---

# Purpose

This document defines how Artificial Intelligence retrieves Business Knowledge within the BUXA Operating System.

Retrieval is the process of identifying, locating, ranking and validating the knowledge required to complete a task.

Artificial Intelligence must retrieve before it reasons, recommends, transforms or generates.

The purpose of AI Retrieval is to ensure that every AI output is grounded in authoritative Business Knowledge rather than unsupported model memory.

Retrieval protects accuracy.

Retrieval preserves consistency.

Retrieval enables traceability.

---

# Vision

BUXA aims to build an AI-native operating system in which Artificial Intelligence can reliably access the right knowledge at the right time.

The retrieval system should enable AI to:

- identify the user’s actual task
- locate authoritative Business Knowledge
- retrieve relevant supporting context
- distinguish current knowledge from outdated information
- recognise relationships between documents
- avoid unnecessary duplication
- produce traceable outputs
- support future agents and automation

AI should never depend on general model knowledge when authoritative BUXA knowledge is available.

---

# Retrieval Philosophy

Artificial Intelligence does not begin with generation.

Artificial Intelligence begins with retrieval.

The preferred operating sequence is:

```text
Understand
        │
        ▼
Classify
        │
        ▼
Retrieve
        │
        ▼
Validate
        │
        ▼
Reason
        │
        ▼
Transform
        │
        ▼
Review
```

Retrieval provides evidence.

Reasoning interprets evidence.

Transformation communicates evidence.

Human governance approves official outcomes.

---

# Retrieval Architecture

The AI Retrieval System follows one standard architecture.

```text
User Task
        │
        ▼
Task Interpretation
        │
        ▼
Task Classification
        │
        ▼
Authority Mapping
        │
        ▼
Metadata Retrieval
        │
        ▼
Relationship Retrieval
        │
        ▼
Document Retrieval
        │
        ▼
Content Extraction
        │
        ▼
Source Validation
        │
        ▼
Retrieval Package
        │
        ▼
Reasoning or Transformation
```

Each stage narrows the knowledge scope.

Retrieval should provide sufficient context without unnecessarily loading the entire repository.

---

# Retrieval Principles

Artificial Intelligence should always:

- retrieve before generating
- prioritise authoritative sources
- use metadata before full-text search
- retrieve relationships as well as documents
- confirm version and status
- prefer current approved knowledge
- preserve source traceability
- report uncertainty
- avoid irrelevant context
- stop when sufficient evidence has been retrieved

Retrieval quality is more important than retrieval volume.

---

# Task Interpretation

Before retrieving knowledge, Artificial Intelligence should determine what the user is actually asking.

Task interpretation should identify:

- requested outcome
- business domain
- target audience
- platform or output format
- required level of detail
- whether the task changes Business Knowledge
- whether human approval is required
- whether external information is needed

A task should not be classified only by its wording.

It should be classified by its business impact.

---

# Task Classification

Every task should be assigned to one or more task classes.

Typical classes include:

- Knowledge Question
- Content Transformation
- Product Task
- PDMS Task
- Brand Task
- Website Task
- Marketing Task
- SEO Task
- Media Task
- Governance Task
- Repository Maintenance
- Analysis
- External Research

Task classification determines which modules and documents should be retrieved.

---

# Authority Mapping

After classifying the task, Artificial Intelligence should identify the authoritative knowledge modules.

Typical authority mapping includes:

| Task Domain | Primary Authority |
|---|---|
| Brand Positioning | `01_Brand` |
| Product Information | `02_Products` |
| PDMS | `03_PDMS` |
| Website Structure | `04_Website` |
| Content Operations | `05_Content Operations` |
| Marketing | `06_Marketing` |
| SEO | `07_SEO` |
| Prompt Standards | `08_Prompts` |
| Knowledge Assets | `09_Knowledge Assets` |
| Automation | `10_Automation` |
| AI Operations | `11_AI` |
| Repository Governance | `AGENTS.md` |

A task may require more than one authority.

When multiple authorities apply, Artificial Intelligence should retrieve all required sources before proceeding.

---

# Authority Hierarchy

When retrieved sources conflict, authority should be resolved using the repository hierarchy.

```text
Founder Instruction
        │
        ▼
AGENTS.md
        │
        ▼
Approved Module Architecture
        │
        ▼
Approved Standards and Policies
        │
        ▼
Approved Knowledge Documents
        │
        ▼
Approved Content Assets
        │
        ▼
Drafts and Working Notes
        │
        ▼
General Model Knowledge
```

Lower-authority sources must not override higher-authority sources.

When authority remains unclear, Artificial Intelligence should identify the conflict rather than silently selecting one version.

---

# Retrieval Order

Artificial Intelligence should use the following retrieval order.

## Step 01

### Repository Governance

Retrieve:

- `AGENTS.md`
- relevant module `README.md`
- relevant architecture documents

This establishes authority, scope and operating rules.

---

## Step 02

### Metadata

Retrieve metadata to identify:

- document type
- status
- version
- owner
- module
- knowledge topic
- relationships
- approval state

Metadata should narrow the search before full documents are loaded.

---

## Step 03

### Authoritative Knowledge

Retrieve the approved Business Knowledge directly relevant to the task.

Examples include:

- Brand Positioning
- Product Passports
- PDMS Standards
- Website Architecture
- Content Standards
- AI Governance

Authoritative knowledge forms the evidence base.

---

## Step 04

### Related Knowledge

Retrieve documents related through explicit relationships.

Examples include:

- parent documents
- child documents
- supporting standards
- related products
- related workflows
- related platform assets
- superseded versions

Relationships provide context that isolated keyword search may miss.

---

## Step 05

### Existing Assets

Retrieve existing approved assets before generating new ones.

Examples include:

- previous content
- templates
- website sections
- FAQs
- scripts
- diagrams
- media assets

Reuse should be preferred over recreation.

---

## Step 06

### External Knowledge

External information should only be retrieved when:

- the user explicitly requests current external information
- the repository does not contain the required knowledge
- laws, regulations, platforms or market conditions may have changed
- verification is required

External information should remain separate from approved BUXA Business Knowledge until reviewed.

---

# Metadata-First Retrieval

Artificial Intelligence should retrieve using metadata before searching full document bodies.

Preferred metadata filters include:

```yaml
module:
document_type:
knowledge_topic:
status:
version:
owner:
audience:
platform:
product:
pdms_stage:
related_documents:
knowledge_sources:
```

Metadata-first retrieval improves:

- precision
- speed
- consistency
- governance
- AI understanding

Full-text search should expand retrieval rather than replace metadata.

---

# Retrieval Query Construction

Retrieval queries should be specific enough to locate authoritative knowledge.

A good retrieval query should include:

- business entity
- task type
- knowledge topic
- authoritative module
- required status
- relevant terminology

Example:

```text
Retrieve approved Product Passport information for HD05,
including finish, dimensions, positioning and related visual standards.
```

Avoid weak queries such as:

```text
Find information about HD05.
```

Precise queries improve retrieval quality.

---

# Semantic and Keyword Retrieval

Artificial Intelligence should combine two retrieval methods.

## Keyword Retrieval

Useful for:

- exact product codes
- filenames
- approved terminology
- standards
- metadata values
- identifiers

Examples:

- `HD05`
- `BUXA PDMS`
- `Flooring Project Engineer`
- `Content Metadata Standard`

---

## Semantic Retrieval

Useful for:

- customer questions
- business concepts
- related topics
- differently worded knowledge
- incomplete user requests

Examples:

- preventing residential project rework
- why designers need delivery management
- choosing oak flooring colour at home

Keyword and semantic retrieval should support one another.

---

# Relationship Retrieval

Artificial Intelligence should retrieve knowledge relationships, not only isolated documents.

Typical relationships include:

```text
supports
references
defines
implements
depends_on
derived_from
related_to
supersedes
reused_by
answers
updates
governed_by
```

Example:

```text
XHS-041
        │
        ├── derived_from → CORE-004
        ├── references → PDMS Framework
        ├── supports → Brand Positioning
        ├── related_to → XHS-036
        └── reused_by → Newsletter-008
```

Relationship retrieval strengthens context and future reuse.

---

# Source Validation

Every retrieved source should be validated before use.

Validation should confirm:

- the file belongs to the correct module
- the document is approved
- the version is current
- the content is not archived or superseded
- the terminology remains valid
- the source has sufficient authority
- the source directly supports the task

Artificial Intelligence should not rely on a source merely because it contains matching words.

---

# Freshness Validation

Artificial Intelligence should evaluate knowledge freshness using:

- document status
- version number
- last updated date
- next review date
- product status
- superseded relationships
- module authority
- business context

A recently modified file is not automatically authoritative.

Content and governance status take precedence over modification timestamps.

---

# Conflict Detection

Retrieved sources may occasionally conflict.

Typical conflicts include:

- different product dimensions
- outdated pricing
- changed terminology
- competing positioning statements
- superseded workflow rules
- draft content contradicting approved knowledge

When conflict is detected, Artificial Intelligence should:

1. identify the conflicting sources
2. compare authority
3. compare version and status
4. select the higher-authority source when clear
5. report unresolved conflicts
6. avoid inventing a compromise

Conflicts should create governance tasks when necessary.

---

# Retrieval Sufficiency

Artificial Intelligence should determine whether enough knowledge has been retrieved before proceeding.

Retrieval is sufficient when:

- the task is clearly understood
- the primary authoritative source is available
- relevant constraints are known
- terminology is confirmed
- conflicts have been resolved or disclosed
- the requested output can be produced without invention

More documents do not always improve retrieval.

Excessive context may reduce clarity and increase error.

---

# Retrieval Package

Before reasoning or generation, Artificial Intelligence should form a structured Retrieval Package.

A Retrieval Package may include:

```yaml
task:
task_class:
authoritative_modules:
primary_sources:
supporting_sources:
related_assets:
approved_terminology:
business_constraints:
platform_constraints:
known_conflicts:
missing_information:
approval_requirements:
```

The Retrieval Package becomes the evidence base for downstream AI work.

---

# Retrieval Traceability

Every important AI output should preserve source traceability.

Traceability may include:

- filenames
- document IDs
- section references
- knowledge source metadata
- version numbers
- relationship records

Official outputs should be reproducible from their source knowledge.

Untraceable claims should not be treated as approved Business Knowledge.

---

# Retrieval and Generation

Artificial Intelligence should not begin generation until retrieval is complete.

The operating boundary is:

```text
Insufficient Retrieval
        │
        ▼
Retrieve More
```

```text
Sufficient Retrieval
        │
        ▼
Reason
        │
        ▼
Transform
```

Generation without retrieval creates inconsistency.

Retrieval without validation creates false confidence.

---

# Retrieval and Reuse

Before creating a new asset, Artificial Intelligence should check whether an appropriate asset already exists.

Possible outcomes include:

- reuse without change
- adapt existing asset
- update existing asset
- create a derived asset
- create new Core Content
- identify a knowledge gap

New creation should be the final option rather than the default option.

---

# Retrieval and Human Review

Human review is required when retrieval reveals:

- conflicting authoritative sources
- missing Business Knowledge
- potential Product Passport changes
- potential PDMS Standard changes
- Brand Positioning changes
- legal or regulatory uncertainty
- unsupported business claims
- governance exceptions

Artificial Intelligence should escalate uncertainty rather than conceal it.

---

# Retrieval Failure

Retrieval may fail when:

- no authoritative source exists
- metadata is incomplete
- document relationships are missing
- relevant files cannot be located
- sources conflict
- the task is outside repository scope
- external verification is unavailable

When retrieval fails, Artificial Intelligence should:

- state what is missing
- identify which sources were checked
- avoid unsupported conclusions
- recommend the required knowledge update or clarification

Failure transparency protects trust.

---

# Retrieval for Different Task Types

## Knowledge Questions

Retrieve the smallest authoritative set needed to answer accurately.

---

## Content Transformation

Retrieve:

- Business Knowledge
- Content Standards
- platform rules
- existing assets
- metadata requirements

---

## Product Tasks

Retrieve:

- Product Passport
- Product Data Standards
- related technical documents
- approved pricing or commercial rules when relevant

---

## PDMS Tasks

Retrieve:

- PDMS Architecture
- workflow
- stage standards
- participant responsibilities
- inspection standards
- evidence requirements

---

## Brand Tasks

Retrieve:

- Brand Positioning
- Brand Architecture
- Communication Standards
- approved terminology

---

## Repository Changes

Retrieve:

- `AGENTS.md`
- module README
- affected architecture
- affected standards
- related files

Repository modification requires broader retrieval than ordinary content generation.

---

# Retrieval Metrics

Retrieval quality may be evaluated through:

- Retrieval Accuracy
- Source Authority Rate
- Relevant Context Ratio
- Conflict Detection Rate
- Reuse Identification Rate
- Missing Knowledge Detection
- Traceability Completeness
- Retrieval Time
- Human Correction Rate

The objective is reliable knowledge access rather than maximum document volume.

---

# AI Retrieval Checklist

Before reasoning or generation, confirm:

✓ Task Interpreted

✓ Task Classified

✓ Authority Mapped

✓ Relevant Module Retrieved

✓ Metadata Checked

✓ Approved Sources Identified

✓ Current Version Confirmed

✓ Related Knowledge Retrieved

✓ Existing Assets Checked

✓ Conflicts Resolved or Disclosed

✓ Retrieval Package Complete

✓ Traceability Preserved

---

# Relationship with Other Documents

This document implements:

- `00_AI Architecture.md`
- `01_AI Governance.md`
- `02_AI Knowledge Loading.md`
- `03_AI Agent Standards.md`

It supports:

- `04_AI Knowledge Transformation.md`
- `05_AI Media Standards.md`
- `06_AI QA Checklist.md`
- `07_AI Collaboration.md`
- `08_AI Platforms.md`
- `10_AI Knowledge Graph.md`
- `11_AI Memory.md`

It also depends upon:

- `AGENTS.md`
- module architecture documents
- module metadata standards
- approved Business Knowledge

AI Retrieval is the operational bridge between the Knowledge Base and AI execution.

---

# Future Development

Future retrieval capabilities may include:

- Hybrid Semantic Retrieval
- Knowledge Graph Retrieval
- Agent-Specific Retrieval
- Query Expansion
- Automated Source Ranking
- Dynamic Context Assembly
- Predictive Knowledge Retrieval
- Retrieval Quality Monitoring
- Multilingual Retrieval
- Context Compression
- Autonomous Conflict Detection

Future capabilities should improve access to knowledge without weakening governance.

---

# Single Source of Truth

AI Retrieval defines how Artificial Intelligence identifies and retrieves authoritative Business Knowledge within the BUXA Operating System.

The user task defines the need.

Authority defines the source.

Metadata narrows the scope.

Relationships provide context.

Validation creates trust.

Retrieval provides evidence.

Artificial Intelligence reasons from evidence.

People remain responsible for governance.

Retrieve before reasoning.

Retrieve before transforming.

Retrieve before generating.

One Brand.

One Knowledge Base.

One Operating System.
