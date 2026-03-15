# ADR-005 — Review Gate and Policy Profiles

## Status
Accepted

## Date
2026-03-15

## Owner
OVIS

---

## Context

ADR-001 established canonical OVIS objects, lineage relations, append-only audit rules, and the Approval object as the canonical review record.

ADR-002 established function calling as the canonical capability bus and fixed policy enforcement as a pre-execution responsibility rather than a script-local convention.

ADR-003 established the Responses API as the canonical runtime substrate and the runtime wrapper as the only canonical model invocation path.

ADR-004 established ZDR-first default session posture and explicit continuation handling without weakening canonical state authority or execution governance.

The current transitional workspace still relies on partial direct checks and legacy approval-like fields, but does not yet define:

- canonical action risk classes
- policy profiles with explicit allowable risk ceilings
- a canonical review gate positioned between plan approval and execution
- execution enforcement rules tied to canonical Approval objects

Therefore OVIS requires a canonical review gate and policy profile system to govern the transition from planning to execution.

---

## Decision

OVIS shall define a canonical review gate positioned between `Plan Job` and `Execute Job`.

The review gate is the governance boundary that determines whether a planned action may become executable under a given policy profile and risk class.

OVIS shall classify requested actions by action risk, not merely by work-object category.

OVIS shall define canonical policy profiles that determine the maximum allowable risk class for execution without additional escalation.

No `Execute Job` may be created, promoted, or dispatched unless the review gate determines that:

- the action has been classified by risk
- the active policy profile permits that risk
- required approval conditions have been satisfied
- canonical Approval and Event records can be emitted

---

## Canonical Review Gate Position

The canonical review gate sits between `Plan Job` and `Execute Job`.

The intended sequence is:

`Signal -> Work Object -> Plan Job -> Review Gate -> Approval -> Execute Job -> Bridge Action / Outcome Events`

This means:

- planning may produce a proposed executable path
- the proposed path is not yet executable authority
- the review gate evaluates the action risk and profile compatibility
- only after the review gate succeeds may an `Execute Job` be created, promoted, or allowed to proceed

The review gate is therefore a canonical pre-execution enforcement boundary, not an optional post-hoc review ritual.

---

## Action Risk Classes

OVIS risk classes `R0` through `R4` classify requested actions and side effects.

### R0

No meaningful side effect.

Examples:

- read-only inspection
- passive analysis
- observational queries
- state-neutral reasoning support

### R1

Low-impact bounded internal state or low-risk controlled changes.

Examples:

- bounded internal metadata updates
- reversible low-impact administrative changes
- low-risk governance-safe state marking

### R2

Bounded local repo or workspace mutation with reversible or reviewable effects.

Examples:

- bounded patch creation
- contained file updates
- reviewable documentation or code changes within allowed scope

### R3

Elevated local execution, network use, or external-system mutation with meaningful operational risk.

Examples:

- local execution with consequential outputs
- network-enabled operations
- controlled writes to external systems
- actions that can materially affect system state outside simple bounded patching

### R4

Architecture, production, trust-boundary, or irreversible high-impact actions requiring explicit human approval.

Examples:

- production-affecting actions
- architecture-changing governance actions
- trust-boundary expansion
- high-impact bridge or connector changes
- irreversible or broadly consequential mutations

---

## Canonical Policy Profiles

OVIS shall canonize the following baseline policy profiles.

### read-only

Maximum allowable risk:
- `R0`

Purpose:
- permit inspection and analysis without meaningful side effects

### patch-only

Maximum allowable risk:
- up to `R2`

Purpose:
- permit bounded local repo or workspace mutation without broader execution authority

### local-exec

Maximum allowable risk:
- up to `R3`

Purpose:
- permit approved local execution paths where consequential local execution is allowed

### network-escalated

Maximum allowable risk:
- up to `R3`

Purpose:
- permit approved outbound/network or external integration actions under explicit escalation rules

### human-approved

Maximum allowable risk:
- required for `R4`

Purpose:
- represent explicit human-gated approval authority for high-risk actions

This profile may also be used as an override path for lower-risk actions when policy or context demands additional human review.

---

## Approval Schema for the Review Gate

ADR-005 refines the canonical Approval record for review-gate enforcement while remaining aligned with ADR-001.

The Approval object must retain the ADR-001 core fields:

- approval_id
- target_object_type
- target_object_id
- decision
- rationale
- reviewer_id
- created_at

For review-gate enforcement, the canonical Approval record must additionally carry:

- risk_class
- policy_profile
- approval_scope
- expires_at or equivalent bounded validity field

Purpose of the extended gate fields:

