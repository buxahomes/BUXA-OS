# Shared Foundation Architecture Review

**Review scope:** `engine_contract.md`, `engine_context.md`, `state_model.md`, and `decision_model.md`  
**Review type:** Architecture consistency review  
**Date:** 2026-07-28  
**Status:** Review report; no source documents modified

## Executive Summary

The four shared foundation documents establish a strong conceptual runtime architecture: immutable Engine Context, structured Engine Result, Orchestrator-controlled state commitment, evidence-backed decisions, explicit governance gates, append-only history, and clear separation between Engine ownership and global state authority. The documents are individually detailed and broadly aligned in intent.

The architecture is not yet safe to implement as a single normative contract. The principal defect is that the Engine Contract defines a mandatory state sequence containing five states that are absent from the State Model's canonical registry. Additional high-severity inconsistencies affect the Engine Context envelope, state terminal semantics, Engine Result fields, Decision required fields, and the boundary between Engine invocation status and Runtime State. These conflicts could cause conforming implementations to reject one another while each claims compliance with an approved document.

All 23 fenced JSON blocks parse successfully as JSON. Their syntax is sound, but several examples are not semantically consistent with the required fields and canonical names defined elsewhere in the same architecture. Cross-references resolve at the filename level, but they are not sufficiently precise or version-bound for four mandatory foundation specifications.

The recommended approach is to make the State Model the sole owner of state names and transitions, make Engine Context the sole owner of the context envelope, add or designate authoritative schemas for shared Runtime Objects, and then reduce the Engine Contract and Decision Model to references plus genuinely universal rules.

## Overall Score (/100)

**62/100 — Conceptually coherent, but not implementation-ready as a normative shared foundation.**

| Review dimension | Assessment |
|---|---|
| Terminology consistency | Partial |
| Runtime Object consistency | Weak |
| Cross-reference integrity | Partial |
| State ↔ Decision consistency | Weak |
| Engine Contract consistency | Weak |
| Context consistency | Weak |
| JSON example consistency | Partial; syntax valid, semantics inconsistent |
| Schema naming consistency | Weak |
| RFC-2119 language consistency | Partial |
| Duplicate definitions | Material duplication present |
| Contradictory definitions | Material contradictions present |
| Missing references | Several |
| Broken references | No missing target files; semantic references are broken |
| Directory consistency | Partial |
| Overall Runtime architecture consistency | Partial |

## Critical Findings

### C-01 — The Engine Contract mandates non-canonical Runtime states

**Dimensions:** State ↔ Decision consistency; Engine Contract consistency; contradictory definitions; overall Runtime architecture consistency  
**Evidence:** `engine_contract.md` §14.2, lines 767–792; `state_model.md` §§7–8, lines 204–349

The Engine Contract declares a mandatory progression containing:

- `approval_ready_or_not_required`
- `skill_output_ready`
- `quality_checked`
- `execution_completed`
- `memory_updated_or_not_required`

None of these names appears in the State Model's canonical state registry. The State Model instead defines states including `approval_pending`, `approval_granted`, `execution_result_ready`, `quality_passed`, `workflow_completed`, `memory_updated`, `memory_rejected`, and `memory_deferred`.

Because the Engine Contract says Engines MUST follow its order while the State Model prohibits unregistered transitions, no implementation can comply with both specifications. This is a broken semantic cross-reference even though both files exist.

## High Findings

### H-01 — The universal Engine Context has two incompatible shapes

**Dimensions:** Context consistency; Runtime Object consistency; JSON example consistency; schema naming consistency  
**Evidence:** `engine_contract.md` §7.2, lines 359–388; `engine_context.md` §§6–12, lines 133–295

The Engine Contract's recommended context uses `engine`, `execution.attempt`, and `inputs`. The canonical Engine Context uses `receiving_engine`, `execution.attempt_number`, and `runtime_objects`, and also requires a separate `task` section. The Contract example omits required context metadata including `schema_name`, `schema_version`, `context_version`, `created_by`, `context_status`, `source`, `checksum`, `classification`, and `retention_class`.

An Engine built against the Contract example will not produce or consume the canonical envelope defined by Engine Context.

### H-02 — Engine Result requirements are internally inconsistent and have no named authoritative schema

**Dimensions:** Engine Contract consistency; Runtime Object consistency; JSON example consistency; schema naming consistency  
**Evidence:** `engine_contract.md` §§8.1–8.3, lines 415–487; Runtime schema directory

