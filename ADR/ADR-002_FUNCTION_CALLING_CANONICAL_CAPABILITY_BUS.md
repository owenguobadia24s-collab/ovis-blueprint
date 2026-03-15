# ADR-002 — Function Calling as the Canonical OVIS Capability Bus

## Status
Accepted

## Date
2026-03-15

## Owner
OVIS

---

## Context

ADR-001 established that canonical OVIS authority is defined by sovereign object types, lineage relations, allowed transitions, append-only audit events, and storage-agnostic state.

The current workspace still contains direct transitional scripts that:

- write to Notion directly
- generate and update legacy work rows directly
- perform bridge dispatch outside a canonical action model
- assume storage-layer fields as execution inputs
- rely on script-local validation rather than a canonical policy boundary

The existing task extraction also identifies a missing tool gateway and explicitly calls for:

- strict JSON-schema tool definitions
- policy checks before execution
- idempotent execution handling
- structured result envelopes
- audit emission for meaningful actions

Therefore OVIS requires a canonical decision fixing function calling as the capability bus through which governed internal actions are invoked.

---

## Decision

OVIS shall use function calling as the canonical capability bus for executable internal actions.

Function calling is the required control surface for:

- state transitions
- approval checks
- audit emission
- bridge dispatch preparation
- governed retrieval and support actions
- execution orchestration inputs

No executable OVIS-native capability shall be treated as canonical unless it is exposed through a function-called interface or a later approved equivalent that preserves the same governance properties.

Raw model text, prompt conventions, or script-local branching logic are not canonical capability boundaries.

---

## Canonical Role of Function Calling

Function calling is the canonical invocation layer between OVIS reasoning and OVIS action.

Its purpose is to ensure that:

- capabilities are explicitly named
- inputs are schema-governed
- outputs are structurally bounded
- side effects are policy-gated
- execution is auditable
- duplicate invocations can be safely controlled

Function calling does not replace the OVIS state model from ADR-001.

Instead, it is the execution bus through which stateful actions must be requested, validated, authorized, executed, and recorded.

---

## Schema Discipline

All canonical function-called capabilities must define explicit machine-validatable schemas.

Minimum schema expectations:

- stable capability name
- explicit version or schema identifier
- required input fields
- optional input fields
- explicit output/result envelope
- declared side-effect class
- declared target object type or object types where applicable

Schema rules:

- free-form action payloads are non-canonical
- missing required arguments must be rejected
- unknown or disallowed arguments must be rejected or ignored by explicit policy
- capability contracts must be specific enough to support deterministic validation and audit recording
- schema changes that affect governance, lineage, or execution meaning require review

The purpose of schema discipline is not cosmetic typing.

It exists to protect canonical state, review lineage, and audit trustworthiness.

---

## Result Envelope Expectations

Canonical capability execution must return a structured result envelope.

Minimum envelope expectations:

- capability name
- execution status
- object type and object id when applicable
- correlation_id
- idempotency outcome
- side-effect outcome
- output reference, payload reference, or payload hash
- policy disposition
- error or refusal details when execution is blocked or fails

Canonical capability execution must never rely on unstructured prose alone as the authoritative result record.

---

## Idempotency Requirements

All canonical side-effecting capabilities must support idempotency controls.

Minimum idempotency expectations:

- a capability invocation must be associated with an idempotency key or an equivalent deterministic replay key
- duplicate submissions of the same governed action must not silently create multiple state transitions or external side effects
- the execution layer must be able to distinguish:
  - first execution
  - safe replay
  - conflicting duplicate

Where applicable, idempotency keys should bind to:

- target object identity
- intended action type
- correlation_id or branch context
- caller-supplied or system-issued replay boundary

Idempotency is required because OVIS cannot maintain trustworthy lineage if retries and duplicates create ambiguous state.

---

## Side-Effect Boundaries

Function calling is the canonical boundary for side effects.

This means:

- model outputs must not directly perform external writes
- model outputs must not directly trigger shell, network, or file mutation without passing through the canonical capability layer
- bridge dispatch must be mediated as a governed capability
- approval-sensitive actions must pause at the policy layer before execution

Canonical side-effect classes must be distinguishable at minimum as:

