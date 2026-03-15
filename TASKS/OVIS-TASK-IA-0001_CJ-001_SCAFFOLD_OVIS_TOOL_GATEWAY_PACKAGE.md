---
OVIS:
  id: OVIS-TASK-IA-0001
  title: CJ-001 Scaffold OVIS Tool Gateway Package
  type: TASK
  status: READY
  version: v0.1
  domain: IA
  repo: ovis-blueprint
  created: 2026-03-15
  last_updated: 2026-03-15
  author: codex
  contributors: []
  tags:
    - cj-001
    - p0
    - scaffold
    - tool-gateway
---

# OVIS-TASK-IA-0001
CJ-001 Scaffold OVIS Tool Gateway Package

## Purpose

Produce the first implementation-ready scaffold plan for the OVIS Tool Gateway package without implementing the gateway itself.

This task defines the minimal package structure, interface modules, result-envelope surface, and test skeleton required to begin controlled implementation in a dedicated runtime repository.

## Change Type

STRUCTURAL

## Target Repository

ovis-runtime

## Problem Statement

OVIS doctrine now defines:

- canonical state and lineage in ADR-001
- function calling as the canonical capability bus in ADR-002
- Responses API and runtime wrapper posture in ADR-003
- ZDR-first continuation posture in ADR-004
- review gate and policy profile enforcement in ADR-005
- branch continuity and compaction expectations in ADR-006

However, there is still no implementation package scaffold for the OVIS Tool Gateway.

Without a scaffolded package boundary, later implementation work will be forced to make package, interface, and test-layout decisions ad hoc.

## Objective

Define the minimal scaffold-only package plan for `ovis-runtime` with:

- package structure only
- registry module
- schema loader
- execution wrapper interface
- policy hook placeholders
- result-envelope structure
- shared types
- test skeleton

The plan must remain scaffold-only and must not implement runtime behavior, bridge behavior, persistence, compaction, or real execution dispatch.

## Package File Tree Proposal

```text
ovis-runtime/
  pyproject.toml
  README.md
  src/
    ovis_tool_gateway/
      __init__.py
      types.py
      registry.py
      schema_loader.py
      executor.py
      policy_hooks.py
      envelopes.py
  tests/
    test_types.py
    test_registry.py
    test_schema_loader.py
    test_executor_interface.py
    test_envelopes.py
```

## Module Responsibilities

### `types.py`

Define shared typed structures only:

- `CapabilityDefinition`
- `ExecutionRequest`
- `PolicyDecision`
- `ExecutionResultEnvelope`

These types must carry enough structure to remain compatible with ADR-001 through ADR-006 without implementing real execution.

### `registry.py`

Define the registry surface for:

- registering canonical capability definitions
- listing available capability definitions
- resolving a capability definition by name

Do not implement live execution, persistence, or side-effect dispatch.

### `schema_loader.py`

Define a scaffolded schema loading surface for:

- loading schema descriptors or schema resources
- validating that schemas can be discovered and attached to capability definitions

Do not implement a final schema format or full validation engine.

### `executor.py`

Define the execution wrapper interface only.

This surface may accept an `ExecutionRequest` and return an `ExecutionResultEnvelope`, but it must not perform real dispatch.

### `policy_hooks.py`

Define placeholder hook interfaces for pre-execution policy handling.

This must support future alignment with:

- ADR-005 risk classes
- ADR-005 policy profiles
- approval-required outcomes

Do not implement real policy evaluation.

### `envelopes.py`

Define the canonical result-envelope structure aligned with ADR-001 and ADR-002.

This module must shape the envelope contract but must not wire it to persistence, runtime, or event systems.

## Initial Interface Expectations

### `CapabilityDefinition`

Minimum fields:

- capability name
- version
- schema reference
- declared side-effect class

### `ExecutionRequest`

Minimum fields:

- capability name
- input payload
- correlation id
- idempotency key
- object context
- branch context

### `PolicyDecision`

Minimum fields:

- allow / deny / defer outcome
- risk class
- policy profile
- approval requirement flag
- rationale

### `ExecutionResultEnvelope`

Minimum fields:

- capability name
- execution status
- correlation id
- idempotency outcome
- policy disposition
- object references
- branch reference
- output or payload reference / hash
- error details

## ADR Compatibility Requirements

The scaffold plan must remain explicitly compatible with:

- ADR-001: canonical object and correlation context
- ADR-002: capability bus, schema discipline, idempotency, result envelopes
- ADR-003: wrapper-only invocation expectations
- ADR-004: ZDR-first and OVIS-owned continuity posture
- ADR-005: risk class and policy profile concepts
- ADR-006: branch continuity compatibility at the type and interface level only

## Forbidden Actions

This scaffold plan must not include:

- runtime implementation
- compaction implementation
- bridge implementation
- persistence wiring
- real execution dispatch
- real policy evaluation

The task is complete when the scaffold plan fixes package boundaries and interface surfaces without turning into full gateway implementation.

## Verification Requirements

1. `ADR_INDEX.md` includes ADR-006.
2. The scaffold plan targets `ovis-runtime`.
3. The file tree proposal includes `tests/test_types.py`.
4. The scaffold plan remains scaffold-only.
5. The scaffold plan references ADR-001 through ADR-006 explicitly.
6. No implementation behavior is specified beyond interfaces, placeholders, and test skeletons.

## Return Report Requirements

### Execution Summary
Short description of work performed.

### Files Created
List of files created.

### Files Updated
List of files updated.

### Verification Results
Results of required checks.

### Assumptions
Any assumptions made during execution.

### Status Recommendation
Ready for review / Blocked / Needs revision

## Review Outcome

To be completed by reviewer.

## Linked Change Record

To be created after task review.
