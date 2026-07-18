# AI Knowledge Graph

Version: 1.0

Status: Approved

Owner: Founder

Last Updated: July 2026

Document Type: AI Knowledge Graph Standard

Module: 11_AI

---

# Purpose

This document defines how Business Knowledge, content assets, operational evidence and organisational relationships are represented within the BUXA Operating System.

The AI Knowledge Graph enables Artificial Intelligence to understand knowledge as a connected system rather than a collection of isolated files.

Its purpose is to make relationships explicit, improve retrieval accuracy, strengthen traceability, support knowledge reuse and enable future AI agents to reason across the operating system.

Documents contain knowledge.

Metadata identifies knowledge.

Relationships connect knowledge.

The Knowledge Graph makes the operating system understandable as one system.

---

# Vision

BUXA aims to build an enterprise knowledge system in which Artificial Intelligence can understand:

- what each knowledge asset represents
- where knowledge originates
- which source is authoritative
- how products, processes and communication relate
- which assets depend on other assets
- how knowledge evolves over time
- where gaps, conflicts and duplication exist
- how one knowledge source can support many business activities

The Knowledge Graph should enable AI to retrieve relationships rather than isolated documents.

The long-term objective is not merely better search.

The objective is organisational intelligence.

---

# Knowledge Graph Philosophy

Business Knowledge does not exist independently.

Brand knowledge affects product communication.

Product knowledge affects website content.

PDMS knowledge affects customer education.

Content creates customer interaction.

Customer interaction generates business evidence.

Business evidence improves knowledge.

The Knowledge Graph represents these connections.

```text
Knowledge
        │
        ▼
Relationships
        │
        ▼
Context
        │
        ▼
Understanding
        │
        ▼
Reasoning
        │
        ▼
Action
```

Without relationships, Artificial Intelligence retrieves files.

With relationships, Artificial Intelligence understands the operating system.

---

# Knowledge Graph Architecture

The BUXA Knowledge Graph consists of three primary components.

```text
Nodes
        │
        ▼
Relationships
        │
        ▼
Properties
```

## Nodes

Nodes represent identifiable business entities or knowledge assets.

## Relationships

Relationships define how nodes are connected.

## Properties

Properties describe the identity, status, authority and operational characteristics of nodes and relationships.

Together, these components form the structured knowledge network of BUXA OS.

---

# Graph Model

The preferred graph model is:

```text
Business Entity
        │
        ▼
Authoritative Knowledge
        │
        ▼
Knowledge Topic
        │
        ▼
Core Asset
        │
        ▼
Platform Asset
        │
        ▼
Customer Evidence
        │
        ▼
Business Learning
        │
        ▼
Knowledge Update
```

This model allows the operating system to connect knowledge creation, communication, execution and learning.

---

# Node Types

Every graph node should belong to a defined node type.

Typical node types include:

- Business Module
- Brand Entity
- Product
- Product Collection
- Product Passport
- PDMS System
- PDMS Stage
- Control Point
- Inspection Standard
- Project Participant
- Website Page
- Knowledge Topic
- Core Content
- Platform Content
- Media Asset
- FAQ
- Template
- Customer Question
- Business Evidence
- Performance Insight
- Workflow
- Standard
- Policy
- AI Agent
- Prompt
- Automation
- External Source

Node types should describe what an entity is rather than where its file is stored.

---

# Business Module Nodes

Each major BUXA OS module should exist as a high-level node.

Examples include:

```text
Brand
Products
PDMS
Website
Content Operations
Marketing
SEO
Prompts
Knowledge Assets
Automation
AI
```

Business Module nodes provide the highest-level organisational context.

Every approved knowledge node should connect to at least one Business Module.

---

# Entity Nodes

Entity nodes represent real business concepts.

Examples include:

- BUXA
- BUXA HOMES
- BUXA PDMS
- Flooring Project Engineer
- Curated Sample Box
- European Oak Flooring
- HD05
- SKB01
- Design Review
- Moisture Inspection