The Contract requires a `primary output` and separately requires `confidence`, but its example uses `primary_output` and nests confidence inside `decision`. The success-completeness rule then requires `output`, a field not present in the example. The architecture also distinguishes `Engine Result` from `Execution Result`, but the schema directory contains `execution_result.schema.json` and no `engine_result.schema.json`.

The result returned by every Engine therefore lacks one unambiguous field contract and schema identity.

### H-03 — `workflow_completed` is classified as both terminal and non-terminal

**Dimensions:** State consistency; contradictory definitions; terminology consistency  
**Evidence:** `state_model.md` §8.10, lines 342–349; §33, lines 882–898

The Core State Registry lists `workflow_completed` under “Terminal States.” Section 33 excludes it from the terminal-state list and explicitly says it may allow administrative closure before the normal terminal state `workflow_closed`.

This contradiction changes whether transitions may legally leave `workflow_completed`, affecting closure, archival, replay, cancellation, and audit behaviour.

### H-04 — A canonical State JSON example permits an unregistered `blocked` state

**Dimensions:** JSON example consistency; State consistency; broken semantic references  
**Evidence:** `state_model.md` §5, lines 130–163; §8, lines 250–349

The canonical Runtime State example includes `blocked` in `allowed_transitions`, but the canonical registry contains only domain-specific blocked states such as `business_judgement_blocked`, `planning_blocked`, and `policy_blocked`. There is a `blocked` category, but no canonical top-level state named `blocked`.

The example therefore violates the State Model's own rule that target states must be registered.

### H-05 — Canonical Decision JSON examples do not conform to the Decision Model's required identity contract

**Dimensions:** Decision consistency; JSON example consistency; Runtime Object consistency  
**Evidence:** `decision_model.md` §9, lines 297–330; §36, lines 1141–1159; §47, lines 1434–1812

Section 9 requires every material Decision to include fields such as `decision_version`, `created_at`, `created_by`, `owner`, `authority_level`, `related_object_ids`, `current_state`, `decision_basis`, `selected_outcome`, `confidence`, `risk`, `approval`, and `validation`. Most specialised “Canonical JSON Examples”—Approval, Retry and Recovery, and State Transition Decisions—omit many of those required fields. Several also omit Engine Contract minimum fields such as assumptions, policy references, reason summary, or review requirement.

The examples may be intended as fragments, but they are labelled canonical and are not identified as partial payloads. Implementers cannot determine whether the universal Decision contract or the specialised example governs.

### H-06 — Engine invocation statuses and Runtime states are not cleanly separated

**Dimensions:** Terminology consistency; State ↔ Decision consistency; Engine Contract consistency  
**Evidence:** `engine_contract.md` §10, lines 557–635; `state_model.md` §§6–8 and §§19–33

The Engine Contract says every invocation MUST “terminate” in a terminal or waiting status, but its allowed list includes the non-terminal statuses `ready` and `running`. It also uses `timeout`, while the State Model uses `timed_out`, and uses generic `blocked`, `failed`, and `cancelled` values alongside similarly named Runtime states and categories.

These may legitimately be separate enums, but the documents do not define the mapping or explicitly prohibit using an Engine status as a Runtime state. That omission invites invalid transition proposals and ambiguous event payloads.

## Medium Findings

### M-01 — Universal definitions are duplicated instead of owned by one specification

**Dimensions:** Duplicate definitions; contradictory definitions; overall Runtime architecture consistency  
**Evidence:** `engine_contract.md` §§7, 14, and 15; corresponding definitions throughout `engine_context.md`, `state_model.md`, and `decision_model.md`

The Engine Contract restates the Context structure, mandatory State progression, and Decision minimum fields. The specialised documents then define those concepts again in greater detail. The duplicated State definition has already diverged critically, and the Context and Decision definitions have also drifted.

The Contract should define the obligation to use these objects and delegate their field-level semantics to their authoritative specifications.

### M-02 — Shared foundation Runtime Objects do not have a complete schema set

**Dimensions:** Schema naming consistency; Runtime Object consistency; missing references  
**Evidence:** `engine_context.md` §§6 and 44; `state_model.md` §§5 and 14; `decision_model.md` §§9 and 47; `Runtime/schemas/`

