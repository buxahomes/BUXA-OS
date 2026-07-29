# Retrieval Engine Specification

Version: `1.0.0`

Status: Validated implementation contract; no executable adapter

## 1. Purpose

The canonical Retrieval Engine acquires, filters, organises, and returns authorised knowledge required by downstream Engines. It preserves evidence and uncertainty; it does not create Business Knowledge or make final Business Judgement.

## 2. Scope

The package covers discovery, authorised retrieval, source ranking, evidence extraction, freshness and completeness assessment, provenance preservation, retrieval verification, waiting, incomplete results, retry proposals, and exception reporting. Production connectors, Tool protocol, Orchestrator behaviour, deployment, and external integrations are out of scope.

## 3. Normative dependencies

- [Engine Contract](../shared/engine_contract.md) owns Engine taxonomy and Engine Result semantics.
- [Engine Context](../shared/engine_context.md) owns all Engine Context structure and integrity.
- [State Model](../shared/state_model.md) owns Runtime States, transitions, dynamic targets, and terminal semantics.
- [Decision Model](../shared/decision_model.md) owns Decision types and semantics.
- [Runtime Objects](../shared/runtime_objects.md) and the [Runtime Object Registry](../../registry/runtime_objects.json) own Runtime Object architecture.
- [Engine SDK ADR](../Engine_SDK_Architecture_Decisions.md) and the [SDK](../sdk/README.md) own package conformance.

This specification references those authorities and does not redefine them.

## 4. Engine identity

`scout.engine.retrieval.default` is implementation identity for canonical Engine type `retrieval`, version `1.0.0`. It contains no deployment, vendor, endpoint, model, credential, region, or instance identity.

## 5. Capability architecture

Authoritative Retrieval controls authority-ordered acquisition. Source Discovery locates candidates without verification. Source Ranking orders retrieved sources without changing their authority. Evidence Extraction creates claim-linked locators. Retrieval Verification assesses coverage, conflicts, provenance, freshness, and scope. The boundaries are complementary and non-overlapping.

## 6. Invocation inputs

An invocation receives Foundation invocation identity, validated Engine Context, current Runtime State and version, authorised Task and Runtime Object references, Decision and governance references, cancellation and timeout signals, abstract Tool availability, resource limits, and trace identifiers. Hidden memory and undeclared sources are prohibited.

## 7. Runtime Object boundaries

Approved inputs and references are declared in the Manifest. Retrieval produces only `knowledge_package`, `engine_result`, and `runtime_event`. It may reference `exception_record` created by the Exception Engine or Runtime Orchestrator and `quality_report` created by the Quality Engine; it never assumes their producing authority. Decision identity is referenced rather than embedded. Provisional `policy_decision` is experimental only. Provisional or planned objects—including checkpoint, Tool Request, and Tool Result—are not conformant outputs.

## 8. Runtime State interaction

The package uses only registered States. The normal path is `retrieval_pending` to `retrieval_running`, then `knowledge_ready` or `knowledge_incomplete`. Local waiting uses `retrieval_running` to `retrieval_waiting`, and resumption uses the registered `retrieval_waiting` to `retrieval_running` edge in a new invocation. Failure, exception, cancellation, and timeout follow registered static edges. Governed retry uses `retry_target`; it cannot jump directly from a failure to an operational stage.

An Engine Result proposes a transition. Only the Runtime Orchestrator validates and commits it. No invocation status is a Runtime State.

## 9. Decision interaction

The Engine may produce or reference only ADR-03 identifiers declared in the Manifest. It may propose selection, prioritisation, proceed, conditional proceed, request-more-knowledge, or escalation Decisions within Retrieval authority. It may consume Approval, Policy, Retry, and Cancellation Decisions. It cannot grant its own approval, policy authority, transition commit, recovery, rollback, or final Business Judgement.

## 10. Permission model

Default deny applies. Capabilities can use only the Manifest ceiling. Internal reads are ordinary; restricted reads require applicable Approval and Policy evidence. Network and Tool access are limited to declared abstract dependencies and invocation grants. Publication, external communication, repository mutation, secrets, and irreversible effects are denied.

## 11. Side-effect model

The package level is `none`. Invocation-local caching and temporary parsing artefacts are allowed only when discarded. Persisted knowledge and provenance are represented through approved Runtime Objects returned as evidence. Repeated calls against identical source versions, scope, and criteria must be idempotent.

## 12. Source authority

Human-readable source classes, highest to lowest when applicable, are: normative repository authority, approved internal knowledge, connected private source, official external primary source, reliable secondary source, unverified external source, user-provided source, and historical provenance source. Classification is metadata inside existing knowledge structures, not a new Runtime Object. Origin is immutable; retrieval must never silently upgrade authority.

## 13. Retrieval quality

