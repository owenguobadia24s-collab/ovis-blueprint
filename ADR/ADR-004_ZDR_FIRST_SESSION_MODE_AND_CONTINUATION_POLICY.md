# ADR-004 — ZDR-First Session Mode and Continuation Policy

## Status
Accepted

## Date
2026-03-15

## Owner
OVIS

---

## Context

ADR-001 established that canonical OVIS authority lives in OVIS-defined objects, lineage relations, append-only Events, and storage-agnostic state rather than in any external runtime surface.

ADR-002 established function calling as the canonical capability bus for executable OVIS-native actions.

ADR-003 established the Responses API as the canonical runtime substrate and the OVIS runtime wrapper as the only canonical model invocation path.

What remains undecided is the default governed session posture for runtime usage, including:

- whether OVIS is ZDR-first by default
- which continuation patterns are canonical
- when OpenAI-managed durable state is allowed, optional, or prohibited
- how branch continuity should be preserved across response chaining and compaction

The existing architecture and task extraction materials repeatedly indicate that:

- OVIS-owned state must remain authoritative
- OpenAI-managed durable state is optional or derived, not sovereign
- ZDR-sensitive and retention-sensitive contexts require lower-retention runtime patterns
- continuation must remain compatible with branch continuity and future compaction behavior

Therefore OVIS requires an explicit session-mode and continuation policy for runtime usage.

---

## Decision

OVIS shall be ZDR-first by default for governed runtime usage.

The default canonical session posture is:

- OVIS-owned canonical state is authoritative
- OpenAI-managed durable state is not authoritative
- runtime continuation should prefer explicit response chaining with minimal retained OpenAI state
- session behavior must preserve branch continuity without transferring authority to runtime-managed durable state

This means the default governed runtime direction is:

- `store=false` or equivalent low-retention posture where supported
- explicit continuation handling through canonical runtime wrapper logic
- canonical branch continuity maintained in OVIS-owned state

Durable OpenAI-managed state is optional only in approved cases and is never canonical by default.

---

## Canonical Default Session Posture

The canonical default session mode for governed OVIS work is ZDR-first.

ZDR-first means:

- prefer the runtime posture with the fewest retention surprises
- keep canonical branch and object state in OVIS-owned storage
- minimize reliance on OpenAI-managed retained context
- treat OpenAI-managed session state as supplemental rather than authoritative

This default exists because OVIS governance, audit, and lineage requirements are stricter than the assumptions of a convenience-first retained session model.

ZDR-first is the canonical default even if some deployments later choose a more permissive mode.

---

## Canonical Continuation Pattern

The canonical continuation pattern is explicit response chaining mediated by the runtime wrapper.

Minimum continuation expectations:

- continuation must be explicit rather than implicit
- continuation identifiers must be runtime-managed and linkable to canonical OVIS branch context where material
- continuation must not redefine branch authority or object authority
- continuation must remain compatible with function-called capability mediation from ADR-002

Where supported and appropriate, the canonical continuation direction is response chaining using patterns such as `previous_response_id`.

Continuation through opaque retained product state alone is non-canonical.

---

## Allowed, Optional, and Prohibited Runtime Session Patterns

### Canonical

The following runtime/session pattern is canonical by default:

- Responses API through the OVIS runtime wrapper
- ZDR-first posture
- OVIS-owned canonical state
- explicit continuation chaining
- function-called capability mediation

### Optional

The following patterns are optional where governance permits and a later implementation profile or deployment policy allows them:

- durable OpenAI-managed state as a convenience layer
- Conversations-style mappings or equivalent retained runtime state adapters
- Background mode for approved non-ZDR workflows
- WebSocket mode where useful for governed continuation and consistent with deployment policy

Optional means:

- allowed only as an explicit deployment choice
- never authoritative for canonical OVIS state
- subject to retention, policy, and audit review

### Prohibited by Default

The following are prohibited as default governed runtime posture:

- making durable OpenAI-managed state the authoritative work object or branch store
- relying on implicit retained session state as the sole continuity mechanism
- using product UX session behavior as canonical infrastructure
- adopting higher-retention session modes as the default without explicit approval

---

## OpenAI-Managed Durable State Policy

OpenAI-managed durable state is classified as follows:

### Prohibited

OpenAI-managed durable state is prohibited when:

