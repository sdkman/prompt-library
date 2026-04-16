# [Feature Title]

*Brief paragraph: what problem does this solve, what is the change, and why does it matter. Focus on the API consumer's perspective — the "outside view." Keep it to 2-3 sentences.*

*Reference: [link to issue or ticket]*

## Behaviour

*Narrative description of the feature from the API consumer's perspective. No implementation detail. A product owner or domain expert should be able to read this section and confirm "yes, that's what I want."*

*Cover: what the API consumer can do that they couldn't before, the interaction model, key constraints they'd notice (authentication, caching, rate limiting, etc).*

## API Contract

*The precise HTTP interface specification: method, path, parameters, request/response shapes, and status codes.*

*This section is the machine-readable complement to the narrative Behaviour section. Together they eliminate ambiguity.*

### Request

```
METHOD /path/{param}?query={value}
```

| Parameter | In    | Required | Description |
|-----------|-------|----------|-------------|
| `param`   | path  | yes      | ...         |
| `query`   | query | no       | ...         |

### Response

| Status   | Body             | When              |
|----------|------------------|-------------------|
| `200 OK` | ...              | ...               |
| `404`    | ...              | ...               |

### Response Body

*Define the response body shape — fields, types, and structure. This should be complete enough to generate an OpenAPI schema from. Use a JSON example with representative values.*

```json
{
  "field": "string",
  "nested": {
    "field": "number"
  }
}
```

## Business Rules

*Numbered, testable rules — each one a discrete behavioural decision. These are the edge cases and design choices already resolved. The planner uses these to scope the work. The builder uses these to know what to test.*

*Each rule should be verifiable from the outside without knowledge of internals. Avoid referencing tables, columns, implementation patterns, or internal architecture.*

1. **Rule name.** Description of the rule and when it applies.
2. **Rule name.** Description of the rule and when it applies.
3. **Rule name.** Description of the rule and when it applies.

## Examples

*Concrete Gherkin scenarios that make the business rules tangible. Use realistic data. Each scenario should illustrate a specific rule or combination of rules.*

*These translate directly into acceptance test scenarios. Make the abstract rules unambiguous.*

```gherkin
Feature: [Feature title]

  Scenario: [Descriptive scenario name]
    Given ...
      And ...
    When ...
    Then ...
      And ...

  Scenario: [Descriptive scenario name]
    Given ...
    When ...
    Then ...
```

## Out of Scope

*Explicit boundaries — what this feature does NOT do. Prevents gold-plating and scope creep, which is especially important for autonomous agent loops that may otherwise "improve" beyond the spec.*

- Not doing X
- Not changing Y
- Not affecting Z

## Acceptance Criteria

*Verifiable checklist — the definition of done. Each item should be confirmable by running the system or the validation chain. Include the quality gate as the final item.*

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] All quality gates pass (build, lint, tests)