Entity nodes should remain stable even when documents describing them change.

The entity is not the document.

The document describes the entity.

---

# Knowledge Asset Nodes

Knowledge Asset nodes represent authoritative documents.

Examples include:

- Brand Positioning
- Product Passport HD05
- PDMS Framework
- Content Metadata Standard
- AI Retrieval Standard
- Installation Standard
- Website Architecture

Knowledge Asset nodes should include:

- document identity
- module
- status
- version
- owner
- authority level
- review date
- file location

Knowledge Asset nodes may change version while preserving identity.

---

# Content Asset Nodes

Content Asset nodes represent communication derived from Business Knowledge.

Examples include:

- Website Article
- Xiaohongshu Post
- Facebook Post
- YouTube Script
- Buying Guide
- FAQ
- Presentation
- Media Asset

Content nodes should never replace authoritative knowledge nodes.

They should connect to their source through explicit relationships.

---

# Evidence Nodes

Evidence nodes represent information generated through business activity.

Examples include:

- Customer Question
- Customer Objection
- Search Query
- Sales Feedback
- Inspection Finding
- Project Issue
- Content Performance Insight
- Product Feedback
- Operational Observation

Evidence is not automatically approved Business Knowledge.

Evidence should connect to review and validation processes before updating authoritative knowledge.

---

# Agent Nodes

Future AI Agents may be represented as graph nodes.

Examples include:

- Brand Agent
- Product Agent
- PDMS Agent
- Content Agent
- SEO Agent
- Website Agent
- Analytics Agent

Agent nodes may connect to:

- authorised modules
- permitted tasks
- governed workflows
- required standards
- accessible knowledge
- escalation rules

AI Agents participate in the graph.

They do not own authoritative knowledge.

---

# Relationship Types

Relationships should use controlled and clearly defined verbs.

Recommended relationship types include:

```text
belongs_to
defines
describes
governed_by
implements
depends_on
references
supports
related_to
derived_from
transformed_into
adapted_from
supersedes
replaces
reused_by
answers
applies_to
owned_by
approved_by
reviewed_by
validated_by
generated_by
published_on
measured_by
produces
identifies
updates
contradicts
requires
```

Relationship names should be explicit and directional.

Avoid vague relationship types such as:

```text
connected_to
associated_with
link
```

unless a more precise relationship cannot yet be determined.

---

# Relationship Direction

Relationships should communicate clear direction.

Example:

```text
XHS-041
        │
        └── derived_from → CORE-004
```

This is different from:

```text
CORE-004
        │
        └── transformed_into → XHS-041
```

Both relationships may be recorded when useful.

Direction improves AI reasoning.

---

# Relationship Examples

## Product Communication

```text
HD05
        │
        ├── described_by → Product Passport HD05
        ├── belongs_to → European Oak Flooring
        ├── represented_by → HD05 Product Images
        ├── featured_on → HD05 Product Page
        ├── included_in → Coastal Light Sample Box
        └── referenced_by → Xiaohongshu Product Post
```

---

## PDMS Communication

```text
Design Review
        │
        ├── belongs_to → BUXA PDMS
        ├── defined_by → PDMS Project Workflow
        ├── performed_by → Flooring Project Engineer
        ├── prevents → Rework Risk
        ├── explained_by → Core Content PDMS-004
        └── adapted_into → XHS-041
```

---

## Content Reuse

```text
Knowledge Topic
        │
        ▼
Core Content
        │
        ├── adapted_into → Website Article
        ├── adapted_into → Xiaohongshu Post
        ├── adapted_into → Facebook Post
        ├── adapted_into → Video Script
        └── answers → FAQ
```

---

## Knowledge Evolution

```text
Customer Question
        │
        ├── identifies → Knowledge Gap
        ├── supports → Business Review
        └── contributes_to → Knowledge Update
```

