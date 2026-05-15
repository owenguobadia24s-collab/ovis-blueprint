---
id: POLICY-GOV-0002
title: OVIS Metadata Standard
type: POLICY
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: gov
repo: ovis-blueprint
path: POLICIES/OVIS_METADATA_STANDARD.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/POLICY-GOV-0002.yaml
module_id: MOD-META-SCHEMA-POLICY-0001
module_slug: meta_schema_policy
system_id: SYS-METADATA-0001
system_slug: metadata_system
related_module_ids:
  - MOD-META-HEADER-0001
  - MOD-META-REGISTRY-0001
---

# Purpose

Define the canonical field set, containment model, document structure, migration phases, ignore policy, and insertion modes for OVIS workspace metadata.

# Scope

This policy governs metadata birth, storage, validation, structural containment, migration, and enforcement rules for the core OVIS repos.

# Content

## Canonical Fields

All in-scope files must carry `id`, `title`, `type`, `status`, `authority`, `version`, `layer`, `domain`, `repo`, `path`, `owner`, `created`, `last_updated`, and `registry`.

## Structural Containment Fields

`module_id` is the primary authoritative containment key for a governed artifact. When `module_id` is declared, the artifact must also declare `module_slug`, `system_id`, and `system_slug` as the canonical containment tuple.

`module_slug` must match the registered slug of `module_id`. `system_id` is stored on the artifact as a denormalized readability and query field, but it is not independently authoritative when `module_id` is present. `system_id` must match the parent system of `module_id` in the structural registry. `system_slug` must match the registered slug of `system_id`.

Containment conflicts fail closed. A governed artifact must not declare system membership that disagrees with the parent system of its primary module.

`related_module_ids` is a secondary advisory association field. It does not define ownership, does not override `module_id`, and must never be treated as identity-defining containment. `related_module_ids` may reference zero or more modules. Duplicate entries are invalid. Repeating the primary `module_id` inside `related_module_ids` is invalid.

Structural containment is introduced now for structural registry records and for new governance artifacts created by the structural taxonomy package. Workspace-wide backfill remains deferred until the file-to-module mapping CJ is completed and approved.

Artifacts created under the active structural mapping regime must carry the full containment tuple. Omission of containment fields is a temporary allowance for legacy or not-yet-mapped artifacts only and is not the long-term standard for newly authored governed files.

Referential integrity for `module_id`, `module_slug`, `system_id`, and `system_slug` against the structural registries is staged. Until structural registry lookup enforcement is introduced, runtime tooling enforces syntactic validity and completeness for the containment tuple but does not claim full registry-backed containment verification.

Relation-registry mirroring for `related_module_ids` is not required in the canonical registries at this stage and may be derived later if needed.

## Identity Separation

Artifact IDs follow `TYPE-DOMAIN-NNNN` and identify the artifact only. Module and system ownership must never be encoded into artifact IDs. ADR-008 is the mandatory authority for this separation, while ADR-007 remains authoritative for runtime/state identity families.

## Registry Authority Interim State

Canonical containment truth now lives in the normalized registry system on committed governed artifacts.

`REGISTRIES/allocators.yaml`, `REGISTRIES/entries/**`, and `REGISTRIES/OVIS_FILE_REGISTRY.yaml` are non-canonical working artifacts during this interim state. They are useful reconciliation inputs, but they are not authoritative sources of truth until a controlled normalization pass regenerates and validates them together.

`REGISTRIES/id_registry.yaml` is a legacy artifact pending separate retirement cleanup. Canonical registry authority now resides in allocator state, entry records, and the aggregate registry as normalized against artifact metadata.

## Insertion Modes

Markdown and text files use YAML frontmatter. Python and other supported source files use comment-prefixed headers. Strict files use sidecars named `<filename>.ovis.yaml`.

When `id: TBD` is used, the `registry` field may temporarily contain the directory placeholder `ovis-blueprint/REGISTRIES/entries/`. This value is transitional, non-canonical, and permitted only for provisional scaffolds. It must be replaced with a concrete entry path when a canonical artifact ID is assigned.

## Document Rule

Document-class files must contain `Purpose`, `Scope`, `Content`, and `References` in that order. Deeper headings are allowed within sections, but additional top-level headings before `References` are invalid.

## Migration Phase

Active migration phase: `M2`. Legacy wrapped or flat metadata is readable only when explicitly enabled and must emit warnings. New and touched files must use canonical metadata.

## Lifecycle Vocabulary

Structural records may use `planned`, `emerging`, `active`, `deprecated`, and `superseded` as lifecycle states. Historical continuity is preserved through `supersedes` and `superseded_by`; structural records are not deleted.

## Relation Authority

Primary relations are `artifact_primary_module` and `module_belongs_to_system`. They are identity-defining and fail closed on inconsistency.

Secondary relations are `artifact_related_module`, `system_spans_repo`, `module_depends_on_module`, `system_depends_on_system`, `adr_governs_target`, and `cj_implements_target`. They are extensible overlays and do not redefine identity.

## Naming Validation

System and module names must describe responsibility rather than act as catch-all containers. Tokens such as `core`, `misc`, `general`, and `utils` are invalid as canonical structural names unless a future ADR explicitly authorizes them for a narrowly defined purpose.

## Ignore Policy

Generated files, vendored content, caches, allocator working state, registry entry working state, legacy registry state, and compiled registry aggregates are excluded from file-level metadata validation.

# References

- ADR-008_WORKSPACE_METADATA_ENVELOPE_AND_REGISTRY_GOVERNANCE.md
- ADR-009_ARTIFACT_IDENTITY_AND_CONTAINMENT_SEPARATION.md
- ADR-010_SYSTEM_MODULE_REPO_REGISTRY_MODEL.md
- validators/OVIS_METADATA_SCHEMA.yaml
