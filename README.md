# Prompt Library

A curated collection of development rules and prompt templates for AI-assisted software development. Drop it into a project as a submodule and reference its rules and templates from your prompts to get consistent, opinionated output from AI coding assistants.

## Quick Start

Add this library to your project as a git submodule:

```bash
git submodule add https://github.com/yourusername/prompt-library.git prompts
git submodule update --init --recursive

# Later, pull updates
git submodule update --remote prompts
```

## Contents

### Rules (`rules/`)

Opinionated standards an assistant should follow when writing code. Each rule is self-contained and includes context (where it applies), core principles, MoSCoW-prioritised rules, patterns and anti-patterns, a decision framework, and a TL;DR.

- `kotlin.md` — Functional Kotlin with Arrow (`Option<A>`, `Either<E,A>`, no nullable types)
- `rust.md` — Modern idiomatic Rust (2025 practices)
- `domain-driven-design.md` — DDD tactical and strategic patterns
- `hexagonal-architecture.md` — Ports & adapters
- `simple-design.md` — Beck/Fowler four rules of simple design (language-agnostic)
- `kotest.md` — Kotest + Testcontainers conventions
- `mcp-best-practices.md` — Designing and implementing MCP servers and clients

### Templates (`templates/`)

Prompt scaffolds for common AI development tasks. Italicised paragraphs inside each template are instructions to the author — replace them with real content when filling the template in.

- `00-ralph_api_template.md` — Feature spec for autonomous coding loops (behaviour, API contract, business rules, Gherkin examples, out-of-scope, acceptance criteria)
- `00-mcp-server_template.md` — Prompt for building an MCP server feature/tool
- `00-feature_spec_template.md` — Full feature specification (domain, schema, endpoints, access matrix, examples)
- `00-implementation_plan_template.md` — Slice-by-slice plan derived from a spec
- `00-backend-prompt-template.md` — Prompt for implementing a single backend slice
- `00-frontend-prompt-template.md` — Prompt for implementing a single frontend slice
- `00-todo-template.md` — Corrective-action TODO list generated from `TODO` comments
- `00-rules-template.md` — Scaffold for adding a new rule to `rules/`

## Usage

Reference rules in your prompts:

```
Consider the following rules when implementing this feature:
- prompts/rules/kotlin.md
- prompts/rules/hexagonal-architecture.md
```

Use a template as the scaffold for a feature spec or prompt:

```
Using prompts/templates/00-ralph_api_template.md, draft a spec for ...
```
