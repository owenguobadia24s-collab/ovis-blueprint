# ADR-006 — Branch Compaction and Continuity

## Status
Accepted

## Date
2026-03-15

## Owner
OVIS

---

## Context

ADR-001 established the canonical `Branch` object as the recursive continuity container and the `Compaction Record` as the governed compression artifact for branch state.

ADR-002 established function calling as the canonical capability bus for executable internal actions.

ADR-003 established the Responses API as the canonical runtime substrate and the runtime wrapper as the only canonical model invocation path.

ADR-004 established ZDR-first default session posture and fixed branch continuity as OVIS-owned rather than runtime-owned.

ADR-005 established the review gate and policy profiles so that execution authority remains governed as work transitions from planning to execution.

What remains undefined is the branch compaction policy itself, including:

- what the canonical branch object must represent as a continuity container
- when compaction must be triggered
- what a compaction record must preserve
- how compacted state supports downstream planning and execution
- which Events must be emitted when compaction occurs

The architecture and task extraction materials indicate that compaction is a canonical primitive for long-running work, recursive sessions, and multi-branch orchestration, and that it must preserve useful lineage rather than replace it with opaque summaries.

Therefore OVIS requires a canonical branch compaction and continuity policy.

---

## Decision

OVIS shall treat the canonical `Branch` object as the continuity container for recursive work.

OVIS shall use governed compaction to reduce long-branch operational noise while preserving the references needed for future planning, execution, audit, and review.

Compaction does not replace canonical branch identity.

Compaction produces a canonical `Compaction Record` linked to the branch and to preserved references needed to resume governed work.

Branch continuity remains OVIS-owned before, during, and after compaction, and must remain compatible with the ZDR-first posture defined by ADR-004.

---

## Canonical Role of the Branch Object

The canonical `Branch` object is the continuity container for a branch of governed work.

Its purpose is to:

- anchor recursive work over time
- associate Signals, Work Objects, Plan Jobs, Approvals, Execute Jobs, Bridge Actions, and Events to one continuity container
- preserve branch identity even when runtime sessions rotate, expire, or are compacted
- support resumability for future planning and execution

The branch object therefore represents continuity authority, not merely a runtime session handle or a storage-row convenience.

Compaction operates on branch state; it does not replace the branch object.

---

## Compaction Trigger Policy

Compaction shall be triggered when branch state becomes large, fragmented, or operationally noisy enough that direct continuation is no longer governance-efficient or planning-efficient.

Canonical compaction trigger classes include:

- before archival of a branch
- before branch fork where continuity should be reduced to a governed compact state
- after meaningful execution milestones
- when recursive session history becomes too long or noisy for reliable downstream planning
- when retained runtime context is no longer an appropriate continuity mechanism under ADR-004

Compaction may also be triggered by explicit operator or system policy when branch maintainability requires it.

Compaction is not required for every branch step.

It is required when branch continuity would otherwise depend too heavily on raw accumulated history rather than governed state.

---

## Compaction Record

The canonical `Compaction Record` remains aligned with ADR-001 and must include:

- `compaction_id`
- `branch_id`
- `source_range`
- `compacted_state_ref`
- `preserved_reference_index`
- `created_at`

For operational continuity, the compaction record must additionally make clear:

- what span of branch material was compacted
- what governed state representation now stands in as the compacted continuity artifact
- what references were intentionally preserved for resumability and audit

The compaction record is the authoritative record of a compaction event; it is not just a convenience summary.

---

## Required Preserved References

Compaction must preserve the references required for governed continuation.

At minimum, the preserved reference index must retain or point to:

- root `Signal` identity
- relevant `Work Object` identities
- current or latest `Plan Job` identity where material
- current or latest `Approval` identity where material
- current or latest `Execute Job` identity where material
- relevant `Bridge Action` identity where material
- material `Event` anchors needed to preserve audit lineage
- branch-level correlation context required for downstream reasoning or audit reconstruction

Compaction may reduce narrative or operational noise, but it must not sever the references required to prove lineage or resume work safely.

---

## Compacted State as a Continuity Artifact

The compacted state referenced by a compaction record is the governed continuity artifact used to support subsequent planning and execution.

Its purpose is to provide:

- a reduced, governed representation of prior branch history
- sufficient continuity for downstream planning
- sufficient continuity for downstream execution support
- a resumable reference point that does not depend on opaque retained runtime state

Compacted state must therefore remain:

- branch-linked
- reference-preserving
- audit-compatible
- usable without requiring the entire raw branch transcript to be replayed

Compacted state is not a free-form replacement for canonical lineage.

It is a governed continuity artifact built on top of canonical lineage.

---

## Planning and Execution After Compaction

Compaction must support future planning and execution rather than forcing a branch restart.

After compaction:

- downstream planning may use the compacted state as the branch continuity input
- downstream execution support may use the compacted state as the resumable branch context
- review and approval requirements remain unchanged
- branch identity remains stable even though the continuity artifact has been reduced

Compaction must therefore preserve enough structured continuity that:

- planning can continue without reconstructing the full prior branch transcript
- execution can proceed with accurate branch context
- governance can still determine how current actions relate to prior branch history

---

## Canonical Event Emission for Compaction

Compaction must emit canonical Events consistent with ADR-001.

At minimum, a compaction operation must produce:

- `compaction.created`

Where material, compaction-related event emission should also include:

- an Event identifying the affected `Branch`
- the branch correlation context
- references to the resulting `Compaction Record`
- references or hashes sufficient to connect the compacted state artifact to the governance trail

Compaction Events must remain append-only and must not overwrite prior branch history.

Compaction reduces continuity noise; it does not erase audit history.

---

## Relationship to ADR-001

ADR-001 defines:

- the canonical `Branch` object
- the canonical `Compaction Record`
- append-only `Event` requirements
- branch-linked lineage expectations

ADR-006 preserves ADR-001 by ensuring that:

- the branch remains the authoritative continuity container
- compaction records remain canonical objects
- preserved references retain the lineage needed for resumability and audit
- compaction emits canonical Events rather than mutating history in place

---

## Relationship to ADR-002

ADR-002 defines function calling as the canonical capability bus.

ADR-006 therefore requires that future compaction execution, when implemented, occur through governed capability boundaries rather than ad hoc direct mutation.

Compaction may become an executable capability later, but the governing rules for continuity and preserved references are fixed here first.

---

## Relationship to ADR-003

ADR-003 defines the runtime wrapper as the only canonical model invocation path.

ADR-006 requires that branch continuity and compaction remain outside ad hoc runtime retention behavior.

This means runtime support for compaction must remain subordinate to canonical branch and compaction objects rather than making runtime session memory the authoritative continuity substrate.

---

## Relationship to ADR-004

ADR-004 established that branch continuity remains OVIS-owned and that ZDR-first posture must avoid over-dependence on retained runtime state.

ADR-006 is explicitly downstream of that decision.

This means:

- compaction must support continuity without requiring durable OpenAI-managed state
- compacted state must remain usable under ZDR-first operating conditions
- branch resumability must survive session expiration, session rotation, or low-retention runtime modes

Compaction is therefore a continuity mechanism that reinforces ADR-004 rather than weakening it.

---

## Relationship to ADR-005

ADR-005 established the review gate and policy profiles for execution governance.

ADR-006 does not weaken those controls.

After compaction:

- planning may continue
- execution support may continue
- review and approval boundaries remain fully in force

Compaction improves continuity handling, not execution authority.

---

## Immediate Consequences

1. OVIS now has a canonical definition of the branch as the continuity container for recursive work.
2. Future compaction design must preserve governed references rather than produce opaque summaries with broken lineage.
3. Future planning and execution support must be able to resume from compacted state without depending on full raw branch replay.
4. Compaction must emit canonical append-only Events rather than replacing audit history.
5. Branch continuity must remain OVIS-owned and compatible with ADR-004 ZDR-first posture.
6. Future implementation must treat compaction as a governance-preserving continuity operation, not a lossy convenience shortcut.

---

## Deferred to Follow-on Decisions

The following are intentionally deferred:

- exact compaction thresholds and heuristics
- exact compacted state format
- exact preserved reference index encoding
- exact operator controls for manual compaction
- exact capability contract for invoking compaction through the tool gateway
- exact interaction between compaction checkpoints and scheduler/orchestration behavior

These will be defined in dependent ADRs and implementation specifications.

---

## Summary

OVIS requires a governed way to keep long-running branches usable without surrendering continuity to raw transcript accumulation or retained runtime state.

This ADR establishes the canonical branch as the OVIS-owned continuity container, defines compaction as the governed reduction of branch noise, requires preserved references and canonical compaction Events, and ensures that future planning and execution can resume from compacted state while remaining aligned with ADR-001 through ADR-005.
