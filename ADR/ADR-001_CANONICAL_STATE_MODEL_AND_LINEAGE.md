# ADR-001 — Canonical State Model and Lineage Contract

## Status
Accepted

## Date
2026-03-15

## Owner
OVIS

---

## Context

The current OVIS workspace contains a functioning but transitional Notion-era pipeline for:

- signal routing
- work object creation
- plan job generation
- execute job generation
- activation checks
- event log writes
- local bridge dispatch

However, the current implementation does not yet provide a canonical OVIS-owned ontology for state, lineage, review, audit, and bridge actions.

Observed problems include:

- missing canonical runtime wrapper
- missing canonical tool gateway
- drift between Notion fields and script assumptions
- partial audit rows without authoritative append-only lineage
- review decisions without full approval object structure
- bridge execution occurring outside a governed canonical action model

Therefore OVIS requires a canonical state model before further runtime or bridge implementation proceeds.

---

## Decision

OVIS shall define and use a canonical state model independent of any specific storage backend.

Notion may remain a transitional operational surface, but it is not canonical OVIS state authority.

Canonical OVIS truth is defined by:

- object types
- object IDs
- lineage relations
- allowed transitions
- event contracts
- approval contracts
- bridge action contracts

All legacy Notion-era schemas and scripts must map explicitly to canonical objects and relations.

No implementation may assume a storage-layer field is canonical unless it is defined in this ADR or in a later dependent canonical schema specification.

---

## Canonical Object Types

### 1. Signal
Represents a raw or compressed input that may initiate work.

Required fields:
- signal_id
- source_type
- source_ref
- raw_input_ref or raw_input_hash
- compressed_summary
- extracted_signals
- nuance
- branch_id
- created_at
- created_by

Primary purpose:
- start or update a branch of work

---

### 2. Work Object
Represents a governed unit of work derived from one or more signals.

Required fields:
- work_object_id
- title
- objective
- status
- priority
- branch_id
- parent_signal_ids
- dependency_ids
- current_plan_id (nullable)
- current_execute_job_id (nullable)
- owner
- created_at
- updated_at

Primary purpose:
- authoritative container for operational work

---

### 3. Plan Job
Represents a planning artifact generated for a work object.

Required fields:
- plan_job_id
- work_object_id
- status
- objective
- assumptions
- constraints
- output_ref
- created_at
- updated_at

Primary purpose:
- planning-stage artifact subject to review

---

### 4. Approval
Represents a formal review decision on a governed object.

Required fields:
- approval_id
- target_object_type
- target_object_id
- decision
- rationale
- reviewer_id
- created_at

Allowed decisions:
- approve
- reject
- revise
- defer

Primary purpose:
- canonical review gate record

---

### 5. Execute Job
Represents a bounded executable action or action bundle.

Required fields:
- execute_job_id
- work_object_id
- parent_plan_job_id
- status
- job_kind
- execution_profile
- input_ref
- output_ref
- run_id (nullable)
- created_at
- updated_at

Primary purpose:
- canonical execution artifact

---

### 6. Event
Represents an append-only audit event.

Required fields:
- event_id
- event_type
- correlation_id
- parent_event_id (nullable)
- object_type
- object_id
- branch_id
- actor_type
- actor_id
- payload_ref or payload_hash
- created_at

Primary purpose:
- sovereign audit lineage

---

### 7. Bridge Action
Represents a controlled external action routed outside the kernel.

Required fields:
- bridge_action_id
- execute_job_id
- target_system
- action_type
- status
- request_ref
- response_ref
- created_at
- updated_at

Primary purpose:
- canonical boundary object for external effects

---

### 8. Branch
Represents a recursive continuity container.

Required fields:
- branch_id
- root_signal_id
- current_state_ref
- latest_compaction_id (nullable)
- status
- created_at
- updated_at

Primary purpose:
- continuity and compaction boundary

---

### 9. Compaction Record
Represents a governed compression of branch state.

Required fields:
- compaction_id
- branch_id
- source_range
- compacted_state_ref
- preserved_reference_index
- created_at

Primary purpose:
- recursive continuity and resumability

---

## Canonical IDs

All canonical IDs must be globally unique and stable.