Quality evaluation covers authority, provenance, exact identity, freshness, completeness, relevance, duplication, contradiction, unsupported inference, confidence, citation integrity, scope, and requested-source fidelity. Statuses must distinguish retrieved, verified, authoritative, disputed, stale, incomplete, and unavailable. Retrieval does not prove every claim true; verification and authority evidence must support that conclusion separately. Existing Quality Report structures are referenced, not redefined.

## 14. Waiting behaviour

`waiting` is final for one invocation when an external dependency may later become available. The Engine Result records the dependency, reason, resumability, available evidence, timeout impact, and proposed `retrieval_waiting`. Resumption creates a new invocation and proposes the registered local return to `retrieval_running`.

## 15. Incomplete retrieval

Partial authorised knowledge is preserved in a Knowledge Package. Missing material distinguishes unavailable from not found, the result uses `partial_success`, and the proposal targets `knowledge_incomplete`. It never claims completion or uses a generic `blocked` State.

## 16. Success behaviour

Success requires valid invocation identity, authorised Context, a declared Capability, a Knowledge Package reference, provenance evidence, complete validation records, no undeclared side effect, `succeeded`, and a registered proposal from `retrieval_running` to `knowledge_ready` under `runtime_execution_evidence_v1`.

## 17. Failure behaviour

Source unavailability may wait or produce incomplete knowledge; permission denial routes through governance or Exception handling; invalid references, adapter failure, malformed material, and evidence-validation failure are classified explicitly. Failed results preserve evidence, produce or reference an Exception Record when material, and use registered routing.

## 18. Cancellation

An authorised cancellation returns `cancelled`, emits cancellation evidence, preserves already acquired evidence where permitted, and proposes the registered `cancelled` State. Cancellation is not failure and does not imply State commitment.

## 19. Timeout

Timeout returns `timeout`, a failure category of `timeout`, timeout event evidence, and a proposal to `timed_out`. Any retry requires a later canonical Retry Decision.

## 20. Retry

Retry is bounded to the Capability maximum, preserves execution identity with a new attempt identity, and requires `scout.decision.retry`. Operational return uses `retry_pending` and `retrying`, then the `retry_target` dynamic mechanism with registered target, Decision evidence, origin context, and Orchestrator validation. Blind or indefinite retry is prohibited.

## 21. Tool boundary

Capabilities declare abstract Tool dependencies with ID, interface version, permissions, side-effect level, contracts, timeout, and retry ceiling. A Tool is not a Capability, Runtime Object, connected source, or external adapter. This package defines none of those interfaces or implementations.

## 22. Evidence and provenance

Every material retrieval conclusion references source identity, location, authority, retrieval time, freshness, verification, limitation, and applicable claim or requirement. Contradictory and excluded evidence remains traceable. Engine Result evidence correlates invocation, Decisions, transition proposal, Runtime Events, collected objects, and validation.

## 23. Security

Context-scoped access, least privilege, default deny, Tool intersection, audit correlation, and explicit restricted-access gates are mandatory. Credentials and secrets never enter Manifest, Capability, example, or evidence files.

## 24. Privacy

The Engine minimises retrieved personal data, honours Context privacy constraints, records applicable classification, and excludes unauthorised material. Retrieval scope never implies permission to disclose.

## 25. Observability

Every invocation correlates start, completion, waiting/failure/cancellation/timeout where applicable, and transition proposal events. Required metrics include duration, source counts, and coverage. Logs do not replace Runtime evidence.

## 26. Conformance

Conformance requires exact Foundation and SDK pins, schema-valid Manifest and Capabilities, verified hashes and references, registered States and transitions, approved certified outputs, mapped Decisions, permission inheritance, correct side effects, passing positives, intended negative failures, valid Markdown, and deterministic evidence.

## 27. Compatibility

This version is tested only against `runtime-foundation-v1.0.0`, Foundation commit `2eb52d62d877dd8b5c5736721423dc1b2d5b5d36`, SDK `1.0.0`, and schema versions `1.0.0`. It emits `runtime_execution_evidence_v1`; Alpha-only output is not SDK v1 conformant.

## 28. Known limitations

There is no executable adapter, Tool implementation, deployment configuration, external certification, or production activation. Policy Decision and Checkpoint remain provisional Foundation objects and cannot be certified outputs. Abstract Tool contract URIs are declarations only.

## 29. Future implementation

A later adapter may implement repository and connected-source access behind the declared Tool boundary. It must preserve these ceilings, add implementation tests, obtain Engine Layer Governance certification, receive deployment approval, and then update Registry lifecycle without changing Foundation semantics.

## 30. Acceptance criteria

Acceptance requires a schema-valid Manifest, five schema-valid capabilities, exact content hashes, coherent non-active Registry registration, seven conformant Engine Result examples, mapped Decisions, approved Runtime Objects, valid State proposals, default-deny permissions, side-effect `none`, eight passing positive fixtures, fifteen intended negative failures, deterministic evidence, no Critical or High architecture findings, and no Foundation modification.