```text
Knowledge Update
        │
        ├── supersedes → Previous Knowledge Version
        ├── updates → Related Content Assets
        └── improves → Future Communication
```

---

# Node Properties

Every node should include properties appropriate to its type.

Standard identity properties may include:

```yaml
node_id:
node_type:
name:
description:
module:
status:
version:
owner:
created:
updated:
```

Governance properties may include:

```yaml
authority_level:
approval:
approved_by:
review_date:
next_review:
archive_status:
```

Retrieval properties may include:

```yaml
keywords:
aliases:
language:
file_path:
source_location:
retrieval_priority:
```

Relationship properties may include:

```yaml
relationship_id:
relationship_type:
source_node:
target_node:
status:
confidence:
created:
validated_by:
```

Not every node requires every property.

Required properties should depend on node type.

---

# Node Identity

Every important node should have a stable identifier.

Examples include:

```text
BRAND-POSITIONING
PRODUCT-HD05
PDMS-DESIGN-REVIEW
CONTENT-CORE-004
XHS-041
FAQ-012
AI-RETRIEVAL
```

Identifiers should be:

- unique
- stable
- human readable
- machine readable
- independent of filenames where possible

Renaming a file should not create a new business entity.

---

# Canonical Names and Aliases

Each entity should have one canonical name.

Example:

```yaml
canonical_name: Flooring Project Engineer
aliases:
  - FPE
```

Aliases improve retrieval.

Aliases should not create competing terminology.

The canonical name remains authoritative.

---

# Authority in the Knowledge Graph

Nodes should include an authority level.

Recommended authority hierarchy:

```text
Level 1 — Founder Authority

Level 2 — Repository Governance

Level 3 — Approved System Architecture

Level 4 — Approved Standards and Policies

Level 5 — Approved Business Knowledge

Level 6 — Approved Content Assets

Level 7 — Working Documents

Level 8 — External Sources

Level 9 — General Model Knowledge
```

When graph relationships conflict, higher-authority nodes take precedence.

Authority should remain explicit rather than inferred.

---

# Graph Governance

The Knowledge Graph is governed Business Knowledge.

Artificial Intelligence may:

- recommend nodes
- recommend relationships
- identify missing links
- detect possible duplicates
- identify possible conflicts
- generate graph metadata

Artificial Intelligence may not independently:

- approve authoritative nodes
- change authority levels
- remove approved relationships
- merge business entities
- redefine canonical terminology
- update Business Knowledge

Graph changes affecting authoritative knowledge require human approval.

---

# Graph Construction Workflow

The preferred construction workflow is:

```text
Identify Entity
        │
        ▼
Assign Node Type
        │
        ▼
Assign Stable ID
        │
        ▼
Attach Metadata
        │
        ▼
Identify Relationships
        │
        ▼
Validate Authority
        │
        ▼
Review
        │
        ▼
Approve
        │
        ▼
Publish to Graph
```

Graph construction should begin with authoritative knowledge rather than generated content.

---

# Graph Extraction from Documents

Artificial Intelligence may analyse approved documents to recommend graph structures.

Extraction may identify:

- business entities
- document types
- products
- workflows
- standards
- participants
- topics
- dependencies
- derived assets
- version relationships

Extracted nodes and relationships remain candidates until validated.

Text similarity alone does not prove a business relationship.

---

# Graph Relationship Validation

Before approving a relationship, confirm:

- both nodes exist
- the relationship type is correct
- the direction is correct
- the relationship is supported by evidence
- the relationship does not conflict with higher authority
- the relationship remains current
- the relationship improves retrieval or reasoning

Relationships should create meaningful context.

More relationships do not automatically create a better graph.

---

# Relationship Confidence

Relationships may include a confidence level.

Recommended values include:

```text
Confirmed
Probable
Candidate
Rejected
```

## Confirmed