Minimum canonical ID set:
- signal_id
- work_object_id
- plan_job_id
- approval_id
- execute_job_id
- event_id
- bridge_action_id
- branch_id
- compaction_id
- correlation_id

Legacy storage-layer row IDs may exist, but they are not sufficient on their own as canonical lineage anchors.

---

## Canonical Relations

The minimum lineage graph is:

Signal
→ Work Object
→ Plan Job
→ Approval
→ Execute Job
→ Bridge Action
→ Outcome Events

Branch is the continuity container across all stages.

Event records must be able to point to:
- the object acted upon
- the branch in which it occurred
- the correlation group to which it belongs

---

## Allowed Transitions

### Work Object
Allowed statuses:
- inbox
- routed
- planning
- awaiting_review
- approved
- executing
- blocked
- completed
- failed
- archived

Example allowed transitions:
- inbox → routed
- routed → planning
- planning → awaiting_review
- awaiting_review → approved
- approved → executing
- executing → completed
- executing → failed
- any active state → blocked

Forbidden:
- inbox → executing
- planning → completed
- failed → approved without explicit new review or reset

---

### Plan Job
Allowed statuses:
- draft
- ready_for_review
- approved
- rejected
- superseded

---

### Execute Job
Allowed statuses:
- draft
- planned
- ready
- running
- review
- completed
- failed
- cancelled

---

### Bridge Action
Allowed statuses:
- pending
- dispatched
- acknowledged
- failed
- cancelled

---

## Lineage Contract

OVIS lineage is considered provable only if all of the following are true:

1. every Signal has a canonical signal_id
2. every Work Object links to at least one parent Signal
3. every Plan Job links to one Work Object
4. every Approval links to exactly one target object
5. every Execute Job links to one Work Object and one parent Plan Job
6. every Bridge Action links to one Execute Job
7. every significant transition emits an Event
8. every Event includes a correlation_id
9. every branchable chain can be associated with a branch_id

If any of these are missing, lineage is partial or unprovable.

---

## Audit Contract

The Event object is append-only.

Rules:
- events are never mutated in place
- corrections are represented by new events
- destructive overwrite of historical events is forbidden
- bridge and execution side effects must emit pre-action and post-action events where possible

Minimum required event families:
- signal.created
- signal.compressed
- work_object.created
- work_object.updated
- plan_job.created
- approval.recorded
- execute_job.created
- execute_job.status_changed
- bridge_action.dispatched
- bridge_action.completed
- bridge_action.failed
- compaction.created

---

## Storage Principle

OVIS canonical state is storage-agnostic.

Storage backends such as:
- Notion
- local files
- databases
- future services

may persist or mirror canonical state, but do not define canonical truth by themselves.

---

## Legacy Mapping Rules

The existing Notion-era pipeline is transitional.

The following mapping rules apply:

- Signal Compressor rows map to canonical Signal
- Work Objects database rows map to canonical Work Object
- Codex Jobs rows may map to either Plan Job or Execute Job depending on sys_job_kind
- sys_review_decision does not replace canonical Approval
- Event Log rows map to canonical Event only if they can satisfy required lineage fields
- bridge scripts are legacy dispatch mechanisms until wrapped by canonical Bridge Action rules

Any legacy field not mapped here is non-canonical until explicitly classified.

---

## Immediate Consequences

1. No further kernel implementation should proceed without referencing this ADR.
2. Runtime wrapper design must use these object and event boundaries.
3. Tool gateway design must emit canonical Event objects.
4. Review gate design must emit canonical Approval objects.
5. Bridge work must produce canonical Bridge Action records.
6. Notion scripts must be treated as transitional adapters, not kernel truth.

---

## Deferred to Follow-on ADRs

The following are intentionally deferred:
- exact runtime wrapper interface
- exact tool gateway schema format
- exact policy profile taxonomy
- exact compaction algorithm
- exact bridge registry implementation
- exact storage backend implementation details

These will be defined in dependent ADRs.

---

## Summary

OVIS does not yet become canonical by having working scripts.
OVIS becomes canonical when all work, review, execution, bridge, and audit behavior can be expressed through a sovereign object model with provable lineage.

This ADR establishes that model.
