---
id: POLICY-GOV-0001
title: AI Write Permissions
type: POLICY
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: gov
repo: ovis-blueprint
path: POLICIES/AI_WRITE_PERMISSIONS.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/POLICY-GOV-0001.yaml
module_id: MOD-REVIEW-GATE-0001
module_slug: review_gate
system_id: SYS-KERNEL-0001
system_slug: ovis_kernel
---

# Purpose

Define the policy named AI Write Permissions.

# Scope

This file governs or documents AI Write Permissions within the ovis-blueprint repository.

# Content

## AI Write Permissions

This policy defines the human-readable rulebook for AI-assisted work across OVIS repositories. It governs ChatGPT planning, Codex execution and review, OVIS runtime agents, tool gateway actions, and external connectors. It does not implement runtime enforcement.

## Authority Hierarchy

The authority order is mandatory:

1. Human operator.
2. Git canonical repository state.
3. OVIS runtime governance and validation tooling.
4. Accepted ADRs and canonical policies.
5. Codex job contracts.
6. ChatGPT planning and review output.
7. Notion cockpit records.

If two authorities conflict, the higher authority wins. Notion is a cockpit and mirror, not the source of truth. ChatGPT may plan and review, but must not silently expand scope. Codex may execute only inside an approved CJ contract. OVIS runtime tooling may validate, classify, and later enforce policy. OVC cannot govern OVIS repository authority.

## AI Actor Classes

| Actor | Allowed role | Forbidden role | Write authority | Evidence before action |
| --- | --- | --- | --- | --- |
| `CHATGPT_PLANNER` | Analyze, plan, identify risks, draft review packets | Mutate repos, run side-effecting commands, silently expand scope | None unless a separate approved execution contract grants it | Inputs, assumptions, scope, risks, proposed validation |
| `CODEX_EXECUTOR` | Apply bounded edits under an explicit CJ contract | Work outside allowlists, write to Notion, bypass validation, broaden scope | Only paths and operations granted by the active CJ | Clean-start state, allowed paths, forbidden actions, validation commands |
| `CODEX_REVIEWER` | Review diffs, risks, test gaps, and policy compliance | Make unapproved edits while reviewing | Read-only unless a repair CJ grants scoped edit authority | Diff, commands run, test results, unresolved issues |
| `OVIS_RUNTIME_AGENT` | Validate, classify, emit governed events, route approved actions | Invent doctrine, bypass review gates, treat connector state as canonical | Runtime-owned state only as authorized by runtime policy | Canonical object IDs, correlation IDs, approval state, event lineage |
| `OVIS_TOOL_GATEWAY` | Execute registered capabilities behind schemas, policy checks, and result envelopes | Execute unknown or disallowed side effects, bypass idempotency or audit | Capability-specific authority only | Capability name, schema-valid input, policy decision, idempotency key, event context |
| `EXTERNAL_CONNECTOR` | Perform explicitly approved external actions through governed bridge boundaries | Define OVIS authority, mutate without approval, store secrets in repos | External side-effect authority only when explicitly approved | Operator approval, target system, action payload, rollback or audit evidence |

## Permission Levels

| Level | Allowed | Forbidden | Grantor | Required evidence |
| --- | --- | --- | --- | --- |
| `READ_ONLY` | Inspect files, logs, diffs, command output | Mutate files or external systems | Default for analysis | Sources read and findings |
| `PLAN_ONLY` | Produce plans and review packets | Apply edits, stage, commit, push | Human operator or planning CJ | Decision-complete plan and assumptions |
| `DRAFT_ONLY` | Draft text or patches for review without applying | Change repo state | Human operator | Draft artifact and scope note |
| `SCOPED_EDIT` | Modify explicitly allowlisted paths | Edit outside allowlist or alter unrelated content | Approved CJ contract | Files changed, diff, validation plan |
| `STAGE_ONLY` | Stage exact approved paths | Use broad staging or commit | Approved CJ contract | Staged file list and cached diff |
| `COMMIT_AUTHORIZED` | Commit reviewed staged changes | Commit unrelated or unreviewed changes | Explicit human operator approval | Commit message, staged diff, validation result |
| `PUSH_AUTHORIZED` | Push committed changes to a remote | Force push or push unapproved branches | Explicit human operator approval | Target remote/branch and commit SHA |
| `EXECUTE_LOCAL_COMMANDS` | Run local commands needed for inspection, tests, validation, or bounded work | Run destructive or unrelated commands | CJ contract or operator approval | Command, working directory, result |
| `EXTERNAL_SIDE_EFFECT` | Perform approved external writes or dispatches | Silent external mutation | Explicit human operator approval | Approval, target, payload summary, audit result |
| `FORBIDDEN` | Nothing | Any action | Policy or CJ denylist | Refusal or hold reason |

## Repository Write Zones