Supported by approved knowledge or human validation.

## Probable

Strongly supported but awaiting formal validation.

## Candidate

Suggested by AI or incomplete evidence.

## Rejected

Reviewed and determined to be invalid.

Only Confirmed relationships should be treated as authoritative.

---

# Duplicate Detection

The Knowledge Graph should help identify duplicated entities and knowledge.

Possible duplicate indicators include:

- identical canonical names
- overlapping aliases
- matching product codes
- highly similar definitions
- competing files describing the same entity
- multiple active versions without supersession links

Artificial Intelligence should recommend review.

It should not automatically merge authoritative nodes.

---

# Conflict Detection

The graph should expose conflicting knowledge.

Examples include:

```text
HD05
        ├── dimension → 1900 × 190 × 14/3 mm
        └── dimension → 2200 × 220 × 15/4 mm
```

Possible outcomes include:

- different product variants
- outdated knowledge
- incorrect relationship
- unresolved business conflict

Artificial Intelligence should identify the conflict and retrieve supporting sources.

It should not invent a compromise.

---

# Version Relationships

Knowledge versions should remain connected.

Example:

```text
Product Passport HD05 v1.0
        │
        └── superseded_by → Product Passport HD05 v2.0
```

The current version should be identifiable through:

- status
- authority
- version
- supersession relationships

Historical versions should remain traceable.

---

# Content Derivation Relationships

Every content asset should connect to its source knowledge.

Minimum derivation model:

```text
Content Asset
        │
        ├── derived_from → Knowledge Topic
        ├── references → Authoritative Knowledge
        ├── adapted_for → Platform
        └── governed_by → Content Standards
```

Untraceable content should not be treated as an approved organisational asset.

---

# Evidence-to-Knowledge Relationships

Evidence should not directly overwrite knowledge.

The preferred model is:

```text
Evidence
        │
        ▼
Business Review
        │
        ▼
Validated Insight
        │
        ▼
Knowledge Update
```

Graph relationships may include:

```text
Customer Question
        └── identifies → Knowledge Gap
```

```text
Repeated Customer Questions
        └── supports → Validated Insight
```

```text
Validated Insight
        └── updates → FAQ
```

```text
Validated Insight
        └── recommends_update_to → Business Knowledge
```

Human governance remains required.

---

# Knowledge Graph and Retrieval

AI Retrieval should use the Knowledge Graph to expand and validate context.

Preferred sequence:

```text
Task
        │
        ▼
Primary Node
        │
        ▼
Authoritative Relationships
        │
        ▼
Supporting Nodes
        │
        ▼
Relevant Documents
        │
        ▼
Retrieval Package
```

Graph retrieval helps AI identify:

- source knowledge
- related standards
- dependent assets
- current versions
- relevant products
- relevant workflows
- potential conflicts

The graph should improve retrieval precision rather than indiscriminately expand context.

---

# Retrieval Depth

Artificial Intelligence should control how far it traverses the graph.

Recommended retrieval depths include:

## Depth 0

Retrieve only the primary node.

Used for exact identity or metadata questions.

---

## Depth 1

Retrieve directly connected nodes.

Used for most ordinary tasks.

---

## Depth 2

Retrieve relationships of directly connected nodes.

Used for cross-module tasks and content transformation.

---

## Depth 3+

Use only for complex analysis, system architecture or major repository changes.

Uncontrolled graph traversal may introduce irrelevant context.

---

# Knowledge Graph and Memory

The Knowledge Graph defines what knowledge exists and how it relates.

AI Memory defines which context should persist across tasks.

The relationship is:

```text
Knowledge Graph
        │
        ▼
Structured Organisational Knowledge
        │
        ▼
Memory Selection
        │
        ▼
Persistent AI Context
```

The Knowledge Graph should not be treated as AI memory itself.

The graph is the structured knowledge environment from which memory may be selected.

---

