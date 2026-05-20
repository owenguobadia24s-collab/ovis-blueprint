---
id: CJ-GOV-0002
title: Modern Notion-Linked Development Protocol
type: CJ
status: draft
authority: operational
version: '0.1'
layer: blueprint
domain: gov
repo: ovis-blueprint
path: TASKS/CJ-GOV-0002_MODERN_NOTION_LINKED_DEV_PROTOCOL.md
owner: Owen Vitae
created: '2026-05-20'
last_updated: '2026-05-20'
registry: ovis-blueprint/REGISTRIES/entries/
module_id: MOD-PLANNING-EXECUTION-LOOP-0001
module_slug: planning_execution_loop
system_id: SYS-KERNEL-0001
system_slug: ovis_kernel
related_module_ids:
  - MOD-REVIEW-GATE-0001
  - MOD-BRIDGE-DISPATCH-0001
---

# Purpose

Codify the modern Notion-linked development protocol in `ovis-blueprint` so Notion remains a command cockpit while canonical authority remains with OVIS runtime and Git.

# Scope

Allowed repo:

- `ovis-blueprint`

Allowed write paths:

- `POLICIES/**`
- `TASKS/**`
- `TEMPLATES/**`
- Optional `ADR/**` only if required to resolve a direct contradiction (avoid if possible)

Required outputs (this CJ owns only these deliverables):

1. `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md`
2. `TASKS/CJ-GOV-0002_MODERN_NOTION_LINKED_DEV_PROTOCOL.md`
3. `TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md`

Optional (alignment-only) updates are permitted if strictly required:

- `TEMPLATES/CODEX_TASK_CONTRACT.md`
- `TEMPLATES/CODEX_REVIEW_CHECKLIST.md`
- `TEMPLATES/CODEX_PROMOTION_GATE.md`

Forbidden actions:

- Do not edit `REGISTRIES/**`.
- Do not inspect or edit any `ovc-*` repos.
- Do not edit `ovis-runtime` or `ovis-knowledge`.
- Do not run legacy Notion scripts.
- Do not implement bridge automation.
- Do not run normalize/reconcile.
- Do not create CJ-RETURN-0005 outputs.

This artifact is provisional until registered by a separate metadata registration CJ. It must not be treated as registry-canonical until registry entry allocation is completed.

# Content

## Deliverables

- Policy: `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md`
  - Defines cockpit vs authority boundaries (Notion vs runtime vs Git).
  - Defines the modern loop and cockpit-to-canonical mapping rules.
  - Defines prohibited actions and hold conditions.
- Template: `TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md`
  - Defines minimum safe job contract for cockpit-issued jobs to be executable by Codex safely.
- CJ record: this document
  - Captures scope boundaries, validation steps, and close conditions.

## Validation (Read-Only)

Run and record results:

- `git status`
- `git diff --stat`
- Confirm only allowed paths changed.
- Metadata validation (read-only) only if already available in the environment:
  - `ovis metadata validate --repo-root . --migration-phase M2`
  - If `ovis` is not available, record: “skipped: ovis CLI not available”.

## Close Conditions

This CJ is eligible to close when:

- All required files exist under allowed paths.
- All required documents include canonical metadata and non-empty sections: Purpose, Scope, Content, References.
- Optional template edits (if any) are alignment-only and do not introduce new doctrine.
- Validation steps are executed and recorded (or explicitly skipped when unavailable).
- No forbidden paths were modified and no forbidden actions were performed.

# References

- `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md`
- `TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md`
- `TEMPLATES/CODEX_TASK_CONTRACT.md`
- `TEMPLATES/CODEX_REVIEW_CHECKLIST.md`
- `TEMPLATES/CODEX_PROMOTION_GATE.md`
- `ADR/ADR-001_CANONICAL_STATE_MODEL_AND_LINEAGE.md`
- `ADR/ADR-002_FUNCTION_CALLING_CANONICAL_CAPABILITY_BUS.md`
