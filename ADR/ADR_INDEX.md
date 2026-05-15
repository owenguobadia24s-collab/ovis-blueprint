---
id: DOC-ARC-0001
title: ADR Index
type: DOC
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: arc
repo: ovis-blueprint
path: ADR/ADR_INDEX.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/DOC-ARC-0001.yaml
module_id: MOD-META-REGISTRY-0001
module_slug: meta_registry
system_id: SYS-METADATA-0001
system_slug: metadata_system
related_module_ids:
  - MOD-META-SCHEMA-POLICY-0001
---

# Purpose

Describe the current accepted ADR set for the OVIS blueprint repository.

# Scope

This index summarizes the authoritative ADR set available in `ovis-blueprint/ADR/`.

# Content

| ADR number | title | status | depends_on | brief summary |
| --- | --- | --- | --- | --- |
| ADR-001 | Canonical State Model and Lineage Contract | Accepted | none | Defines canonical OVIS object types, lineage rules, and append-only audit boundaries. |
| ADR-002 | Function Calling as the Canonical OVIS Capability Bus | Accepted | ADR-001 | Defines function calling as the governed capability bus for executable OVIS-native actions. |
| ADR-003 | Responses API as the Canonical OVIS Runtime | Accepted | ADR-001, ADR-002 | Defines the runtime substrate and wrapper boundary for governed model invocation. |
| ADR-004 | ZDR-First Session Mode and Continuation Policy | Accepted | ADR-001, ADR-002, ADR-003 | Defines ZDR-first continuation behavior and OVIS-owned branch continuity. |
| ADR-005 | Review Gate and Policy Profiles | Accepted | ADR-001, ADR-002, ADR-003, ADR-004 | Defines the canonical execution review gate, risk classes, and policy profiles. |
| ADR-006 | Branch Compaction and Continuity | Accepted | ADR-001, ADR-002, ADR-003, ADR-004, ADR-005 | Defines branch compaction, preserved references, and continuity guarantees. |
| ADR-007 | Canonical Identity and ID Generation | Accepted | ADR-001, ADR-002, ADR-003, ADR-004, ADR-005, ADR-006 | Defines canonical OVIS identity families and generation rules. |
| ADR-008 | Workspace Metadata Envelope and Registry Governance | Accepted | ADR-007 | Defines canonical workspace metadata authority, split registry authority, workspace artifact identity rules, and fail-closed reconciliation. |
| ADR-009 | Artifact Identity and Containment Separation | Proposed | ADR-007, ADR-008 | Separates artifact identity from module/system containment and fixes primary versus related module semantics. |
| ADR-010 | System Module Repo Registry Model | Proposed | ADR-008, ADR-009 | Defines systems, modules, repos, and relation registries as first-class governed objects. |
| ADR-011 | Module Introduction and Taxonomy Governance | Proposed | ADR-009, ADR-010 | Defines when modules may be created, split, merged, deprecated, or superseded. |
| ADR-012 | OVIS OVC Ecosystem Boundary | Proposed | ADR-008, ADR-010 | Defers OVC structure and fixes the current OVIS-only structural scope. |


# References

- ADR/ADR-008_WORKSPACE_METADATA_ENVELOPE_AND_REGISTRY_GOVERNANCE.md
