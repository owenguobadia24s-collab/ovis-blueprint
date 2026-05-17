---
id: ADR-ARC-0012
title: OVIS OVC Ecosystem Boundary
type: ADR
status: accepted
authority: canonical
version: '0.1'
layer: blueprint
domain: arc
repo: ovis-blueprint
path: ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-05-17'
registry: ovis-blueprint/REGISTRIES/entries/ADR-ARC-0012.yaml
module_id: MOD-BRIDGE-DISPATCH-0001
module_slug: bridge_dispatch
system_id: SYS-INTEGRATION-0001
system_slug: integration_bridge
---

# Purpose

Define the canonical separation boundary between OVC and OVIS so OVIS remains stable as governor before any OVC inspection, classification, or recovery work begins.

# Scope

This ADR governs:

- the OVC/OVIS system separation boundary
- repo role and authority boundaries between OVC and OVIS
- forbidden collapse rules (OVC must not govern repos; OVIS must not become trading doctrine)
- the allowed cross-system bridge surface (metadata and governance only)
- the rules and constraints for the first read-only OVC inspection (CJ-RETURN-0005)

This ADR does not recover, normalize, or rewrite any OVC repos. It defines the rules OVIS must follow before inspecting OVC.

# Content

## Core Diagnosis

OVC became tangled because trading doctrine, chart language, symbolic structure, repo implementation, scripts, data pipelines, and recovery attempts collapsed into one web.

OVIS was created to prevent this drift. OVIS itself became entangled by absorbing OVC recovery work before its own authority boundaries were stabilized.

Therefore, before inspecting OVC, OVIS must define and enforce what belongs to each system and what is permitted at the bridge.

## System Definitions

### OVC

OVC means Owen Vitae Chart.

OVC is the market-reading and trading-structure system.

OVC is allowed to contain:

- trading methodology
- chart language
- function catalog and variations
- TPO/profile interpretation
- day types
- liquidity behaviors
- energy dynamics
- session/time-block logic
- Pine scripts
- OANDA/market data tooling
- chart execution artifacts
- trading journals and screenshots

OVC answers:

- What is the market doing?
- What function is active?
- What evidence confirms the function?
- What is the next likely function?
- Is this tradeable?
- What invalidates the read?

### OVIS

OVIS means Owen Vitae Infrastructure System.

OVIS is the governance, metadata, registry, runtime, and authority system.

OVIS is allowed to contain:

- ADRs
- policies
- metadata standards
- structural registries
- validators and schema definitions
- normalizers (as a controlled, separately authorized operation)
- event log tooling
- runtime gateway and provider adapters
- operator CLI
- branch lifecycle logic
- compaction logic
- knowledge extraction rules
- audit rules
- promotion and review gates

OVIS answers:

- What is canonical?
- What is derived?
- What is operational?
- What changed?
- What approved it?
- Where is the event recorded?
- Which artifact owns authority?
- Which repo/layer is allowed to mutate?

## Forbidden Collapse

- OVC must not become the governor of repositories.
- OVIS must not become a trading doctrine.
- OVC may produce trading artifacts.
- OVIS may govern the storage, metadata, validation, promotion, and audit of those artifacts.
- The OVIS/OVC bridge must not contain trading interpretation itself.

## Repo Boundary

| Repo | System | Role | Authority |
|---|---|---|---|
| ovis-blueprint | OVIS | Doctrine, ADRs, policy, metadata, registries | Canonical |
| ovis-runtime | OVIS | CLI, validators, runtime tools, executable infrastructure | Operational |
| ovis-knowledge | OVIS | Derived summaries, archive, knowledge extraction | Derived/advisory |
| ovc-infra | OVC / Legacy | Transitional OVC implementation history | Quarantined |
| ovc-infra-fresh | OVC / Divergent | Divergent OVC implementation state | Quarantined |
| Any OVC chart docs | OVC | Trading doctrine and chart interpretation | Not OVIS authority |
| Any OVC scripts | OVC | Trading implementation | Not OVIS authority |

