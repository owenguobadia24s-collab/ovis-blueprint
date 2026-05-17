---
id: TEMPLATE-OVC-0001
title: OVC Repo State Report Template
type: TEMPLATE
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: return
repo: ovis-blueprint
path: TEMPLATES/OVC_INSPECTION/OVC_REPO_STATE_REPORT.md
owner: Owen Vitae
created: '2026-05-17'
last_updated: '2026-05-17'
registry: ovis-blueprint/REGISTRIES/entries/TEMPLATE-OVC-0001.yaml
module_id: MOD-BRIDGE-DISPATCH-0001
module_slug: bridge_dispatch
system_id: SYS-INTEGRATION-0001
system_slug: integration_bridge
---

# Purpose

Provide the reusable template named OVC Repo State Report Template.

# Scope

This template is copied and filled during CJ-RETURN-0005 (read-only OVC inspection).

CJ-RETURN-0004 does not create inspection outputs. CJ-RETURN-0005 must declare its own date-stamped output location and then copy this template there.

# Content

## Report Header

- CJ: CJ-RETURN-0005
- Output location: (declare here; must be outside active repos and outside OVC repos)
- Inspection mode: read-only
- Operator: (name)
- Date: (YYYY-MM-DD)

## Repo Inventory

List each OVC repo inspected and its baseline state.

| repo | remote | default_branch | head_commit | branch_count | tags | notes |
|---|---|---|---|---:|---:|---|
| ovc-infra |  |  |  |  |  |  |
| ovc-infra-fresh |  |  |  |  |  |  |

## Working Tree / Dirty State

Record whether any working-tree changes were present before inspection began (should be none).

## ACL / Access Conditions

Record any operator environment ACL constraints encountered while reading OVC repos (do not “fix” here; report only).

## Summary Findings

Concise summary of high-level repo status, observed risks, and any “do not touch” areas discovered.

# References

- ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
