# [Feature Name] — Implementation Plan

## Reference

- **Specification**: `specs/[feature-spec].md`

*The spec is the source of truth for domain model, endpoint contracts, database schema, configuration, and access matrix. Refer to it for any details not repeated here.*

## Rules

*List the rules files that MUST be read before implementing any slice.*

**Before implementing each slice, you MUST read and internalize:**
- rules/my-rules-1.md
- rules/my-rules-2.md

**If this plan conflicts with the rules, THE RULES WIN.**

## Slices

*Each slice is independently implementable and testable. A fresh context loads the next unchecked slice. Each slice should contain enough context to execute without reading prior slices, but should reference the spec for full details.*

---

### - [ ] Slice 1: [Slice Title]

**Goal:** *One sentence describing what this slice achieves.*

**Spec reference:** *Which sections of the spec are relevant (e.g., "Database Schema > New Table: users").*

**Files to create or modify:**
- `path/to/file.kt` — description of change
- `path/to/migration.sql` — description of change

**Implementation steps:**
1. Step 1
2. Step 2
3. Step 3

**Tests to write:**
- Test description 1 (happy path)
- Test description 2 (unhappy path)

**Acceptance criteria:**
- [ ] Criterion 1
- [ ] Criterion 2

---

### - [ ] Slice 2: [Slice Title]

*Same structure as above. Repeat for each slice.*

---

## Post-Implementation

*Any cleanup, verification, or follow-up tasks to perform after all slices are complete.*

- [ ] Final verification item 1
- [ ] Final verification item 2