# Knowledge Graph and Content Operations

The Content Operating System should use the Knowledge Graph to support:

- topic discovery
- knowledge reuse
- source traceability
- asset relationships
- duplication detection
- knowledge gap detection
- content updates
- platform adaptation
- performance learning

Example:

```text
Customer Question
        │
        ▼
Knowledge Gap
        │
        ▼
Knowledge Topic
        │
        ▼
Core Content
        │
        ▼
Platform Assets
        │
        ▼
Customer Interaction
```

The graph closes the connection between knowledge and communication.

---

# Knowledge Graph and Agents

Future AI Agents should retrieve graph subsets based on their authority.

Examples include:

## Brand Agent

May access:

- Brand Nodes
- Communication Standards
- Approved Terminology
- Related Content Nodes

---

## Product Agent

May access:

- Product Nodes
- Product Passports
- Collections
- Technical Standards
- Product Media

---

## PDMS Agent

May access:

- PDMS Stages
- Control Points
- Inspection Standards
- Project Participants
- Evidence Structures

---

## Content Agent

May access:

- Knowledge Topics
- Core Content
- Platform Assets
- Templates
- Content Performance

Agent access should follow governance and task requirements.

---

# Graph Quality Standards

A healthy Knowledge Graph should be:

- authoritative
- traceable
- current
- explicit
- non-duplicative
- navigable
- governed
- useful for retrieval
- understandable by people and AI

Graph quality should be measured by usefulness rather than node count.

---

# Graph Quality Metrics

Possible metrics include:

- Node Coverage
- Relationship Coverage
- Authority Completeness
- Metadata Completeness
- Orphan Node Rate
- Duplicate Node Rate
- Conflict Detection Rate
- Current Version Identification Rate
- Retrieval Success Rate
- Relationship Validation Rate
- Cross-Module Connectivity
- Knowledge Gap Resolution Rate

Metrics should improve organisational intelligence.

---

# Orphan Nodes

An orphan node has no meaningful relationship with the wider operating system.

Examples include:

- content with no knowledge source
- product media with no product relationship
- an FAQ with no Knowledge Topic
- a standard with no governing architecture
- an archived document without a supersession relationship

Orphan nodes should be:

- linked
- reviewed
- archived
- or removed from active retrieval

Approved active nodes should rarely remain isolated.

---

# Graph Review Cycle

The Knowledge Graph should be reviewed when:

- new Business Knowledge is approved
- products are added or changed
- PDMS stages or standards change
- new content is published
- terminology changes
- assets are archived
- conflicts are detected
- AI identifies repeated retrieval failures

Graph maintenance should follow Business Knowledge updates.

---

# Graph Change Management

Graph changes should follow:

```text
Change Candidate
        │
        ▼
Relationship Review
        │
        ▼
Authority Validation
        │
        ▼
Impact Analysis
        │
        ▼
Human Approval
        │
        ▼
Graph Update
        │
        ▼
Retrieval Validation
```

Changing one node may affect many dependent assets.

Impact should be evaluated before approval.

---

# Graph Impact Analysis

Before changing an authoritative node, identify:

- dependent documents
- derived content
- related products
- affected workflows
- website pages
- FAQs
- media assets
- agent instructions
- automation rules

Example:

```text
Product Specification Update
        │
        ├── affects → Product Passport
        ├── affects → Product Page
        ├── affects → Buying Guide
        ├── affects → Sales Material
        ├── affects → AI Answers
        └── affects → Product Media
```

The Knowledge Graph makes change impact visible.

---

# Graph Storage Principles

The graph may initially be implemented through:

- structured Markdown metadata
- stable identifiers
- explicit relationship fields
- repository links
- controlled vocabularies
- index documents

A specialised graph database is not required during the initial stage.

Architecture should precede software.

The repository should first become graph-ready.

---

# Repository-First Implementation

The initial Knowledge Graph should be represented through consistent metadata.

