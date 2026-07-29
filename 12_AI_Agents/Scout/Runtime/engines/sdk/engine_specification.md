# Engine SDK Specification

Version: `1.0.0`

Status: Normative Engine Layer Contract

## 1. Foundation Dependency

Certified SDK v1 packages MUST pin `runtime-foundation-v1.0.0`. The SDK MUST NOT modify or reinterpret Foundation authorities.

## 2. Package Contract

One Engine package version consists of:

- one Manifest;
- one or more separate immutable Capability files;
- implementation code in a later phase;
- positive and negative fixtures;
- immutable conformance evidence.

Inline Capabilities are invalid for conformant or certified packages.

## 3. Identity

`engine_id`, `capability_id`, Engine type, canonical name, Decision identifiers, Runtime Object `object_type`, Runtime State names, schema IDs, and versions MUST follow the accepted ADR exactly. Human labels are non-authoritative.

Engine identity MUST NOT contain deployment data. Deployment instances, credentials, endpoints, environment, scaling, and concrete resource allocation remain outside the Manifest and Registry.

## 4. Manifest

The Manifest records stable declarative behaviour only. It defines identity, compatibility, embedded Engine Profile, Capability references, permission ceiling, maximum side-effect level, Decision obligations, Runtime Object access, State interaction, quality, security, observability, and conformance requirements.

Declarations do not grant authority. Effective permissions are the intersection of Manifest, Capability, deployment, policy, and approval.

## 5. Capability

A Capability is bounded behaviour within one canonical Engine responsibility. It is not an Engine, Runtime Object, Tool, Task type, or authority grant.

Capability permissions MUST be a subset of Manifest permissions. Capability side effects MUST NOT exceed the Manifest level. Its Engine type MUST match the Manifest and Engine ID. Referenced Capability ID and version MUST match the external file.

## 6. Runtime Objects

Certified packages may consume and produce only approved Runtime Objects, subject to Registry producer and consumer authority and schema rules. Planned objects are prohibited. Provisional references are experimental only and cannot enter certified output.

`policy_decision` and `checkpoint` remain provisional compatibility gates. The SDK MUST report rather than waive those restrictions.

## 7. State and Decision Interaction

Every State must exist in `shared/state_model.md`. Static proposals must be registered. Dynamic proposals may use only the seven governed mechanisms and require all Foundation Decisions, checkpoints, origin context, and Orchestrator validation.

Decision identifiers are closed references mapped to `shared/decision_model.md`. They do not redefine Decision semantics.

## 8. Invocation and Result

Implementations receive the released Engine Context through an SDK adapter and return the released Engine Result. New SDK v1 producers MUST emit `runtime_execution_evidence_v1`. Alpha Engine Results may be consumed for compatibility but cannot certify a new producer.

Only the Runtime Orchestrator may commit Runtime State.

## 9. Tools

The SDK declares minimal Tool metadata only. It defines no Tool protocol. Planned `tool_request` and `tool_result` Runtime Objects are unavailable until separate governance promotes them.

## 10. Registration

The Engine Registry contains exactly eleven canonical type entries. Multiple implementations may be registered under a type. Actual deployment selection remains external.

An implementation cannot be active without certified evidence and exact Foundation compatibility. The current Phase 1 Registry contains no implementations.

## 11. Conformance

Conformance MUST follow [validation_rules.md](validation_rules.md) and [conformance_suite.md](conformance_suite.md). Any failed mandatory rule blocks advancement.

