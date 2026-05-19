---
type: reference
created: 2026-04-29
last_updated: 2026-04-29
tags:
  - architecture
  - backend
  - tagging
  - best-practices
---

# ChiroHD Tagging Best Practices

Reference guide for the generic tagging system adopted 2026-04-29. Based on AWS's tagging best practices whitepaper, adapted for ChiroHD's multi-tenant architecture.

## Core Concept

A tag is a **key-value pair** applied to any entity in the system. The key is the question ("What type of visit is this?"), the value is the answer ("re-exam"). This replaces the older group-label pattern where a "tag group" contained a list of "tags" that were just labels.

## Schema Overview

```
public schema
├── tag_entity_type          — lookup: which entity types can be tagged
└── tag_key_definition       — system-wide key definitions (seed table)

network_{id} schema
├── tag_key_definition       — per-network keys (synced from public + custom)
└── tag                      — all tag assignments for all entity types
```

### tag_entity_type (public)

Lookup table of valid entity types. Add a row here when a new entity becomes taggable.

| Column | Purpose |
|--------|---------|
| `code_name` | Machine identifier (`appointment`, `appointment_type`) |
| `name` | Display name (`Appointment`, `Appointment Type`) |

### tag_key_definition (public + network)

Defines what tag keys exist and how their values are constrained.

| Column | Purpose |
|--------|---------|
| `key_name` | Namespaced identifier (`appt:visit-type`) |
| `display_name` | Human-readable label (`Visit Type`) |
| `tag_entity_type_id` | Scopes key to an entity type (null = any entity) |
| `value_type` | `enum`, `freetext`, or `pattern` |
| `allowed_values` | For enums: `{new-patient,re-exam,adjustment}` |
| `value_pattern` | For patterns: regex string |
| `system_managed` | `TRUE` = ChiroHD-owned (migrations only), `FALSE` = practice-owned |

**Public table**: Seed definitions. Default `system_managed = TRUE`. Not read at runtime — used for provisioning new networks and as a migration reference.

**Network table**: Runtime definitions. Has `source_tag_key_definition_id` back-reference to public, plus `created_by_user_id` audit fields.

### tag (network)

Single table for all tag assignments across all entity types.

| Column | Purpose |
|--------|---------|
| `tag_entity_type_id` | FK to `public.tag_entity_type` |
| `entity_id` | ID of the tagged entity (FK enforced in app code) |
| `tag_key_definition_id` | FK to `network.tag_key_definition` |
| `tag_value` | The value (nullable for boolean/label-style tags) |

Unique constraint: `(tag_entity_type_id, entity_id, tag_key_definition_id, tag_value)`.

## Key Naming Convention

Use **two-level `domain:attribute`** format:

```
domain:attribute
```

| Domain | Example Keys | Used For |
|--------|-------------|----------|
| `appt` | `appt:visit-type`, `appt:billing-class` | Appointment classification |
| `trp` | `trp:rof`, `trp:r4`, `trp:r5`, `trp:r6` | TRP coaching metrics |
| `report` | `report:category` | Report classification |
| `user` | `user:label` | User-generated freeform labels |

### Rules

- Lowercase, kebab-case: `appt:visit-type` not `Appt:VisitType`
- Domain is the broad category, attribute is the specific concept
- Prefix queries are efficient: `WHERE key_name LIKE 'appt:%'` uses B-tree index
- Expandable to three-level (`chd:appt:visit-type`) later if org-level namespacing is needed

## How to Add a New Tag Key

### System-managed key (shipped to all networks)

1. Add a row to `public.tag_key_definition` in a migration
2. Loop through all `network_%` schemas to insert the corresponding row with `system_managed = TRUE` and `source_tag_key_definition_id` pointing back to the public row
3. Regenerate `db-interface.ts`

### Practice-created key (single network)

API endpoint inserts directly into `network_{id}.tag_key_definition` with `system_managed = FALSE` and `source_tag_key_definition_id = NULL`.

## How to Add a New Taggable Entity

1. Add a row to `public.tag_entity_type` (e.g., `('Patient Case', 'patient_case')`)
2. That's it — the `tag` table already supports it via `tag_entity_type_id` + `entity_id`

## Validation Rules (API Layer)

When assigning a tag, the API must check:

1. **Key exists**: `tag_key_definition_id` must reference an active definition
2. **Entity type matches**: If the definition has a `tag_entity_type_id`, the tag's entity type must match (null on definition = any entity allowed)
3. **Value is valid**:
   - `enum`: value must be in `allowed_values` array
   - `pattern`: value must match `value_pattern` regex
   - `freetext`: any non-null string accepted
4. **System-managed protection**: Definitions with `system_managed = TRUE` cannot be edited or deactivated by practice users

## Querying Tags

```sql
-- All tags on a specific appointment
SELECT t.*, tkd.key_name, tkd.display_name
FROM tag t
JOIN tag_key_definition tkd ON t.tag_key_definition_id = tkd.id
JOIN public.tag_entity_type tet ON t.tag_entity_type_id = tet.id
WHERE tet.code_name = 'appointment' AND t.entity_id = 1234;

-- All appointments with a specific tag key and value
SELECT t.entity_id
FROM tag t
JOIN tag_key_definition tkd ON t.tag_key_definition_id = tkd.id
JOIN public.tag_entity_type tet ON t.tag_entity_type_id = tet.id
WHERE tet.code_name = 'appointment'
  AND tkd.key_name = 'appt:visit-type'
  AND t.tag_value = 're-exam';

-- All tags in a domain (prefix query, B-tree indexable)
SELECT tkd.key_name, t.tag_value, t.entity_id
FROM tag t
JOIN tag_key_definition tkd ON t.tag_key_definition_id = tkd.id
WHERE tkd.key_name LIKE 'trp:%';
```

## Relationship to Existing Tag Systems

| System | Status | Relationship |
|--------|--------|-------------|
| `patient_tag` / `patient_patient_tag_join` | Active, untouched | Separate system for patient-level clinical tags. Not migrated. |
| `system_tag` / `patient_case_system_tag_join` | Active, untouched | Event journal for case status transitions. Misleading name. |
| `custom_report_tag` | Active, untouched | Lightweight freeform tags for custom report organization. |
| Generic `tag` (this system) | New | Key-value tagging for entity classification, reporting, and future user-facing features. |

These systems are independent. Unifying them is a future project, not in scope.

## Reference

- AWS Tagging Best Practices Whitepaper: https://docs.aws.amazon.com/whitepapers/latest/tagging-best-practices/tagging-best-practices.html
- Decision record: [[2026-04-29 Adopt AWS key-value tagging model]]
- Migration: `chirohd-api/sculpture/version/1776947699-reporting-tags/`
