---
id: CJ-GOV-0001
title: Workspace Metadata Rollout
type: CJ
status: draft
authority: canonical
version: '0.1'
layer: blueprint
domain: gov
repo: ovis-blueprint
path: TASKS/CJ-GOV-0001_WORKSPACE_METADATA_ROLLOUT.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/CJ-GOV-0001.yaml
module_id: MOD-META-SCHEMA-POLICY-0001
module_slug: meta_schema_policy
system_id: SYS-METADATA-0001
system_slug: metadata_system
related_module_ids:
- MOD-META-REGISTRY-0001
- MOD-META-RECONCILE-0001
- MOD-META-SCAFFOLD-0001
---

# Purpose

Specify the implementation work needed to convert the core OVIS workspace to the canonical metadata standard.

# Scope

This construction job covers blueprint governance artifacts, runtime tooling, corpus backfill, registry initialization, and CI wiring for the three core repos.

# Content

## Deliverables

- ADR-008 and supporting policy/schema files
- split canonical registry with allocator state and per-file entry records
- runtime metadata parser, scanner, validator, reconciler, and scaffold helper
- CI workflows for blueprint, runtime, and knowledge

## Execution Notes

The rollout must not guess canonical IDs from uncertain allocator state. Any non-deterministic conflict fails closed for human review.

Post-Wave 1, artifact-level metadata on committed governed artifacts is the temporary canonical authority for containment truth. `REGISTRIES/allocators.yaml`, `REGISTRIES/entries/**`, and `REGISTRIES/OVIS_FILE_REGISTRY.yaml` are held as non-canonical working artifacts until a controlled normalization pass regenerates and validates them together.

The registry-resolution path completed as hybrid staged normalization: registry working state was reconciled against artifact metadata, then allocator state, entry records, and aggregate registry were regenerated together in one controlled pass. `REGISTRIES/id_registry.yaml` is now a separate legacy-retirement cleanup item rather than part of canonical registry authority.

CI metadata enforcement remains held until the normalized registry layer becomes canonical. It must not be enabled against the current interim registry working state.

# References

- POLICIES/OVIS_METADATA_STANDARD.md
- ADR/ADR-008_WORKSPACE_METADATA_ENVELOPE_AND_REGISTRY_GOVERNANCE.md
