# ADR-003 — Responses API as the Canonical OVIS Runtime

## Status
Accepted

## Date
2026-03-15

## Owner
OVIS

---

## Context

ADR-001 established the canonical OVIS state model, lineage contract, append-only audit rules, and storage-agnostic authority boundaries.

ADR-002 established function calling as the canonical OVIS capability bus for executable internal actions.

The current OVIS workspace still lacks a canonical runtime wrapper and still contains transitional direct scripts rather than a single governed model invocation path.

The existing task extraction and architecture materials also identify the need for a runtime decision that:

- centralizes model invocation
- prevents scattered direct model calls
- supports tool mediation
- supports continuation across multi-step work
- preserves governance in ZDR-sensitive and audit-sensitive contexts
- treats OpenAI-managed durable state as optional rather than authoritative

Therefore OVIS requires a canonical runtime decision that fixes the Responses API as the runtime substrate and the OVIS runtime wrapper as the only canonical model invocation path.

---

## Decision

OVIS shall use the Responses API as the canonical runtime substrate for model invocation.

OVIS shall implement and use a runtime wrapper as the only canonical model invocation path for core implementation.

The runtime wrapper shall be the boundary through which OVIS:

- invokes models
- manages continuation and session handling
- mediates function-called capabilities
- applies runtime policy and deployment-mode rules
- records runtime-relevant lineage and audit anchors

Direct model invocation scattered across scripts, services, or bridge logic is non-canonical.

No core implementation path may rely on ad hoc model calls once the canonical runtime wrapper exists.

---

## Canonical Role of the Runtime Wrapper

The runtime wrapper is the canonical invocation layer between OVIS logic and the Responses API.

Its purpose is to ensure that:

- model access is centralized
- runtime behavior is configurable without architectural drift
- continuation rules are consistent
- tool mediation occurs through approved capability boundaries
- runtime decisions can be audited and governed

The runtime wrapper is not the canonical state authority.

ADR-001 remains authoritative for objects, lineage, transitions, and audit requirements.

The runtime wrapper exists to interact with that canonical state safely and consistently.

---

## Responses API as Runtime Substrate

Responses API is the canonical substrate because it supports the required OVIS runtime properties, including:

- structured model invocation
- multi-turn continuation
- function calling
- official forward-looking API support
- compatibility with governed session patterns

OVIS shall treat the Responses API as the official runtime foundation rather than relying on deprecated, UX-only, or ad hoc invocation surfaces.

This decision fixes the runtime substrate, not the final implementation details of the wrapper.

---

## Wrapper-Only Invocation Rule

The OVIS runtime wrapper is the only canonical path for core model invocation.

This means:

- core services must not call models directly
- bridge logic must not call models directly
- review logic must not call models directly
- execution orchestration must not call models directly
- legacy or support scripts that still call models directly are non-canonical unless explicitly approved as transitional exceptions

The wrapper-only rule exists to prevent:

- runtime drift
- inconsistent retention behavior
- inconsistent continuation semantics
- inconsistent capability mediation
- fragmented audit and policy behavior

---

## Continuation and Session Handling Expectations

OVIS continuation behavior must be explicit and governed.

Minimum runtime expectations:

- multi-step work must support continuation through canonical runtime-managed session semantics
- continuation identifiers must be linkable to canonical branch and object context where material
- runtime state used for continuation must not replace ADR-001 canonical state
- continuation behavior must be consistent enough to support branch resumability and compaction-aware operation later

The default runtime direction for governed work is:

- OVIS-owned canonical state first
- minimal retained OpenAI-managed state by default
- continuation through explicit runtime-managed chaining mechanisms

Where supported and appropriate, continuation should favor patterns compatible with explicit response chaining such as `previous_response_id`.

---

## OpenAI-Managed Durable State Classification

OpenAI-managed durable state is optional and derived unless later explicitly approved by a dependent ADR.

This means:

