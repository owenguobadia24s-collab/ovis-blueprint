---
id: POLICY-GOV-0003
title: Notion Command Cockpit Protocol
type: POLICY
status: draft
authority: operational
version: '0.1'
layer: blueprint
domain: gov
repo: ovis-blueprint
path: POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md
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

Define the modern Notion-linked development protocol and authority boundaries for OVIS.

# Scope

This policy governs:

- how Notion is used as a command cockpit for intake, triage, planning visibility, and review decisions
- how cockpit rows map to canonical OVIS objects defined by ADR-001
- prohibited actions and hard hold conditions for Notion-linked workflows

This policy does not:

- implement any bridge automation, job dispatch, or Notion API tooling
- change OVIS runtime behavior or schemas
- change registry state, allocators, or entry records under `REGISTRIES/**`
- authorize running legacy Notion-era scripts as operational tooling

This artifact is provisional until registered by a separate metadata registration CJ. It must not be treated as registry-canonical until registry entry allocation is completed.

# Content

## Authority Chain (Normative)

The authority chain is mandatory:

1. **Git repos are the source of truth.**
   - Governed artifacts, code, and doctrine live in Git repos.
   - If a cockpit row disagrees with Git, Git is authoritative.
2. **OVIS runtime is the authority for canonical operations.**
   - Canonical state transitions, approvals, event emission, and bridge actions MUST be owned by OVIS runtime and MUST be auditable.
3. **Notion is the command cockpit, not authority.**
   - Notion rows are cockpit inputs and mirrors/projections of canonical objects.
   - Notion schemas and views are operational convenience, not kernel truth.
4. **Codex is a bounded execution satellite.**
   - Codex MAY modify Git repos only under an explicit contract with allowed paths, forbidden actions, and validation commands.
   - Codex MUST NOT write to Notion directly.
5. **ChatGPT is planner/reviewer.**
   - ChatGPT produces plans, review packets, and risk surfacing.
   - ChatGPT MUST NOT perform side effects (no direct Notion writes, no ungoverned dispatch).

## Modern Loop (Normative)

The required lifecycle loop is:

`Signal → Work Object → Plan Job → Review → Execute Job → Review → Event Log`

Normative constraints:

- The loop MUST be representable as canonical object types and transitions as defined by `ADR/ADR-001_CANONICAL_STATE_MODEL_AND_LINEAGE.md`.
- Review decisions captured in Notion MUST be converted into canonical Approval objects by OVIS runtime (Notion decisions are cockpit inputs only).
- Event Log authority is runtime-owned and append-only. A Notion “Event Log” (if present) is a cockpit mirror only.

## Legacy Cockpit Schema (Descriptive; Non-Authoritative)

The legacy Notion-equipped bootstrap (treated as transitional evidence only) used a single Notion parent page containing child databases:

- **OVIS Artifacts**: mirror of repo artifacts identified by `ov_id`, including checksum fields.
- **OVIS Work Objects**: human-facing work registry used to stage planning and execution.
- **OVIS Codex Jobs**: job rows used to stage plan/execute prompts and store outcomes.
- **OVIS Sessions**: optional session tracking.
- **OVIS Signal Compressor**: signal intake + compression fields and routing readiness.
- **OVIS Event Log (v2)**: operational event rows with optional relations to artifacts/work objects/jobs/sessions.

This schema is documented here only to support explicit mapping rules and drift detection. It is not permission to revive or trust legacy scripts.

## Mapping Rules (Normative; ADR-001/ADR-002 Aligned)

These mappings define how cockpit rows relate to canonical objects. They do not make Notion authoritative.

- Notion **Signal Compressor** rows map to canonical **Signal** objects.
- Notion **Work Objects** rows map to canonical **Work Object** objects.
- Notion **Codex Jobs** rows map to canonical **Job** objects.
  - Where present, `sys_job_kind` is a cockpit discriminator for job subtype (e.g., Plan vs Execute). It does not replace canonical job typing in runtime state.
- Notion `sys_review_decision` is a cockpit input only and MUST NOT be treated as canonical Approval.
  - OVIS runtime MUST create canonical Approval objects and emit canonical Events for approval transitions.
- Notion **Event Log** rows map to canonical **Event** objects only if they satisfy canonical lineage and correlation requirements.
  - If lineage/correlation anchors are missing, the row is treated as a cockpit note, not a canonical Event.

### Legacy Script Classification (Evidence Only)

The following legacy scripts are classified as evidence and must not be used as canonical operational tooling without explicit rework behind the capability bus:

| Legacy file | Classification | Canonical capability concept (target) |
| --- | --- | --- |
| `create_ovis_notion_workspace.py` | rebuild | governed “ensure cockpit schema” (bridge-prep only) |
| `create_ovis_signal_compressor.py` | rebuild | governed “ensure signal intake schema” (bridge-prep only) |
| `route_ovis_signals_v2.py` | rebuild | governed “signal → work object + draft job issuance” transition |
| `route_ovis_signals_to_work_objects.py` | deprecate | historical stage; superseded routing behavior |
| `create_ovis_plan_jobs.py` | rebuild | governed plan job issuance from work object state |
| `create_ovis_execute_jobs.py` | rebuild | governed execute job issuance from approved plan + Approval object |
| `activate_ovis_codex_jobs.py` | rebuild | governed activation gate (policy checks + event emission) |
| `ovis_bridge_v0_2.py` | quarantine | unsafe local dispatch hook runner; legacy evidence only |
| `run_ovis_pipeline.py` | quarantine | legacy orchestrator; encourages non-canonical execution |
| `create_ovis_event_log_v2.py` | rebuild | governed event schema mirror (cockpit-only) |
| `sync_ovis_artifacts.py` | rebuild | governed artifact mirror from Git source-of-truth |
| `sync_ovis_artifacts_patched.py` | salvage | evidence for status normalization rules (non-canonical tooling) |
| `upgrade_ovis_codex_jobs_schema.py` | deprecate | one-off migration helper; not a canonical workflow |

## Prohibited Actions (Hard)

The following are forbidden:

- Codex (or any execution satellite) writing to Notion directly.
- Treating Notion rows or fields as canonical state authority.
- Running or reviving legacy Notion-era scripts as operational tooling.
- Bridge dispatch or external side effects that bypass the canonical capability bus described by `ADR/ADR-002_FUNCTION_CALLING_CANONICAL_CAPABILITY_BUS.md`.
- Any change that requires editing `REGISTRIES/**` as part of this workflow.

## Hold Conditions (Hard)

If any hold condition is met, work MUST stop and escalate for human review:

- A required change would edit `REGISTRIES/**` (allocators, entries, aggregates).
- The protocol cannot be implemented without running legacy Notion scripts or performing direct Notion writes.
- Any dependency on inspecting or modifying `ovc-*` repos appears.
- A contradiction with ADR-001 or ADR-002 is discovered that would require new doctrine rather than codification.

# References

- `ADR/ADR-001_CANONICAL_STATE_MODEL_AND_LINEAGE.md`
- `ADR/ADR-002_FUNCTION_CALLING_CANONICAL_CAPABILITY_BUS.md`
- `REPO_ROLE_MAP.md`
- `ovis-notion-bootstrap/*` (legacy evidence set only)
