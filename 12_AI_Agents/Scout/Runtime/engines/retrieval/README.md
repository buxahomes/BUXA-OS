# Scout Retrieval Engine v1

Version: `1.0.0`

Package lifecycle: `validated`

Conformance status: `conformant`

Registry status: `validated`, non-default, not activation-eligible

Foundation: `runtime-foundation-v1.0.0`

SDK: `1.0.0`

## Purpose

This is the first SDK-conformant package for the canonical Retrieval Engine. It declares the contracts, capabilities, examples, fixtures, and deterministic evidence needed to acquire, filter, organise, verify, and return authorised knowledge. It contains no executable retrieval adapter.

The package consumes authorised Engine Context and Task information, preserves source identity and provenance, and normally produces Knowledge Package, Engine Result, and Runtime Event evidence. It may produce an Exception Record or Quality Report when the declared path requires one.

## Status boundaries

- Package lifecycle describes repository maturity. `validated` means the package has passed its declared checks.
- Conformance status describes recorded SDK evidence. `conformant` means automated checks passed; Engine Layer Governance has not granted `certified`.
- Registry status describes discoverability. The candidate is registered under Retrieval Engine but is neither active nor default.
- Runtime execution status belongs to one Engine Result invocation and is independent of package and Registry status.

## Capabilities

- [Authoritative Retrieval](capabilities/authoritative_retrieval.capability.json)
- [Source Discovery](capabilities/source_discovery.capability.json)
- [Source Ranking](capabilities/source_ranking.capability.json)
- [Evidence Extraction](capabilities/evidence_extraction.capability.json)
- [Retrieval Verification](capabilities/retrieval_verification.capability.json)

The capability files are immutable at version `1.0.0`. They are package-local declarations, not a second Runtime registry.

## Runtime boundaries

The Engine may operate only in `retrieval_pending`, `retrieval_running`, and `retrieval_waiting`. It may propose registered transitions related to retrieval readiness, incompleteness, waiting, failure, exception handling, retry routing, cancellation, and timeout. It never commits Runtime State; the Runtime Orchestrator validates and commits or rejects proposals.

Certified outputs use only approved Runtime Objects. `policy_decision` is an experimental reference because it remains provisional. Checkpoint, Tool Request, and Tool Result are not conformant outputs.

## Permissions and side effects

The Manifest ceiling is `read_internal`, `read_restricted`, `invoke_tool`, and `network_access`. Every permission not declared is denied. Restricted retrieval requires applicable approval and policy evidence. The package grants no publication, communication, repository modification, secret access, or irreversible side-effect permission.

The side-effect level is `none`. Retrieval may use invocation-local caches and temporary artefacts, but this package does not persist them. Evidence and provenance are returned through Foundation Runtime Objects and events. Repeated invocation is expected to be idempotent against identical immutable source versions and scope.

## Tool boundary

`scout.tool.repository_reader` and `scout.tool.source_search_adapter` are abstract dependency identifiers only. They do not define a Tool protocol, endpoint, credential, provider, adapter, or external integration. Tool Request and Tool Result remain future planned boundaries.

## Layout

- [Engine Manifest](engine_manifest.json)
- [Engine specification](retrieval_engine_specification.md)
- `capabilities/` — immutable Capability declarations
- `examples/` — canonical invocation and Decision-reference examples
- `fixtures/positive/` and `fixtures/negative/` — conformance cases
- `certification/` — evidence, checklist, and report
- [Architecture review](Retrieval_Engine_Architecture_Review.md)

## Validation

Validation covers strict JSON and duplicate keys, SDK and Foundation schemas, capability hashes and references, Registry consistency, permission inheritance, side effects, all 70 Runtime States, static transitions, dynamic mechanisms, approved Runtime Objects, ADR-03 Decision identifiers, positive and negative fixtures, Markdown, local references, and `git diff --check`.

## Non-goals and future implementation

This package does not implement search, crawling, repository access, vector retrieval, embeddings, ranking models, external APIs, Tool protocol, Orchestrator logic, deployment configuration, credentials, or background jobs. A future executable adapter must implement these declarations without changing Foundation authority and must complete governance certification and deployment approval before activation.

