# Mobile Inspection Workflow

Version: 1.0

Status: Approved

Owner: Founder

Last Updated: July 2026

Document Type: Workflow Standard

---

# Purpose

This document defines the official mobile inspection workflow used by the BUXA PDMS™ Project Delivery Management System.

The workflow is designed to minimise manual input while ensuring complete, traceable and high-quality inspection records.

The Flooring Project Engineer focuses on inspecting the project.

The system manages the workflow.

---

# Design Philosophy

Inspect.

Capture.

Confirm.

Move Forward.

The Flooring Project Engineer should spend time inspecting the project.

Not completing forms.

The mobile application should be simple, intuitive and mission-oriented.

---

# Mission-Based Experience

BUXA PDMS™ is designed around project missions rather than software menus.

The Flooring Project Engineer should never need to understand the internal system structure.

Instead of navigating through:

- Stages
- Quality Gates
- Control Points
- Inspection Standards

the system presents only the inspection tasks that currently require attention.

Each completed task automatically updates project progress.

The system manages workflow.

The Flooring Project Engineer manages the project.

---

# Core Principles

## Reality First

Inspection captures actual project conditions.

The system never creates inspection facts.

Only the Flooring Project Engineer records project conditions.

---

## Task-Based Workflow

The Flooring Project Engineer works by completing inspection tasks.

The system automatically manages:

- Project Progress
- Stage Progress
- Task Dependencies
- Quality Gate Progress
- Rectification Status

The user never manually navigates project structures.

---

## Flexible Task Management

The system organises inspections into project missions.

Within each mission, all inspection tasks that are ready to be performed are presented to the Flooring Project Engineer.

The Flooring Project Engineer may complete any available task according to actual site conditions and professional judgement.

The system automatically manages task dependencies.

Tasks that rely on unfinished work remain locked until prerequisite tasks have been completed.

This provides maximum flexibility while maintaining workflow integrity.

---

## Minimum Manual Input

The system automatically associates:

- Project
- Stage
- Quality Gate
- Control Point
- Inspection Standard
- Acceptance Criteria
- Inspector
- Date
- Time

The Flooring Project Engineer only records:

- What happened
- What was measured
- What was observed

---

## Evidence Before Documentation

Evidence is captured first.

Documentation is generated automatically.

Evidence always has higher priority than written descriptions.

---

## Exception-Based Workflow

Normal inspections remain simple.

Additional information is only requested when required.

Examples include:

- Failed inspections
- Missing evidence
- AI identifies potential risks
- Rectification required

The system requests additional information only when project conditions require it.

---

# Localisation

BUXA PDMS™ is designed as a multilingual platform.

The interface supports localisation without changing the underlying project data.

Supported interface modes include:

- Chinese
- English
- Bilingual (Chinese + English)

Inspection terminology is standardised across all languages.

Project data remains language-independent.

---

# Standard Workflow

Project

↓

Today's Mission

↓

Mission Board

↓

Select Task

↓

Capture Project Facts

↓

Capture Evidence

↓

AI Analysis

↓

FPE Decision

↓

Inspection Record Created

↓

Task Completed

↓

Quality Gate Progress Updated

---

# Step 01

## Select Project

The Flooring Project Engineer selects the assigned project.

The system automatically loads:

- Project Information
- Current Stage
- Today's Mission
- Mission Board
- Outstanding Rectifications

---

# Step 02

## Mission Board

The Mission Board is the primary workspace for the Flooring Project Engineer.

Rather than displaying project structures or navigation menus, the Mission Board presents all inspection tasks that are currently relevant to the project.

Tasks are organised by status to provide clear visibility while maintaining maximum flexibility on site.

The Flooring Project Engineer determines the execution order of all available Ready tasks.

The system automatically manages task dependencies.

### Mission Board Structure

The Mission Board consists of four task groups.

---

### Ready

Inspection tasks that can be performed immediately.

The Flooring Project Engineer may complete any Ready task.

Example

Ready (5)

① Subfloor Moisture Verification

基层含水率检查

Living Room

Estimated Time

3 min

Start →

---

② Subfloor Flatness Verification

基层平整度检查

Living Room

Estimated Time

4 min

Start →

---

③ Material Delivery Verification

材料到场检查

Start →

---

### Waiting

Tasks that cannot yet be performed because prerequisite conditions have not been satisfied.

Examples include:

- Previous inspection not completed
- Materials not delivered
- Rectification not completed
- Quality Gate not approved

Waiting tasks automatically become Ready once conditions have been satisfied.

---

### Attention Required

Tasks requiring immediate attention.

