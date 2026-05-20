---
id: TEMPLATE-TEMP-0001
title: Codex Task Contract Template
type: TEMPLATE
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: temp
repo: ovis-blueprint
path: TEMPLATES/CODEX_TASK_CONTRACT.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-05-20'
registry: ovis-blueprint/REGISTRIES/entries/TEMPLATE-TEMP-0001.yaml
module_id: MOD-PLANNING-EXECUTION-LOOP-0001
module_slug: planning_execution_loop
system_id: SYS-KERNEL-0001
system_slug: ovis_kernel
related_module_ids:
- MOD-REVIEW-GATE-0001
---

# Purpose

Provide the reusable template named Codex Task Contract Template.

# Scope

This file governs or documents Codex Task Contract Template within the ovis-blueprint repository.

# Content

## CODEX Task Contract Template

Use this template to define a bounded Codex task contract suitable for safe execution against Git repos.

This template is aligned with `TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md` and the cockpit authority rules in
`POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md`.

### Contract Fields (Minimum)

- Task ID: `TBD`
- Job kind: `Plan|Execute`
- Objective: `TBD`
- Allowed paths (allowlist): `TBD`
- Forbidden actions (denylist): `TBD`
- Validation commands: `TBD`
- Required return format:
  - summary
  - files changed/created
  - checks run + results
  - assumptions
  - open issues

### Contract Template (Fill-In)

- Task ID:
- Job kind:
- Repo:
- Objective:
- Inputs:
- Allowed outputs:
- Allowed paths:
- Forbidden actions:
- Validation commands:
- Required return format:

# References

- REGISTRIES/allocators.yaml
  - `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md`
  - `TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md`