The documents name logical schemas `engine_context`, `runtime_state`, and `state_transition`, and require structured Decisions and Engine Results. No corresponding `engine_context.schema.json`, `runtime_state.schema.json`, `state_transition.schema.json`, `decision.schema.json`, or `engine_result.schema.json` exists in the schema directory.

This review does not require those schemas to exist, but the specifications do not identify whether the absence is intentional, planned, or satisfied by another schema. Compatibility and validation requirements therefore lack concrete schema targets.

### M-03 — Runtime Object naming is inconsistent around approval and results

**Dimensions:** Terminology consistency; Runtime Object consistency; schema naming consistency  
**Evidence:** `engine_context.md` §12, lines 274–299; `engine_contract.md` §§8 and 28; `decision_model.md` §§24 and 47.3

Engine Context lists an `Approval Record`; the Engine Contract capability matrix assigns the Approval Engine an `Approval Decision`; and the Decision Model example contains both a Decision and an `approval_id`. Similarly, `Engine Result` and `Execution Result` are both Runtime Objects, but their relationship is not formally defined.

These names may represent distinct layers, but their identity, ownership, containment, and schema relationships are missing.

### M-04 — Cross-references are file-level only and are not version-bound

**Dimensions:** Cross-reference integrity; missing references; broken references  
**Evidence:** dependency metadata in `engine_context.md`, `state_model.md`, and `decision_model.md`

The referenced files exist and relative filenames resolve correctly. However, normative dependencies do not identify required document versions, section anchors, schema IDs, or compatibility ranges. The Engine Contract also does not declare the specialised foundation documents as dependencies even though it normatively relies on their concepts.

This makes change impact and compatibility validation difficult, particularly because all four documents are approved version `1.0.0`.

### M-05 — Context metadata requirements conflict with the canonical logical example

**Dimensions:** Context consistency; JSON example consistency  
**Evidence:** `engine_context.md` §§6–7, lines 133–199

The canonical logical structure omits required fields declared immediately afterward: `created_by`, `context_status`, `source`, `checksum`, `classification`, and `retention_class`. The later enterprise example includes them, but the first example remains an incomplete object without being labelled partial.

### M-06 — State lifecycle diagrams omit mandatory governance decisions and contain an unclear stage sequence

**Dimensions:** State ↔ Decision consistency; Context consistency; overall Runtime architecture consistency  
**Evidence:** `state_model.md` §7, lines 204–247; §§21–23; `decision_model.md` §35

The canonical happy-path lifecycle omits policy and approval states even though entry and transition rules require their outcomes where applicable. It also moves from `execution_blueprint_ready` directly to `execution_running` without showing the conditional approval/policy path and contains an extra standalone down-arrow before `workflow_completed`.

Optional branches are acknowledged in prose, but there is no canonical mapping showing where the governance Decisions attach to the happy path.

### M-07 — RFC-2119-style language is similar but not governed as one vocabulary

**Dimensions:** RFC-2119 language consistency; duplicate definitions  
**Evidence:** §3 of all four documents

Each document repeats a slightly different normative-language definition. None cites RFC 2119 or RFC 8174, defines whether uppercase-only usage is normative, or consistently states the exception requirements for `SHOULD` and `SHOULD NOT`. `MUST` and `SHALL` are treated as synonyms, increasing the normative vocabulary without adding meaning.

The practical intent is clear, but automated linting and audit interpretation cannot rely on one shared rule set.

### M-08 — The shared foundation's parent directory conflicts with repository-level AI architecture documentation

**Dimensions:** Directory consistency; cross-reference integrity  
**Evidence:** actual path `12_AI_Agents/Scout/Runtime/engines/shared/`; root `README.md` sections “Root Directory” and “11_AI”

The four reviewed files are mutually consistent in location and naming. However, the repository-level architecture still states that the AI Runtime is defined within `11_AI`, while the reviewed implementation is under `12_AI_Agents/Scout/Runtime`. The root directory examples also use governance filenames that no longer match the current root filenames.

This is outside the four-file correction scope, but it weakens discoverability and authority resolution for the shared foundation.

## Low Findings

### L-01 — The Context category list and canonical tree use different labels

**Dimensions:** Terminology consistency; Context consistency  
**Evidence:** `engine_contract.md` §7.1; `engine_context.md` §6

