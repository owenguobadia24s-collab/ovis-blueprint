---
id: ADR-ARC-0008
title: Workspace Metadata Envelope and Registry Governance
type: ADR
status: accepted
authority: canonical
version: '0.1'
layer: blueprint
domain: arc
repo: ovis-blueprint
path: ADR/ADR-008_WORKSPACE_METADATA_ENVELOPE_AND_REGISTRY_GOVERNANCE.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/ADR-ARC-0008.yaml
module_id: MOD-META-SCHEMA-POLICY-0001
module_slug: meta_schema_policy
system_id: SYS-METADATA-0001
system_slug: metadata_system
related_module_ids:
  - MOD-META-REGISTRY-0001
---

# Purpose

Record the canonical OVIS decision for workspace artifact metadata format, metadata-governed identity behavior, registry authority, and fail-closed reconciliation behavior.

# Scope

This ADR governs workspace artifact identity grammar, artifact metadata schema authority, artifact containment fields carried on governed files, and canonical allocator/reconcile authority for governed file IDs across `ovis-blueprint`, `ovis-runtime`, and `ovis-knowledge`.

This ADR explicitly governs:

- artifact ID grammar for governed files shaped as `TYPE-DOMAIN-NNNN`
- canonical metadata fields and their semantics
- artifact containment fields on governed files, including `module_id`, `module_slug`, `system_id`, `system_slug`, and `related_module_ids`
- canonical file-registry allocator authority and reconciliation authority for governed file IDs
- the prohibition on encoding module or system containment into artifact IDs

This ADR explicitly does not govern:

- the system/module/repo structural object model
- structural registry semantics beyond artifact-level references to structural objects
- module lifecycle split/merge governance
- OVIS/OVC ecosystem boundary policy
- runtime/state identity families governed by ADR-007

# Content

## Decision

OVIS uses canonical root-level metadata fields with no `OVIS:` wrapper for Phase 0 and the initial ecosystem rollout. This ADR is the mandatory authority for workspace artifact metadata and for metadata-governed workspace artifact IDs shaped as `TYPE-DOMAIN-NNNN`.

Artifact IDs governed by this ADR identify artifacts only. Artifact IDs must not encode module or system containment. Containment is declared only through metadata fields. The presence of containment metadata on an artifact does not make this ADR the authority for the structural object model itself; the structural object model and structural registry semantics are governed separately by ADR-010.

ADR-007 remains authoritative for runtime/state identity families such as `event_id`, `branch_id`, `correlation_id`, `compaction_id`, and job-family identifiers. ADR-008 does not replace those runtime identity rules.

## Registry Model

The normalized end state governed by this ADR keeps allocation authority in `ovis-blueprint/REGISTRIES/allocators.yaml` and governed file records in `ovis-blueprint/REGISTRIES/entries/*.yaml`.

During the post-Wave 1 interim state, `ovis-blueprint/REGISTRIES/allocators.yaml`, `ovis-blueprint/REGISTRIES/entries/*.yaml`, and `ovis-blueprint/REGISTRIES/OVIS_FILE_REGISTRY.yaml` are non-canonical working artifacts. They are useful reconciliation inputs, but they are not authoritative sources of truth until a controlled normalization pass regenerates and validates them together.

After normalization completes, canonical containment truth lives in the normalized registry authority set on committed governed artifacts. `ovis-blueprint/REGISTRIES/id_registry.yaml` is a legacy artifact pending separate retirement cleanup.

Artifacts may reference structural objects through containment metadata such as `module_id` and `system_id`, but ADR-008 does not define the structural object model for systems, modules, or repos. Dedicated structural registries under `ovis-blueprint/REGISTRIES/` are governed by ADR-010.

Partial publication of file-entry or allocator state is not allowed while the registry layer remains in this interim non-canonical posture. Canonical registry authority resumes only after normalization regenerates allocator state, file-entry records, and the aggregate registry from artifact metadata and validates them together.

## Reconciler Rule

Reconciliation is fail-closed. It may update allocator and entry state only after a complete successful scan with no duplicate IDs, no ambiguous repo/path collisions, and no allocator drift.

Reconciliation must not invent or rewrite module/system containment under ambiguity. Primary containment and structural registry identity are authoritative inputs, not silent repair targets.

## Structural Containment

Primary artifact ownership is carried by `module_id`. Optional associative links are carried by `related_module_ids`. Module ownership determines the parent `system_id`.

Containment fields are introduced by this ADR and normalized by follow-on ADRs governing structural taxonomy. Workspace-wide enforcement may be phased, but new structural registry artifacts and new governance artifacts authored under this package must use the containment model immediately.

Registry-backed referential-integrity checks between artifact containment metadata and structural registries are staged. Initial runtime tooling is required to enforce syntactic containment validity and fail closed on contradictions it can prove, but it does not imply full structural-registry lookup enforcement in the initial rollout.

## Migration

Wrapped `OVIS:` and legacy flat `ov_*` metadata are non-canonical. Migration is phased and explicitly governed by policy.

## Consequences

- Metadata schema authority, registry authority, and allocation constraints for workspace artifacts now anchor explicitly under ADR-008.
- Structural containment is metadata-governed and cannot be inferred from artifact IDs.
- New structural registries for systems, modules, repos, and relations are referenced by artifact containment but governed separately.
- Post-Wave 1 artifact metadata is the temporary canonical source of truth for containment until registry normalization is completed.
- `REGISTRIES/allocators.yaml`, `REGISTRIES/entries/*.yaml`, and `REGISTRIES/OVIS_FILE_REGISTRY.yaml` must be treated as non-canonical working artifacts during the interim period.
- Future structural mutation requires the supporting ADR package for containment separation, registry modeling, taxonomy governance, and ecosystem boundaries.

## Module Seal

This ADR is part of `MOD-META-SCHEMA-POLICY-0001`, the metadata schema and policy module inside `SYS-METADATA-0001`.

# References

- ADR-007_CANONICAL_IDENTITY_AND_ID_GENERATION.md
- ADR-009_ARTIFACT_IDENTITY_AND_CONTAINMENT_SEPARATION.md
- ADR-010_SYSTEM_MODULE_REPO_REGISTRY_MODEL.md
- POLICIES/OVIS_METADATA_STANDARD.md
- TASKS/CJ-GOV-0001_WORKSPACE_METADATA_ROLLOUT.md
