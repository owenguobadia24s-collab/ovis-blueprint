---
id: TEMPLATE-TEMP-0004
title: Notion Codex Job Contract Template
type: TEMPLATE
status: draft
authority: operational
version: '0.1'
layer: blueprint
domain: temp
repo: ovis-blueprint
path: TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md
owner: Owen Vitae
created: '2026-05-20'
last_updated: '2026-05-20'
registry: ovis-blueprint/REGISTRIES/entries/TEMPLATE-TEMP-0004.yaml
module_id: MOD-PLANNING-EXECUTION-LOOP-0001
module_slug: planning_execution_loop
system_id: SYS-KERNEL-0001
system_slug: ovis_kernel
related_module_ids:
  - MOD-REVIEW-GATE-0001
  - MOD-BRIDGE-DISPATCH-0001
---

# Purpose

Provide the minimum safe contract template for a Notion-cockpit-issued job that is intended to be executed by Codex against Git repos under OVIS governance.

# Scope

This template defines the minimum fields required for a job to be considered safe for execution by Codex as an execution satellite.

This template does not:

- authorize direct Notion writes by Codex
- implement bridge automation or dispatch tooling
- override canonical authority boundaries defined by `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md`

This artifact is provisional until registered by a separate metadata registration CJ. It must not be treated as registry-canonical until registry entry allocation is completed.

# Content

## Minimum Safe Contract (Checklist)

A job MUST specify:

- `job_kind`: `Plan` or `Execute`
- `job_id`: runtime-assigned job identifier (preferred) or cockpit temporary identifier
- `branch_id`: canonical branch identifier (or `TBD` if the runtime will allocate it on ingestion)
- `correlation_id`: correlation anchor for audit/event linkage
- `idempotency_key`: replay-safe key for side-effect control

- Objective: single-sentence objective plus bounded acceptance criteria
- Inputs: explicit inputs only (no “read anything” or “use all context”)
- Allowed outputs: explicit deliverables (files, reports, diffs) and required return format
- Allowed paths: repo-relative allowlist; default deny-by-omission
- Forbidden actions: explicit denylist (must include “no Notion writes” and “no scope expansion”)
- Validation commands: exact commands Codex must run and report results for
- Review gate: who approves and what evidence is required before Execute jobs may run

## Contract Template (Fill-In)

### Identity

- job_kind: `Plan|Execute`
- job_id: `TBD`
- branch_id: `TBD`
- correlation_id: `TBD`
- idempotency_key: `TBD`

### Objective

- objective: `TBD`
- acceptance_criteria:
  - `TBD`

### Inputs (Explicit)

- required_inputs:
  - `TBD`
- allowed_context_sources:
  - `Git repo content under allowed paths only`
  - `OVIS runtime-provided job packet (if any)`

### Allowed Outputs (Explicit)

- deliverables:
  - `TBD`
- required_return_format:
  - summary
  - files changed/created (paths)
  - checks run (commands + results)
  - assumptions
  - open issues / follow-ups

### Allowed Paths (Allowlist)

- repo: `TBD`
- allowed_paths:
  - `TBD`

### Forbidden Actions (Denylist)

At minimum:

- No direct Notion writes (all Notion updates are runtime bridge actions).
- No edits to `REGISTRIES/**`.
- No scope expansion beyond the job objective and allowed paths.
- No execution against `ovc-*` repos unless an explicit separate authorized job permits it.

### Validation Commands

List exact commands to run (examples only; replace with project-specific commands):

- `git status`
- `git diff --stat`
- `python -m pytest -q`

### Review Gate

- Plan jobs require: human review approval before creating Execute jobs.
- Execute jobs require: explicit approval decision recorded by the cockpit and ingested by runtime into a canonical Approval object.
- Evidence required:
  - bounded scope and allowlist
  - forbidden actions list present
  - validation commands present

### Event Emission Checklist (Conceptual)

Runtime-owned events SHOULD exist for:

- job.issued
- approval.recorded
- job.started
- job.completed OR job.failed

Codex output must include enough structured information for runtime to emit these events (correlation_id, job_id, validation results, and artifact references).

# References

- `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md`
- `ADR/ADR-001_CANONICAL_STATE_MODEL_AND_LINEAGE.md`
- `ADR/ADR-002_FUNCTION_CALLING_CANONICAL_CAPABILITY_BUS.md`