The Contract uses “input runtime objects,” “time limits,” and “approval context,” while the Context document uses `runtime_objects`, a combined `limits` section, and `approval`. The concepts are recognisable but not presented as exact canonical names.

### L-02 — Engine names and Runtime Object names mix prose casing with identifier casing without a stated conversion rule

**Dimensions:** Terminology consistency; schema naming consistency  
**Evidence:** throughout all four documents

The documents alternate between names such as “Business Judgement Engine,” `business_judgement_engine`, “Engine Result,” and inferred schema/file forms such as `engine_result`. A short naming convention would make display names, IDs, object types, schema names, and filenames predictably related.

### L-03 — JSON validation certificates prove syntax only, not architectural conformance

**Dimensions:** JSON example consistency; cross-reference integrity  
**Evidence:** appendices of `engine_context.md`, `state_model.md`, and `decision_model.md`

The certificates accurately indicate syntactic validity, but their prominent placement may imply broader validation. The examples still contain missing required fields, non-canonical values, and cross-document mismatches. The certificate scope should explicitly say “JSON syntax only” and identify schema/semantic validation as not performed where no schema exists.

### L-04 — No explicit glossary exists for shared Runtime terms

**Dimensions:** Terminology consistency; duplicate definitions; missing references  
**Evidence:** terms distributed across all four documents

Core terms including Runtime Object, Engine Result, Execution Result, Decision, State, status, Context, checkpoint, snapshot, approval, and policy decision are defined or implied in multiple places. A compact shared terminology section or referenced runtime glossary would reduce local redefinition without changing the enterprise Glossary.

## Recommended Fixes

1. Make `state_model.md` the sole authority for every Runtime state name, category, transition, and terminal semantic. Replace the Engine Contract's state list with a versioned reference to the canonical registry.
2. Resolve `workflow_completed` as either non-terminal readiness/administrative state or a terminal state, then align its category, allowed transitions, and examples.
3. Remove `blocked` from the canonical State example or register and define a generic `blocked` state. Prefer domain-specific states if they are the intended architecture.
4. Make `engine_context.md` the sole authority for Engine Context field names. Update the Contract example to use `receiving_engine`, `attempt_number`, `runtime_objects`, `task`, and all required metadata, or explicitly label it as non-normative pseudostructure.
5. Define one authoritative Engine Result object and schema. Clarify its relationship to Execution Result, and standardise `primary_output` versus `output` and the location of `confidence`.
6. Define whether Decision examples are complete objects, embedded payloads, or fragments. If complete, add all universal required fields; if embedded, specify the containing object and inherited fields.
7. Add or formally defer schemas for Engine Context, Engine Result, Runtime State, State Transition, and Decision. Reference exact schema IDs and compatibility versions from the documents.
8. Publish a mapping between Engine invocation status, Decision status, Runtime State, Context status, validation status, approval status, and integrity/freshness status. State explicitly that values from one enum cannot substitute for another.
9. Standardise Runtime Object names and relationships, especially Approval Record versus Approval Decision and Engine Result versus Execution Result.
10. Replace duplicated normative sections with authoritative cross-references. Retain only the rules genuinely owned by each document.
11. Adopt one normative-language section based on RFC 2119 as clarified by RFC 8174, including uppercase-only applicability and one exception process. Reference it from the other documents.
12. Add versioned dependencies with section anchors and schema references. Include reciprocal impact notes where a foundation change can break another document.
13. Align the root repository architecture with the active `12_AI_Agents/Scout/Runtime` directory, subject to the required human architecture approval.
14. Add automated validation for duplicate JSON keys, required properties, enum membership, state-transition registry membership, cross-document identifiers, and Markdown references—not JSON syntax alone.

## Suggested Document Update Order

1. **`state_model.md`** — establish the authoritative state registry, terminal semantics, transition rules, and status-to-state boundary first.
2. **`engine_context.md`** — finalise the canonical input envelope after state fields and state-version semantics are stable.
3. **`decision_model.md`** — align Decision state references, required fields, specialised payloads, approval terminology, and canonical examples.
4. **`engine_contract.md`** — update last so the universal contract references the final authoritative Context, State, and Decision specifications without duplicating them.
5. **Shared Runtime schemas and schema README** — create or align schemas immediately after the four specifications stabilise, then validate every example against them.
6. **Runtime and repository navigation documents** — reconcile `11_AI` versus `12_AI_Agents/Scout/Runtime` only after explicit architecture approval.
