# Content Metadata Standard

Version: 1.0

Status: Approved

Owner: Founder

Last Updated: July 2026

Document Type: Metadata Standard

Module: 05_Content Operations

---

# Purpose

This document defines the metadata standard for all content assets within the BUXA Content Operating System.

Metadata provides structured information that enables people, Artificial Intelligence and future automation to identify, retrieve, understand and reuse content consistently.

Metadata is not simply descriptive information.

Metadata forms the identity of every content asset.

Every approved content asset should include metadata that follows this standard.

---

# Metadata Philosophy

Content should be understandable without reading the entire document.

Metadata should explain:

- what the asset is
- why it exists
- where it originated
- who it serves
- how it relates to other knowledge
- whether it may be reused

Metadata enables organisational intelligence.

Well-structured metadata enables scalable Artificial Intelligence.

---

# Metadata Architecture

Every metadata record is organised into five layers.

```text
Identity
        │
        ▼
Business
        │
        ▼
Content
        │
        ▼
Relationships
        │
        ▼
Governance
```

Each layer has one clear responsibility.

---

# Layer 01

## Identity Metadata

Identity uniquely identifies a content asset.

Typical fields include:

- Content ID
- Title
- Asset Type
- Version
- Language
- Author
- Owner
- Created Date
- Last Updated

Identity should remain stable throughout the asset lifecycle.

---

## Standard Fields

```yaml
content_id:
title:
asset_type:
version:
language:
author:
owner:
created:
updated:
```

---

# Layer 02

## Business Metadata

Business Metadata explains why the asset exists.

Typical fields include:

- Business Module
- Knowledge Topic
- Objective
- Audience
- Customer Journey
- Business Value
- Product
- PDMS Stage

---

## Standard Fields

```yaml
module:
knowledge_topic:
objective:
audience:
customer_journey:
business_value:
product:
pdms_stage:
```

---

# Layer 03

## Content Metadata

Content Metadata describes the communication itself.

Typical fields include:

- Platform
- Content Type
- Content Pillar
- Keywords
- Summary
- Reading Time
- Call To Action
- Media

---

## Standard Fields

```yaml
platform:
content_type:
pillar:
keywords:
summary:
reading_time:
call_to_action:
media:
```

---

# Layer 04

## Relationship Metadata

Every content asset should explicitly describe its relationships.

Relationships enable Artificial Intelligence to understand context.

Relationship examples include:

- Business Knowledge
- Product Passport
- Brand Document
- Website Page
- FAQ
- Previous Version
- Derived Assets
- Related Topics

---

## Standard Fields

```yaml
knowledge_sources:
related_products:
related_documents:
related_assets:
parent_asset:
child_assets:
reused_by:
supersedes:
```

Relationships should be explicit.

Artificial Intelligence should not infer critical relationships.

---

# Layer 05

## Governance Metadata

Governance Metadata defines the operational status of a content asset.

Typical fields include:

- Status
- Approval
- Reviewer
- Publish Date
- Review Date
- Archive Status

---

## Standard Fields

```yaml
status:
approval:
reviewer:
publish_date:
next_review:
archive_status:
```

Only approved content may enter the Content Library.

---

# Metadata Example

The following example illustrates a complete metadata record.

```yaml
content_id: XHS-041

title: Why Designers Fear Rework More Than Cost

asset_type: Social Post

version: 1.0

language: en

author: Founder

owner: BUXA

created: 2026-07-18

updated: 2026-07-18

module: PDMS

knowledge_topic: Project Delivery Management

objective: Education

audience: Interior Designers

customer_journey: Consideration

business_value: Brand Authority

product: BUXA PDMS

pdms_stage: Design Review

platform: Xiaohongshu

content_type: Carousel

pillar: Professional Knowledge

keywords:
  - PDMS
  - Rework
  - Designer
  - Delivery

summary: Explains why preventing rework creates greater project value than reducing material cost.

reading_time: 3 min

call_to_action: Follow for more delivery insights.

media:
  - Cover
  - 6 Carousel Images

knowledge_sources:
  - PDMS Framework
  - Brand Positioning

related_products:
  - BUXA PDMS

related_documents:
  - Product Delivery Framework

related_assets:
  - XHS-036
  - WEB-012

parent_asset: CORE-004

child_assets:
  - FB-041
  - LI-012

reused_by:
  - Newsletter-008

supersedes:

status: Approved

approval: Founder

reviewer: Founder

publish_date: 2026-07-20

next_review: 2027-01-20

archive_status: Active
```

---

# Metadata Naming Rules

Metadata values should be:

- consistent
- human readable
- AI readable
- reusable
- platform independent

Avoid:

- ambiguous terminology
- duplicated values
- inconsistent abbreviations
- unnecessary free text

Controlled vocabularies should be preferred whenever possible.

---

# Controlled Vocabulary

Certain metadata fields should use predefined values.

Examples include:

## Asset Type

- Knowledge Asset
- Core Content
- Website Article
- Social Post
- Video
- Presentation
- FAQ
- Case Study
- Template

---

## Status

- Draft
- Review
- Approved
- Published
- Archived

---

## Objective

- Education
- Awareness
- Consideration
- Conversion
- Retention
- Internal Training

---

## Audience

- Homeowners
- Interior Designers
- Architects
- Builders
- Developers
- Installers
- Partners

Using controlled vocabulary improves consistency and AI retrieval.

---

# Metadata Lifecycle

Metadata evolves together with content.

Typical lifecycle:

```text
Draft
        │
        ▼
Review
        │
        ▼
Approved
        │
        ▼
Published
        │
        ▼
Updated
        │
        ▼
Archived
```

Metadata should always reflect the current operational state of the asset.

---

# AI Retrieval Principles

Artificial Intelligence should retrieve content using metadata before searching document bodies.

Preferred retrieval order:

1. Governance
2. Business Metadata
3. Relationship Metadata
4. Content Metadata
5. Full-text Search

Structured retrieval improves speed, consistency and accuracy.

---

# Metadata Quality Checklist

Every approved asset should satisfy the following checklist.

✓ Identity Complete

✓ Business Purpose Defined

✓ Audience Specified

✓ Objective Recorded

✓ Knowledge Sources Linked

✓ Related Assets Identified

✓ Governance Complete

✓ Controlled Vocabulary Used

✓ Metadata Reviewed

✓ AI Retrieval Ready

---

# Relationship with Other Documents

This document supports:

- Content System Architecture
- Content Principles
- Content Workflow
- Content Standards

It enables:

- Content Library
- Platform Standards
- Performance Framework
- Future AI Agents
- Knowledge Graph

All future content assets should implement this metadata standard.

---

# Single Source of Truth

Metadata defines the structured identity of every content asset within the BUXA Content Operating System.

Business Knowledge remains the foundation.

Metadata provides structure.

Relationships provide context.

Governance provides trust.

Artificial Intelligence retrieves structured knowledge.

People govern structured knowledge.

Together they create an intelligent, scalable and reusable Content Operating System.

One Brand.

One Knowledge Base.

One Operating System.

Infinite Content.
