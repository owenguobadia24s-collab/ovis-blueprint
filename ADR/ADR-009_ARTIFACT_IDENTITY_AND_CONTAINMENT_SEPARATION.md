---
id: ADR-ARC-0009
title: Artifact Identity and Containment Separation
type: ADR
status: proposed
authority: canonical
version: '0.1'
layer: blueprint
domain: arc
repo: ovis-blueprint
path: ADR/ADR-009_ARTIFACT_IDENTITY_AND_CONTAINMENT_SEPARATION.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/ADR-ARC-0009.yaml
module_id: MOD-META-SCHEMA-POLICY-0001
module_slug: meta_schema_policy
system_id: SYS-METADATA-0001
system_slug: metadata_system
related_module_ids:
  - MOD-META-REGISTRY-0001
---

# Purpose

Define the canonical separation between artifact identity and structural containment.

# Scope

This ADR governs workspace artifact IDs, primary versus related module semantics, and the prohibition on deriving containment from artifact IDs.

# Content

## Decision

Artifact identity and structural containment are separate governance concerns.

Workspace artifact IDs identify the artifact itself and follow the metadata-governed `TYPE-DOMAIN-NNNN` pattern authorized by ADR-008.

Containment is declared through metadata, not inferred from artifact IDs. Every governed artifact may declare:

- exactly one primary module through `module_id`
- zero or more associative module links through `related_module_ids`
- the owning system through `system_id`

Primary ownership determines canonical placement. Related modules do not transfer ownership.

## Implications

- Artifact IDs must not encode module or system membership.
- Structural placement is fail-closed when containment fields are inconsistent or partial.
- Registry records for systems, modules, and repos may use subject-prefixed keys such as `subject_module_id` and `subject_system_id` so record identity stays distinct from file-artifact ownership metadata.

## Consequences

- Metadata schema must validate containment fields as a coherent set.
- File-to-module mapping becomes the path for backfilling containment onto existing artifacts.
- Structural registries can evolve without forcing artifact ID renumbering.

# References

- ADR-008_WORKSPACE_METADATA_ENVELOPE_AND_REGISTRY_GOVERNANCE.md
- ADR-010_SYSTEM_MODULE_REPO_REGISTRY_MODEL.md
- POLICIES/OVIS_METADATA_STANDARD.md