- governance requires ZDR-first handling
- retention assumptions are incompatible with the deployment context
- the workload involves canonical branch continuity that cannot safely depend on retained OpenAI state
- the runtime mode would blur the distinction between derived context and canonical OVIS authority

### Optional

OpenAI-managed durable state is optional when:

- a deployment explicitly allows it
- retention and compliance implications are accepted
- OVIS-owned state remains authoritative
- the runtime wrapper continues to maintain canonical branch/object linkage outside the retained session

### Allowed but Non-Canonical

Even when allowed, durable OpenAI-managed state remains:

- convenience state
- derived state
- cached continuation state

It does not become sovereign OVIS state unless a later ADR explicitly says otherwise.

---

## Branch Continuity and OVIS-Owned State

Branch continuity belongs to OVIS-owned state, not to runtime-managed durable state.

This means:

- each governed branch must remain representable through ADR-001 branch objects and related canonical objects
- runtime continuation identifiers may assist branch progression, but do not replace branch identity
- canonical progress across Signal, Work Object, Plan Job, Approval, Execute Job, Bridge Action, and Event objects must remain anchored outside retained runtime state

Branch continuity must therefore remain recoverable even if:

- retained runtime state is unavailable
- runtime sessions expire
- deployment mode changes

This is required for portability, auditability, and long-running governed work.

---

## Branch Continuity and Compaction

Compaction is a branch-governance concern, not a reason to transfer authority to retained runtime state.

The session policy must therefore preserve compatibility with future compaction by ensuring that:

- branch continuity is reconstructible from OVIS-owned state
- continuation artifacts can be associated with branch context
- compacted branch state can be used to resume downstream planning or execution without depending on opaque runtime retention

In practical terms:

- runtime continuation should remain disposable or replaceable
- compacted branch state should remain the governed continuity artifact
- retained runtime state must not become the only usable memory of a branch

---

## Relationship to ADR-001

ADR-001 defines canonical state, lineage, branch identity, and compaction record concepts.

ADR-004 preserves ADR-001 by requiring that:

- canonical state remains OVIS-owned
- branch identity remains outside runtime-managed durable state
- continuation state is linkable but non-authoritative
- append-only audit and lineage remain reconstructible without trusting retained runtime state as sovereign

If a session pattern cannot preserve these properties, it is not canonical for governed use.

---

## Relationship to ADR-002

ADR-002 defines function calling as the canonical capability bus.

ADR-004 therefore requires that continuation and session handling do not bypass that action boundary.

This means:

- retained runtime context must not silently turn into an ungoverned action path
- continuation across turns must still mediate executable actions through function-called capabilities
- session convenience must not weaken policy, schema, or idempotency expectations

---

## Relationship to ADR-003

ADR-003 defines the Responses API as the canonical runtime substrate and the runtime wrapper as the only canonical invocation path.

ADR-004 narrows the default operating posture for that runtime by fixing:

- ZDR-first as the default governed mode
- explicit continuation chaining as the canonical continuation pattern
- durable OpenAI-managed state as optional, deployment-scoped, and non-authoritative

ADR-003 and ADR-004 must be read together for runtime design.

---

## Immediate Consequences

1. OVIS runtime design must assume ZDR-first as the default governed session posture.
2. Runtime continuation work must prioritize explicit response chaining over implicit retained durable session behavior.
3. OVIS-owned state must remain the continuity anchor for branches and governed objects.
4. OpenAI-managed durable state may be supported only as an optional deployment mode, not as the default canonical posture.
5. Future compaction design must assume branch continuity can be reconstructed without relying on retained runtime state.
6. Runtime implementation must distinguish clearly between canonical session patterns and optional convenience modes.

---

## Deferred to Follow-on ADRs

The following are intentionally deferred:

- exact session mode matrix by deployment class
- exact runtime wrapper API for continuation storage and lookup
- exact approval requirements for enabling durable OpenAI-managed state
- exact Background mode and WebSocket eligibility rules
- exact retention profile table for supported tools and runtime options
- exact compaction-trigger interaction with continuation checkpoints

These will be defined in dependent ADRs and implementation specifications.

---

## Summary

OVIS must preserve governed continuity without confusing convenience retention with sovereign state.

This ADR establishes that the default governed posture is ZDR-first, that continuation should be explicit and wrapper-managed, and that branch continuity remains anchored in OVIS-owned state rather than in OpenAI-managed durable session memory.

Durable runtime state may exist as an optional operating mode, but it is not canonical by default and does not outrank ADR-001 state authority.