Example:

```yaml
node_id: XHS-041
node_type: Platform Content
module: Content Operations
status: Published

relationships:
  derived_from:
    - CORE-004
  references:
    - PDMS-FRAMEWORK
    - BRAND-POSITIONING
  published_on:
    - XIAOHONGSHU
  related_to:
    - XHS-036
  reused_by:
    - NEWSLETTER-008
```

This approach allows the graph to grow without introducing unnecessary technical complexity.

---

# Minimum Graph Requirements

Every approved knowledge or content asset should define:

```yaml
node_id:
node_type:
module:
status:
version:
owner:
relationships:
```

Every Content Asset should additionally define:

```yaml
knowledge_sources:
parent_asset:
related_assets:
```

Every superseded asset should define:

```yaml
superseded_by:
archive_status:
```

---

# AI Graph Operations

Artificial Intelligence may perform the following graph operations:

- Discover Candidate Node
- Discover Candidate Relationship
- Retrieve Node
- Traverse Relationship
- Detect Duplicate
- Detect Conflict
- Detect Orphan
- Recommend Link
- Recommend Update
- Generate Impact Report

Artificial Intelligence may not independently:

- approve nodes
- merge authoritative entities
- delete approved nodes
- change authority
- promote evidence into knowledge
- redefine relationship vocabulary

---

# Human Review Requirements

Human review is required when graph operations affect:

- Brand Positioning
- Product specifications
- PDMS Standards
- business policies
- canonical terminology
- authority levels
- node merging
- node deletion
- supersession
- cross-module dependencies

Artificial Intelligence supports graph maintenance.

People govern the graph.

---

# Knowledge Graph Checklist

Before approving a node or relationship, confirm:

✓ Stable Node ID Assigned

✓ Node Type Defined

✓ Canonical Name Confirmed

✓ Module Assigned

✓ Authority Level Recorded

✓ Version and Status Confirmed

✓ Source Knowledge Traceable

✓ Relationships Explicit

✓ Relationship Direction Correct

✓ Relationship Evidence Available

✓ Duplicate Check Completed

✓ Conflict Check Completed

✓ Human Approval Recorded Where Required

---

# Relationship with Other Documents

This document implements:

- `00_AI Architecture.md`
- `01_AI Governance.md`
- `02_AI Knowledge Loading.md`
- `03_AI Agent Standards.md`
- `09_AI Retrieval.md`

It supports:

- `04_AI Knowledge Transformation.md`
- `05_AI Media Standards.md`
- `06_AI QA Checklist.md`
- `07_AI Collaboration.md`
- `08_AI Platforms.md`
- `11_AI Memory.md`

It also depends upon:

- `AGENTS.md`
- Module Architecture Documents
- Metadata Standards
- Approved Business Knowledge
- Content Governance

The AI Knowledge Graph connects the BUXA Knowledge Base into one structured organisational system.

---

# Future Development

Future Knowledge Graph capabilities may include:

- Automated Entity Extraction
- Semantic Relationship Discovery
- Graph-Based Retrieval
- Graph Embeddings
- Dynamic Impact Analysis
- Temporal Knowledge Graphs
- Agent-Specific Graph Views
- Multilingual Entity Alignment
- Automated Conflict Monitoring
- Knowledge Gap Prediction
- Enterprise Graph Analytics
- Graph-Based Workflow Automation

Future technology should extend the graph without changing its governance principles.

---

# Single Source of Truth

The AI Knowledge Graph defines how Business Knowledge and organisational relationships are represented within the BUXA Operating System.

Nodes identify knowledge.

Properties describe knowledge.

Relationships create context.

Authority creates trust.

Retrieval provides access.

Artificial Intelligence reasons across relationships.

People govern authoritative knowledge.

The Knowledge Graph transforms isolated documents into an intelligent organisational system.

One Brand.

One Knowledge Base.

One Operating System.
