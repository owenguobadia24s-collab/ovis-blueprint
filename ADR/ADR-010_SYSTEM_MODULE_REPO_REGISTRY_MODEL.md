---
id: ADR-ARC-0010
title: System Module Repo Registry Model
type: ADR
status: proposed
authority: canonical
version: '0.1'
layer: blueprint
domain: arc
repo: ovis-blueprint
path: ADR/ADR-010_SYSTEM_MODULE_REPO_REGISTRY_MODEL.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/ADR-ARC-0010.yaml
module_id: MOD-META-REGISTRY-0001
module_slug: meta_registry
system_id: SYS-METADATA-0001
system_slug: metadata_system
related_module_ids:
  - MOD-META-SCHEMA-POLICY-0001
---

# Purpose

Define systems, modules, repos, and relations as first-class governed registry objects.

# Scope

This ADR governs systems, modules, and repos as first-class structural objects in `ovis-blueprint`, together with the structural registry spaces and relation recording model that connect artifacts, modules, systems, repos, ADRs, and CJs.

This ADR explicitly governs:

- systems, modules, and repos as first-class structural objects
- canonical structural registry spaces under `REGISTRIES/systems/`, `REGISTRIES/modules/`, `REGISTRIES/repos/`, and `REGISTRIES/relations/`
- subject-prefixed keys such as `subject_system_id`, `subject_module_id`, and `subject_repo_id` for structural-record files
- the primary versus secondary structural relation model
- structural registry semantics for object records and relation registries

This ADR explicitly does not govern:

- artifact ID grammar for governed file artifacts
- allocator authority for governed file IDs
- runtime/state identity families
- artifact metadata backfill execution itself

# Content

## Decision

ADR-010 governs the structural object model, not artifact identity grammar. Artifact IDs and artifact-level metadata fields remain governed by ADR-008. Where a governed file records a structural object, the file remains an artifact under ADR-008 while the described system, module, or repo object is governed by ADR-010.

Structural knowledge is stored canonically in dedicated registry spaces under `ovis-blueprint/REGISTRIES/`:

- `systems/`
- `modules/`
- `repos/`
- `relations/`

Each registry file is itself a governed artifact and therefore carries file-level metadata. The governed object described by the file is recorded with subject-prefixed keys such as `subject_system_id`, `subject_module_id`, and `subject_repo_id`.

## Primary Relations

The following relations are authoritative for structural containment and must remain fail-closed:

- `artifact_primary_module`
- `module_belongs_to_system`

These relations define containment and must fail closed on inconsistency.

## Secondary Relations

The following relations are overlay relationships:

- `artifact_related_module`
- `system_spans_repo`
- `module_depends_on_module`
- `system_depends_on_system`
- `adr_governs_target`
- `cj_implements_target`

These relations are secondary overlays. They may evolve without redefining artifact identity or structural containment ownership.

## Registry Model

Systems, modules, and repos each receive one canonical record. Relations are stored in dedicated relation registries rather than embedded implicitly in folder layout.

# References

- ADR-008_WORKSPACE_METADATA_ENVELOPE_AND_REGISTRY_GOVERNANCE.md
- ADR-009_ARTIFACT_IDENTITY_AND_CONTAINMENT_SEPARATION.md
- RELATION_RULES.md
