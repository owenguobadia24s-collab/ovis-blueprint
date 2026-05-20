---
id: TEMPLATE-TEMP-0002
title: Codex Review Checklist Template
type: TEMPLATE
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: temp
repo: ovis-blueprint
path: TEMPLATES/CODEX_REVIEW_CHECKLIST.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-05-20'
registry: ovis-blueprint/REGISTRIES/entries/TEMPLATE-TEMP-0002.yaml
module_id: MOD-REVIEW-GATE-0001
module_slug: review_gate
system_id: SYS-KERNEL-0001
system_slug: ovis_kernel
---

# Purpose

Provide the reusable template named Codex Review Checklist Template.

# Scope

This file governs or documents Codex Review Checklist Template within the ovis-blueprint repository.

# Content

## CODEX Review Checklist Template

Use this checklist when reviewing Codex outputs (plan or execution) for scope safety, policy compliance, and verification completeness.

This template is aligned with `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md` and
`TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md`.

### Checklist (Minimum)

- Scope validation
  - Output matches the stated objective.
  - No scope expansion beyond the contract.
  - All changes stay within the allowlisted paths.
- Forbidden actions
  - No edits to `REGISTRIES/**`.
  - No direct Notion writes.
  - No work performed in `ovc-*` repos.
- Verification evidence
  - Validation commands were run (or an explicit, justified skip was recorded).
  - Results are reported clearly.
- Output completeness
  - Summary is present.
  - Files changed/created are listed by path.
  - Checks run are listed with results.
  - Assumptions and open issues are captured.
- Review/approval status
  - If this is a Plan Job: approval decision is recorded before issuing an Execute Job.
  - If this is an Execute Job: approval decision is recorded before dispatch.

# References

- REGISTRIES/allocators.yaml
  - `POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md`
  - `TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md`
