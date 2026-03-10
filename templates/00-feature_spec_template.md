# [Feature Name] Specification

## Overview

*Write a brief paragraph describing the high-level purpose and context. What problem does this solve? What is the main objective? What is the current state and what changes? Keep this concise and focused on the "why" rather than the "how".*

*End with a **Key Properties** list summarizing the most important architectural/design decisions at a glance.*

**Key Properties:**
- Key property 1
- Key property 2
- Key property 3

## Requirements

*List the specific, measurable acceptance criteria that define when this feature is complete. These should be testable and unambiguous. Think of these as your definition of done for the entire feature — not individual slices.*

- Requirement 1
- Requirement 2
- Requirement 3

## Rules

*Specify any rules files that should be read and internalized before implementing any slice of this feature. The rules files are usually found under the `rules/` directory of this project.*

**Before implementing, you MUST read and internalize:**
- rules/my-rules-1.md
- rules/my-rules-2.md

**If a oneshot prompt conflicts with the rules, THE RULES WIN.**

## Domain

*Describe the core domain model changes using pseudo-code in the project's primary language. Include new types, modified types, interfaces, and value objects. This is the canonical reference that all oneshot prompts will point back to.*

*Focus on types and their relationships — not implementation details.*

## Database Schema

*Define new tables, modified tables, and migration ordering. Include the full SQL for each migration. Call out design decisions (e.g., why a column is NOT NULL, why a sentinel value is used instead of nullable).*

*Specify which migration version numbers to use (check existing migrations to determine the next available).*

## Endpoints

*For each new or modified endpoint, specify:*
- *HTTP method and path*
- *Authentication requirements*
- *Request body shape (with JSON example)*
- *Response body shape for each status code (with JSON example)*
- *Behaviour notes (e.g., idempotency, precedence rules)*

*Group endpoints logically (e.g., by subdomain: auth, admin, core).*

## Access Matrix

*A table showing which user types can access which endpoints. This is the single source of truth for authorization decisions.*

| Endpoint | Anonymous | User Type 1 | User Type 2 |
|----------|-----------|-------------|-------------|
| GET /example | Yes | Yes | Yes |
| POST /example | No | Scoped | Yes |

## Configuration

*Document all configuration changes: what is added, what is removed, what is retained. Include the HOCON/YAML/properties format with environment variable overrides.*

*Show the impact on existing config classes — before and after.*

## Extra Considerations

*List important factors that need special attention across all slices. These are cross-cutting concerns, edge cases, security considerations, and gotchas that don't belong to a single slice.*

- Consideration 1
- Consideration 2
- Consideration 3

## Testing Considerations

*Describe the testing strategy for the entire feature. Group tests by functional area (not by slice). Each test should specify happy/unhappy paths. Individual oneshot prompts will cherry-pick relevant tests from this section.*

*Specify the testing framework, style, and any test infrastructure requirements.*

## Specification by Example

*Provide concrete HTTP request/response examples for the most important flows. These make the abstract requirements tangible and serve as acceptance criteria that can be directly translated into tests.*

*Cover: happy paths, error cases, edge cases, and any flows where the behaviour might be ambiguous from the requirements alone.*

## Suggested Slice Breakdown

*Propose a decomposition into independently implementable and testable oneshot prompts. Each slice should be committable on its own. List them in dependency order — earlier slices are prerequisites for later ones.*

*This section is advisory — the actual oneshot prompts may be structured differently based on implementation experience.*

1. **Slice name** — brief description of scope
2. **Slice name** — brief description of scope
3. **Slice name** — brief description of scope

## Verification

*Create a checklist to verify that the entire feature is complete and working correctly. These should be actionable items that can be checked off systematically after all slices are implemented.*

- [ ] Verification item 1
- [ ] Verification item 2
- [ ] Verification item 3