## Authority Boundary

### OVIS Authority

OVIS has authority over:

- metadata envelopes
- registry IDs and repo classification
- promotion gates and artifact lifecycle
- validation rules
- audit trails
- runtime execution control
- branch/compaction/event-log rules

### OVC Authority

OVC has authority over:

- trading function definitions
- chart classification rules
- TPO/profile forms
- liquidity behavior taxonomy
- execution grading
- session/time-block logic
- market examples
- strategy interpretation

### Shared Boundary (OVC ↔ OVIS Bridge)

The bridge between OVC and OVIS may only contain:

- artifact metadata (envelopes only)
- file registration and repo/path mapping
- validation status
- promotion status
- source path
- checksum/hash
- version
- authority classification
- event log reference

The bridge must not contain chart interpretation, strategy logic, or trading meaning-layer content.

## OVC Inspection Rules (CJ-RETURN-0005)

Before inspecting OVC, classify each discovered artifact as one of:

1. OVC Doctrine
2. OVC Implementation
3. OVC Archive
4. OVC Data
5. OVC Examples/Screenshots
6. OVC Journals
7. Cross-System Bridge
8. Non-Canonical / Unresolved

First inspection is read-only:

- no promotion
- no merge
- no delete
- no rewrite
- no rebase
- no force push

**CJ-RETURN-0005 must declare its own date-stamped output location when it opens.**

Preferred output location characteristics:

- outside active repos (`ovis-blueprint`, `ovis-runtime`, `ovis-knowledge`)
- outside OVC repos (`ovc-infra`, `ovc-infra-fresh`)

Recommended example path format (Windows; example only; not created by CJ-RETURN-0004):

`C:\\Users\\Owner\\OVIS_RETURN_REPORTS\\ovc\\<YYYY-MM-DD>\\`

## OVIS Protection Rules (during OVC inspection)

OVIS must not be changed during initial OVC inspection except for:

- creating the inspection report artifacts in the CJ-RETURN-0005 output location
- recording classification results
- logging unresolved questions
- noting required future governance jobs

No OVIS architecture changes are allowed during OVC inspection.

No metadata normalization is allowed during OVC inspection unless a separate approved CJ is opened.

## Branch Boundary

### OVIS

Active branches:

- ovis-blueprint/main
- ovis-runtime/ovc
- ovis-knowledge/main

Rescue branches are retained as recovery evidence.

### OVC

Current treatment:

- ovc-infra = legacy/transitional, inspect later
- ovc-infra-fresh = divergent/quarantined, inspect later
- no merging
- no rebasing
- no deletion
- no promotion until classification is complete

## First OVC Inspection Output (CJ-RETURN-0005)

The first OVC inspection must produce (reports only):

- OVC_REPO_STATE_REPORT.md
- OVC_ARTIFACT_CLASSIFICATION_LEDGER.md
- OVC_CANONICAL_CANDIDATES.md
- OVC_LEGACY_ARCHIVE_CANDIDATES.md
- OVC_OVIS_BRIDGE_CANDIDATES.md

No mutation of OVC repos is permitted during CJ-RETURN-0005.

## Success Criteria

CJ-RETURN-0004 is complete when:

- OVC and OVIS are formally distinguished
- repo roles are defined
- authority boundaries are written
- forbidden collapse rules are stated
- first OVC inspection rules are defined
- no OVC repo has been modified
- no OVIS architecture has been changed (beyond adopting this boundary doctrine)

Result: **BOUNDARY DEFINED**

Then the next job may begin: **CJ-RETURN-0005 — Read-Only OVC Repo State Inspection**

# References

- ADR-008_WORKSPACE_METADATA_ENVELOPE_AND_REGISTRY_GOVERNANCE.md
- ADR-010_SYSTEM_MODULE_REPO_REGISTRY_MODEL.md
- REPO_ROLE_MAP.md
