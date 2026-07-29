# Scout Runtime Engine SDK

Version: `1.0.0`

Foundation: `runtime-foundation-v1.0.0`

Status: Phase 1 SDK Contract

## Purpose

This SDK is the canonical conformance layer for Scout Runtime Engine implementations. It validates implementation identity, capability declarations, Foundation compatibility, permissions, side effects, State and Runtime Object references, Engine Result obligations, registration, and certification.

It is not an Engine, Orchestrator, Tool protocol, deployment system, Runtime registry replacement, or Foundation specification.

## Authority

The accepted [Architecture Design](../Engine_SDK_Architecture_Design.md) and [Architecture Decisions](../Engine_SDK_Architecture_Decisions.md) govern this SDK. The released Foundation remains authoritative for all Runtime semantics.

## Files

| File | Purpose |
|---|---|
| [engine_specification.md](engine_specification.md) | Normative SDK behaviour |
| [engine_manifest.schema.json](engine_manifest.schema.json) | Manifest machine contract |
| [engine_capability.schema.json](engine_capability.schema.json) | Capability machine contract |
| [engine_registry.schema.json](engine_registry.schema.json) | Registry machine contract |
| [engine_registry.json](engine_registry.json) | Eleven-type implementation registry |
| [manifest_template.json](manifest_template.json) | Complete Manifest authoring template |
| [capability_template.json](capability_template.json) | Complete Capability authoring template |
| [example_manifest.json](example_manifest.json) | Non-executable Retrieval example |
| [example_capability.json](example_capability.json) | Non-executable Retrieval Capability example |
| [validation_rules.md](validation_rules.md) | Validation algorithm and semantic checks |
| [conformance_suite.md](conformance_suite.md) | Fixtures, evidence, and certification |

## Foundation Boundary

SDK files must reference and must not redefine:

- canonical Engine taxonomy;
- Runtime State and transition semantics;
- Decision semantics;
- Engine Context;
- Engine Result;
- Runtime Objects and their Registry;
- approval and policy ownership.

## Phase 1 Limit

No Engine implementation is included. The Retrieval files are schema-valid examples only. The Registry contains no implementation entries and no default implementation.

