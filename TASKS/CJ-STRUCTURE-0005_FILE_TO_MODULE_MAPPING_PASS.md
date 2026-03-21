---
id: CJ-STRUCTURE-0005
title: File to Module Mapping Pass
type: CJ
status: planned
authority: canonical
version: '0.1'
layer: blueprint
domain: structure
repo: ovis-blueprint
path: TASKS/CJ-STRUCTURE-0005_FILE_TO_MODULE_MAPPING_PASS.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/CJ-STRUCTURE-0005.yaml
module_id: MOD-META-REGISTRY-0001
module_slug: meta_registry
system_id: SYS-METADATA-0001
system_slug: metadata_system
---

# Purpose

Produce the non-mutating candidate ownership map required before containment backfill.

# Scope

This CJ covers inspection and mapping only. It does not authorize silent metadata rewrite or reconcile-based reassignment.

# Content

The mapping pass must surface:

- current candidate primary module for each governed artifact
- ambiguous ownership cases
- required module splits or taxonomy refinements

# References

- ADR/ADR-009_ARTIFACT_IDENTITY_AND_CONTAINMENT_SEPARATION.md
- ADR/ADR-011_MODULE_INTRODUCTION_AND_TAXONOMY_GOVERNANCE.md