- `risk_class`: records the action-risk basis for the decision
- `policy_profile`: records the active policy context under which the decision was made
- `approval_scope`: records what exact class of action or boundary the approval covers
- `expires_at`: prevents open-ended approval reuse where bounded validity is required

These fields refine the canonical Approval object for execution governance; they do not replace the ADR-001 Approval foundation.

---

## Enforcement Rules

The review gate must enforce the following rules before execution:

1. every proposed executable action must be assigned a risk class
2. every proposed executable action must be evaluated against an active policy profile
3. an action whose risk exceeds the profile maximum must be blocked before execution
4. no `Execute Job` may be created, promoted, or dispatched unless the review gate permits it
5. `R4` actions must require explicit human approval
6. lower-risk actions may still require human approval if profile or context demands it
7. approval outcomes must emit canonical `approval.recorded` and related gate Events consistent with ADR-001
8. blocked actions must be represented as blocked governance outcomes rather than silently ignored or opportunistically executed

The review gate must therefore operate as an enforcement mechanism, not as advisory metadata.

---

## Execute Job Creation and Promotion Rules

The review gate controls not only dispatch but also the transition from planning into executable authority.

This means:

- a `Plan Job` may contain a proposed path
- a proposed path does not become an executable authority artifact until review-gate conditions are satisfied
- `Execute Job` creation, promotion, or readiness changes must remain downstream of review-gate enforcement

If a planned action is rejected, deferred, or blocked by profile mismatch, execution must not proceed through workaround paths.

---

## Relationship to ADR-001

ADR-001 defines:

- canonical objects
- canonical lineage
- canonical Approval object
- canonical Event requirements

ADR-005 preserves ADR-001 by ensuring that:

- approvals remain canonical objects rather than loose storage flags
- execution gating preserves lineage between `Plan Job`, `Approval`, and `Execute Job`
- governance-relevant decisions are represented by append-only Events
- review authority is expressed through canonical object and event relationships

---

## Relationship to ADR-002

ADR-002 defines function calling as the canonical capability bus and requires policy enforcement before execution.

ADR-005 provides the review-gate and policy-profile model that those pre-execution checks must enforce.

This means:

- function-called capabilities must not bypass risk classification
- schema-valid actions may still be blocked by risk/profile mismatch
- capability invocation authority depends on passing the review gate where required

---

## Relationship to ADR-003

ADR-003 defines the runtime wrapper as the only canonical model invocation path.

ADR-005 requires that runtime-mediated execution pathways route executable transitions through the review gate rather than allowing model invocation to create execution authority directly.

The runtime wrapper must therefore respect the review gate as a governance boundary, not just a convenience workflow step.

---

## Relationship to ADR-004

ADR-004 establishes ZDR-first session posture and explicit continuation handling.

ADR-005 requires that no continuation or session convenience weaken the review boundary.

This means:

- retained session state does not count as approval
- continuation across turns does not authorize execution by itself
- review requirements remain binding regardless of session mode

---

## Relationship to Legacy Direct Scripts

Current direct script checks and legacy storage-layer approval fields remain transitional only.

This includes fields such as:

- `sys_review_decision`
- script-local activation checks
- storage-row status markers that imply readiness without canonical review-gate enforcement

These are explicitly non-canonical substitutes for the Approval object until mapped through the canonical review gate.

Legacy direct checks may remain operationally useful during transition, but they do not define canonical governance.

---

## Immediate Consequences

1. OVIS now has a canonical review boundary between `Plan Job` and `Execute Job`.
2. Future execution design must classify requested actions by `R0–R4` risk before execution authority is granted.
3. Future policy enforcement work must implement the baseline policy profiles defined here.
4. `R4` actions must require explicit human approval.
5. Approval records used for execution governance must satisfy both ADR-001 core Approval requirements and ADR-005 gate fields.
6. Legacy approval-like storage fields must be treated as transitional adapters, not canonical governance authority.

---

## Deferred to Follow-on Decisions

The following are intentionally deferred:

- exact risk classification rubric for every capability and bridge action
- exact approval scope vocabulary
- exact approval expiration defaults
- exact human-review UX or operator workflow
- exact mapping table from each legacy script path to gate-enforced execution
- exact implementation of policy-profile resolution in the tool gateway/runtime wrapper

These will be defined in dependent ADRs and implementation specifications.

---

## Summary

OVIS requires more than planning and execution artifacts.

It requires a canonical governance boundary that decides when planned actions may become executable authority.

This ADR establishes that boundary through a review gate positioned between `Plan Job` and `Execute Job`, using action-centric risk classes, canonical policy profiles, and Approval records that preserve lineage and enforcement consistency across ADR-001 through ADR-004.
