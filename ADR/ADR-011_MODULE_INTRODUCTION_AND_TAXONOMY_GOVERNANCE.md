---
id: ADR-ARC-0011
title: Module Introduction and Taxonomy Governance
type: ADR
status: proposed
authority: canonical
version: '0.1'
layer: blueprint
domain: arc
repo: ovis-blueprint
path: ADR/ADR-011_MODULE_INTRODUCTION_AND_TAXONOMY_GOVERNANCE.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/ADR-ARC-0011.yaml
module_id: MOD-META-REGISTRY-0001
module_slug: meta_registry
system_id: SYS-METADATA-0001
system_slug: metadata_system
related_module_ids:
  - MOD-META-SCHEMA-POLICY-0001
---

# Purpose

Define when modules may be introduced, expanded, split, merged, deprecated, or superseded.

# Scope

This ADR governs module lifecycle and taxonomy hygiene across the OVIS structural model.

# Content

## Decision

A module is valid only when it satisfies all of the following:

- it has a stable named responsibility
- it has a bounded mutation surface
- it has a coherent dependency profile
- it can be reasoned about independently
- it provides a repeatable append surface for future artifacts

New work must enter an existing module unless doing so would break one of those conditions.

## Lifecycle

Modules and systems are never deleted. They move through `planned`, `emerging`, `active`, `deprecated`, and `superseded`.

Splits and merges are recorded with `supersedes` and `superseded_by` so historical continuity is preserved.

## Governance

New modules and structural splits require an ADR-backed rationale before registry mutation. Catch-all modules and vague containers are prohibited.

# References

- ADR-010_SYSTEM_MODULE_REPO_REGISTRY_MODEL.md
- MODULE_TAXONOMY.md
- POLICIES/OVIS_METADATA_STANDARD.md