- OpenAI-managed retained context is not canonical OVIS state authority
- runtime continuation state held by OpenAI does not replace canonical OVIS objects
- durable OpenAI state may be used where governance allows, but only as a deployment-mode option
- retained OpenAI state must be treated as convenience state, cached state, or derived state rather than sovereign operational truth

Where governance, retention, or ZDR constraints are stronger, the runtime should prefer lower-retention session patterns such as `store=false` with explicit continuation handling.

---

## Relationship to ADR-001

ADR-001 defines the canonical OVIS objects and lineage contract.

The runtime wrapper must preserve ADR-001 by ensuring that:

- model invocation does not redefine canonical state
- continuation state does not become authoritative branch state
- runtime outputs that matter operationally are attached back to canonical objects
- runtime-relevant actions can be represented through canonical Event linkage where material

The runtime wrapper may assist with object creation, planning, execution support, compaction support, or bridge preparation, but the authoritative meaning of those objects remains defined by ADR-001.

---

## Relationship to ADR-002

ADR-002 defines function calling as the canonical capability bus.

The runtime wrapper must therefore treat function-called capabilities as the canonical executable action boundary.

This means:

- model reasoning may propose actions
- executable capabilities must still be mediated through ADR-002 function-called interfaces
- runtime invocation must not bypass capability schemas, policy checks, idempotency controls, or result envelope expectations

In practice:

- Responses API supplies the runtime substrate
- the runtime wrapper centralizes invocation behavior
- function-called capabilities remain the action bus

These roles are complementary and must not be collapsed into scattered direct calls.

---

## Prohibition on Scattered Direct Model Calls

Scattered direct model calls in core implementation are prohibited.

Examples of non-canonical patterns include:

- script-local model client setup for core workflows
- bridge modules making direct model calls outside the wrapper
- review or planning services calling models without runtime mediation
- service-specific retention or continuation behavior that bypasses the canonical runtime policy

Such patterns are prohibited because they fragment:

- runtime governance
- continuation behavior
- auditability
- retention controls
- capability mediation discipline

---

## Relationship to Legacy Direct Scripts

The current transitional direct scripts remain non-canonical runtime paths.

Scripts that currently orchestrate work through storage updates, direct bridge behavior, or script-local assumptions may remain as legacy operational assets, but they do not define canonical runtime behavior.

If any legacy script later needs model invocation, it must do so through the canonical runtime wrapper rather than inventing a new model access path.

---

## Session Mode Expectations

OVIS runtime behavior must support deployment-mode-aware session handling.

Minimum expectations:

- a governed mode that minimizes retained OpenAI state
- a continuation mode that supports multi-step work without redefining canonical state authority
- optional support for durable OpenAI-managed state only where governance permits
- compatibility with function calling and future compaction-aware branch handling

The exact runtime mode matrix is deferred, but the canonical direction is ZDR-friendly and OVIS-state-first.

---

## Immediate Consequences

1. Responses API is fixed as the canonical runtime substrate for OVIS model invocation.
2. The runtime wrapper is fixed as the only canonical model invocation path for core implementation.
3. Future runtime work must centralize continuation, session behavior, and tool mediation in the wrapper.
4. Core implementation must not introduce scattered direct model calls.
5. OpenAI-managed durable state must be treated as optional or derived unless later explicitly approved.
6. Runtime implementation must remain aligned with ADR-001 state authority and ADR-002 capability mediation.

---

## Deferred to Follow-on ADRs

The following are intentionally deferred:

- exact runtime wrapper interface
- exact model/provider configuration surface
- exact continuation token storage strategy
- exact session mode matrix
- exact webhook, background, or WebSocket operational policy
- exact retry behavior and failure handling rules

These will be defined in dependent ADRs.

---

## Summary

OVIS requires a single canonical runtime substrate and a single canonical invocation path if continuation, governance, capability mediation, and audit behavior are to remain coherent.

This ADR establishes Responses API as the runtime substrate and the OVIS runtime wrapper as the only canonical model invocation boundary for core implementation.

OpenAI-managed durable state may exist as an optional deployment choice, but canonical OVIS authority remains outside it unless a later ADR explicitly says otherwise.
