---
id: CJ-STRUCTURE-0004
title: Metadata Schema Extension for Module and System Membership
type: CJ
status: planned
authority: canonical
version: '0.1'
layer: blueprint
domain: structure
repo: ovis-blueprint
path: TASKS/CJ-STRUCTURE-0004_METADATA_SCHEMA_EXTENSION_FOR_MEMBERSHIP.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/CJ-STRUCTURE-0004.yaml
module_id: MOD-META-SCHEMA-POLICY-0001
module_slug: meta_schema_policy
system_id: SYS-METADATA-0001
system_slug: metadata_system
related_module_ids:
  - MOD-META-REGISTRY-0001
---

# Purpose

Extend the metadata policy and schema so artifact containment is machine-readable.

# Scope

This CJ governs the schema and policy work only. It does not authorize workspace-wide containment backfill.

# Content

The extension introduces:

- `module_id`
- `module_slug`
- `system_id`
- `system_slug`
- `related_module_ids`

Workspace-wide enforcement remains deferred until the mapping pass is approved.

# References

- POLICIES/OVIS_METADATA_STANDARD.md
- validators/OVIS_METADATA_SCHEMA.yaml
