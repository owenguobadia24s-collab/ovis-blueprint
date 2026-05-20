---
id: TEMPLATE-TEMP-0003
title: Codex Promotion Gate Template
type: TEMPLATE
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: temp
repo: ovis-blueprint
path: TEMPLATES/CODEX_PROMOTION_GATE.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-05-20'
registry: ovis-blueprint/REGISTRIES/entries/TEMPLATE-TEMP-0003.yaml
module_id: MOD-REVIEW-GATE-0001
module_slug: review_gate
system_id: SYS-KERNEL-0001
system_slug: ovis_kernel
---

# Purpose

Provide the reusable template named Codex Promotion Gate Template.

# Scope

This file governs or documents Codex Promotion Gate Template within the ovis-blueprint repository.

# Content

## CODEX Promotion Gate Template

Use this template to define minimal promotion gates for the Plan → Execute → Dispatch lifecycle.

This template is aligned with `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md` and
`TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md`.

### Gate 1: Plan → Approved

- Entry criteria
  - Plan job contract is present and bounded (objective + allowlist + forbidden actions).
  - Risks and unknowns are surfaced.
- Required evidence
  - Review checklist completed.
- Exit criteria
  - Explicit human approval decision recorded (cockpit input), ingested by runtime into canonical Approval.

### Gate 2: Execute → Activated

- Entry criteria
  - Execute job references an approved plan.
  - Allowed paths and forbidden actions are explicit.
  - Validation commands are explicit.
- Required evidence
  - Activation checks confirm required contract fields are present.
- Exit criteria
  - Job is marked ready for dispatch by runtime policy.

### Gate 3: Activated → Dispatched

- Entry criteria
  - Execute job remains bounded and unchanged in scope since approval.
  - No forbidden areas will be touched.
- Required evidence
  - Final pre-dispatch review complete (may be lightweight for low-risk jobs).
- Exit criteria
  - Dispatch occurs only through governed capability bus pathways (no direct scripts).

# References

- REGISTRIES/allocators.yaml
  - `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md`
  - `TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md`