Examples include:

- Failed inspections
- Outstanding rectifications
- Critical AI recommendations
- Customer-reported issues

These tasks should normally be prioritised before routine inspections.

---

### Completed

Tasks successfully completed during the current mission.

Completed tasks remain available for project tracking and audit purposes.

---

## Mission Board Principles

The Mission Board provides project visibility without increasing operational complexity.

The Flooring Project Engineer focuses on project execution.

The system manages workflow logic.

Artificial Intelligence provides recommendations.

Project decisions remain the responsibility of the Flooring Project Engineer.

---

# Step 03

## Capture Project Facts

Depending on the inspection type, the Flooring Project Engineer may:

- Enter one or more measurements
- Select Pass / Fail / N/A
- Record observations
- Record voice notes
- Capture photographs
- Capture videos

Only the required fields are displayed.

---

# Step 04

## Flexible Measurement Collection

The system never enforces a fixed number of measurements.

The Flooring Project Engineer determines:

- Number of measurements
- Measurement locations
- Measurement strategy

based on:

- Actual site conditions
- Project complexity
- Professional judgement

Examples

One Measurement

Living Room

10.2%

or

Multiple Measurements

Living Room

10.1%

10.3%

10.0%

9.9%

The system supports unlimited measurements within one Inspection Record.

---

# Step 05

## Capture Evidence

Evidence may include:

- Photo
- Video
- Instrument Reading
- Voice Note
- Drawing Markup

Evidence is automatically linked to the Inspection Record.

Project, time and inspector information are automatically recorded.

---

# Step 06

## AI Analysis

Where applicable, Artificial Intelligence analyses:

- Images
- Measurements
- Instrument readings

AI may provide:

- Suggested Result
- Confidence Score
- Risk Warning
- Suggested Additional Measurements
- Suggested Task Priority
- Possible Defect Identification

AI recommendations are advisory only.

---

# Step 07

## FPE Decision

The Flooring Project Engineer reviews:

Inspection Facts

↓

Evidence

↓

AI Recommendation

↓

Professional Judgement

↓

Final Decision

The Flooring Project Engineer remains accountable for every inspection decision.

---

# Step 08

## Inspection Record Created

After submission, the system automatically creates:

- Inspection Record
- Evidence Links
- Audit Trail
- Project History
- Quality Gate Progress

Manual report writing is never required.

---

# Step 09

## Exception Workflow

If the inspection passes:

- Task Status = Completed

The remaining Ready tasks continue to be available.

If the inspection fails:

The system automatically:

- Creates a Rectification Item
- Assigns the responsible party
- Links supporting evidence
- Schedules re-insspection
- Updates project progress

The Flooring Project Engineer confirms the corrective action.

---

# Quality Gate Progress

The system continuously monitors project progress.

Example

Stage 04

Installation Preparation

Mission Progress

Ready

5

Waiting

2

Attention Required

1

Completed

8

Quality Gate Progress

90%

Outstanding Issues

1

Critical Issues

0

Quality Gate approval becomes available only after:

- All mandatory tasks are completed
- All critical issues are closed
- Required evidence has been submitted

---

# AI Assisted Inspection

Artificial Intelligence may:

- Analyse photographs
- Read instrument values
- Analyse measurements
- Detect potential defects
- Recommend additional inspections
- Suggest task priorities
- Suggest rectification actions
- Generate inspection summaries

Artificial Intelligence shall never:

- Modify inspection facts
- Approve inspections
- Close rectifications
- Approve Quality Gates

Artificial Intelligence continuously learns from completed projects.

The Flooring Project Engineer remains accountable for every inspection decision.

---

# Workflow Principles

Every inspection shall:

Capture facts.

Capture evidence.

Generate one Inspection Record.

Support future auditing.

Support AI learning.

Support automatic report generation.

The Flooring Project Engineer focuses on today's mission.

The system manages the workflow.

---

# Future Expansion

Future versions may support:

- Offline Inspection
- QR Code Project Identification
- Bluetooth Instrument Integration
- Automatic Measurement Import
- BIM Location Mapping
- Voice-First Inspection
- Wearable Device Support
- AI Task Prioritisation
- Multi-language Expansion
- Real-Time Collaboration

without changing the workflow.

---

# Single Source of Truth

The Mobile Inspection Workflow defines the official inspection workflow for every BUXA PDMS™ project.

The Flooring Project Engineer completes project missions.

The system manages workflow.

Inspection Records are generated automatically.

Reports are generated automatically.

Artificial Intelligence provides recommendations.

The Flooring Project Engineer remains accountable for every inspection decision.