| Zone | Examples | Default AI permission |
| --- | --- | --- |
| `LOW_RISK_DOCS` | README files, bounded explanatory docs | `SCOPED_EDIT` when a CJ allowlists the files |
| `GOVERNANCE_DOCS` | ADRs, policies, CJ records, templates | `PLAN_ONLY` by default; `SCOPED_EDIT` only under explicit governance CJ |
| `REGISTRY_METADATA` | `REGISTRIES/entries/**`, allocators, aggregate registry | `READ_ONLY` by default; `SCOPED_EDIT` only under registry repair CJ |
| `RUNTIME_SOURCE` | `ovis-runtime/src/**` | `PLAN_ONLY` by default; implementation CJ required |
| `RUNTIME_TESTS` | `ovis-runtime/tests/**` | `PLAN_ONLY` by default; implementation or test CJ required |
| `VALIDATION_TOOLING` | validators, metadata scanner/reconciler/normalizer | `FORBIDDEN` unless a validator-specific CJ is approved |
| `BRIDGE_AND_CONNECTORS` | Notion bridge, MCP/connector code, dispatch code | `FORBIDDEN` unless an external-side-effect CJ is approved |
| `OVC_DOMAIN_SURFACES` | OVC repos, trading doctrine, chart logic | `FORBIDDEN` from OVIS governance jobs |
| `SECRETS_AND_CREDENTIALS` | tokens, keys, private credentials, env secrets | `FORBIDDEN` |

## Default Rules

- No AI agent may write outside the active CJ allowed paths.
- No AI agent may broaden scope without returning `SPLIT-SCOPE`.
- No AI agent may commit unless commit authority is explicitly granted.
- No AI agent may push unless push authority is explicitly granted.
- No AI agent may modify secrets, credentials, tokens, or private keys.
- No AI agent may perform external side effects without explicit operator approval.
- No AI agent may alter OVC domain logic from an OVIS governance job.
- No AI agent may repair validation tooling inside an unrelated job.
- No AI agent may use broad staging commands unless explicitly authorized.
- No AI agent may treat Notion as canonical source of truth.
- No AI agent may delete registry entries unless a registry-retirement CJ explicitly authorizes it.
- No AI agent may change artifact status, authority, or metadata identity outside an approved metadata or governance job.

## Required CJ Contract Fields

Every AI-executed CJ must include:

- CJ ID
- Job Type
- Objective
- Allowed repositories
- Allowed read paths
- Allowed write paths
- Forbidden actions
- Required steps
- Validation commands
- Return format
- Close / Hold / Split conditions
- Continuity log requirement
- Commit authority
- Push authority

Missing required fields are a hold condition unless the job is read-only planning and the missing field is irrelevant to that plan.

## Exit Decisions

- `CLOSE`: Work is complete, validation and scope checks pass, required evidence is reported, and no required follow-up remains inside the current CJ.
- `HOLD-REPAIR`: Work cannot safely continue because of dirty starting state, validation failure, missing authority, malformed metadata, ambiguous allocator state, or another repairable blocker.
- `SPLIT-SCOPE`: The requested work would require a different job, broader doctrine change, implementation work, registry repair, validator repair, OVC/Notion work, or another scope boundary.
- `PROMOTE-NEXT-CJ`: The current CJ is complete and has identified a separate follow-on CJ that should be opened under its own contract.

## Evidence Requirements

Every AI execution must return:

- files changed
- commands run
- validation result
- diff/stat
- staged files
- open issues
- exit decision
- continuity log line

When a command cannot be run, the agent must report the exact command, why it could not run, and whether the exit decision is `HOLD-REPAIR` or `SPLIT-SCOPE`.

## Staging and Commit Rules

- Prefer exact `git add <path>` commands.
- Avoid `git add .`.
- Avoid `git add <directory>` unless directory-only staging is explicitly safe and authorized.
- Never commit unreviewed changes.
- Never mix unrelated repos in one commit.
- If a CJ touches multiple repos, commit separately per repo.
- Report cached diff before any commit request is considered satisfied.
- If unexpected files appear in `git status`, stop and classify them before staging.

## External Side-Effect Rules

The following require explicit operator approval:

- push to remote
- send email
- write to Notion
- write to calendar
- call production API
- deploy
- modify external database
- trigger bridge dispatch
- publish generated artifacts

External side effects must be routed through governed capability or bridge boundaries when those boundaries exist. Direct connector writes are forbidden unless the active CJ explicitly grants that connector authority and records the required evidence.

## Relationship to Future Enforcement

This policy is the canonical human-readable rulebook for AI write permissions in OVIS.

Future runtime policy hooks, tool gateway checks, or connector gates may enforce this policy. This policy does not implement enforcement, does not change runtime behavior, and does not authorize bridge dispatch. Until enforcement exists, AI agents and human operators must apply this policy through CJ contracts, review gates, validation commands, and explicit evidence reporting.

# References

- ADR/ADR-001_CANONICAL_STATE_MODEL_AND_LINEAGE.md
- ADR/ADR-002_FUNCTION_CALLING_CANONICAL_CAPABILITY_BUS.md
- ADR/ADR-003_RESPONSES_API_CANONICAL_RUNTIME.md
- ADR/ADR-005_REVIEW_GATE_AND_POLICY_PROFILES.md
- ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
- POLICIES/NOTION_COMMAND_COCKPIT_PROTOCOL.md
- POLICIES/OVIS_METADATA_STANDARD.md
- TEMPLATES/CODEX_TASK_CONTRACT.md
- TEMPLATES/CODEX_REVIEW_CHECKLIST.md
- TEMPLATES/CODEX_PROMOTION_GATE.md
- TEMPLATES/NOTION_CODEX_JOB_CONTRACT.md