- read-only
- state-write
- local execution
- external bridge write
- approval-required high-risk action

Direct execution paths that bypass the capability bus are legacy or non-canonical unless explicitly approved in a later ADR.

---

## Policy Enforcement Expectations

Policy enforcement must occur before a canonical capability executes.

Minimum policy expectations:

- validate schema before execution
- determine action class before execution
- require approval where policy requires approval
- reject disallowed actions before side effects occur
- emit canonical Event records for allowed, blocked, and failed governed actions where material

Policy enforcement must be centralized enough that governance does not depend on each script remembering to behave correctly.

Function calling therefore serves not only as the invocation surface, but as the control point where policy is attached.

---

## Relationship to ADR-001

ADR-001 defines:

- canonical objects
- canonical relations
- lineage anchors
- append-only event rules

ADR-002 defines how executable actions interact with that state model.

Function-called capabilities must therefore preserve ADR-001 by ensuring that:

- object-changing actions target canonical objects
- approvals are represented canonically rather than as loose storage fields
- Events are emitted in a way that preserves append-only lineage
- bridge-related effects are represented through canonical Bridge Action boundaries

If a capability cannot preserve ADR-001 lineage requirements, it is not ready to be canonical.

---

## Relationship to Legacy Direct Scripts

The current Notion-era and direct execution scripts remain transitional only.

This includes scripts that:

- create or update Notion rows directly
- generate plan or execute jobs by storage-field convention
- write event rows without canonical correlation requirements
- dispatch local hooks or bridge actions outside a governed capability contract

These scripts may continue to exist as legacy operational adapters or migration references, but they are not the canonical capability bus.

No direct script path should be treated as canonical merely because it currently works.

To become governance-safe, a legacy script must be:

- wrapped behind a canonical capability definition
- constrained by schema validation
- subject to policy enforcement
- made auditable through canonical Event linkage
- mapped to ADR-001 object semantics

Until then, direct scripts are legacy mechanisms, not kernel truth.

---

## Relationship to Skills

Skills are not the canonical capability bus.

Skills may package:

- process guidance
- planning playbooks
- review routines
- operator conventions

But skills are not authoritative executable action interfaces.

Skills may guide how a capability should be used.
They do not replace the need for function-called executable boundaries.

---

## Relationship to MCP

MCP is not the canonical internal capability bus.

MCP may be used later as a gated external connector layer where approved, but it must not serve as:

- canonical OVIS-owned state authority
- trusted internal service mesh
- default execution path for internal governed actions

Where MCP is used, it must sit outside the canonical kernel boundary and behind OVIS-controlled policy and bridge rules.

---

## Relationship to Bridges

Bridges are external effect boundaries, not the core capability bus.

Canonical bridge behavior must be initiated through function-called OVIS-controlled capabilities first.

Bridge execution must remain consistent with ADR-001 by producing or linking:

- Execute Job context
- Bridge Action records
- append-only Event records
- required approvals where policy demands them

This means Notion integration, MCP-backed connectors, and future external systems must all be downstream of the canonical capability bus rather than bypassing it.

---

## Immediate Consequences

1. Function calling is fixed as the canonical invocation layer for executable OVIS-native capabilities.
2. Future tool gateway work must enforce explicit schemas, idempotency, result envelopes, and policy checks.
3. Legacy direct scripts must be treated as transitional adapters until wrapped or replaced by canonical capabilities.
4. Skills may remain process assets, but not executable authority boundaries.
5. MCP may remain optional and gated, but not canonical internal plumbing.
6. Bridge work must be downstream of the canonical capability bus, not parallel to it.

---

## Deferred to Follow-on ADRs

The following are intentionally deferred:

- exact tool registry format
- exact JSON schema encoding strategy
- exact policy profile taxonomy
- exact approval escalation matrix
- exact bridge registry implementation
- exact runtime adapter interface

These will be defined in dependent ADRs.

---

## Summary

OVIS requires a single canonical action surface if lineage, approvals, idempotency, and audit are to remain trustworthy.

This ADR establishes function calling as that surface.

Capabilities may evolve, bridges may expand, and storage backends may change, but executable authority must remain governed through explicit function-called boundaries rather than raw scripts, prompt rituals, or untrusted connector plumbing.
