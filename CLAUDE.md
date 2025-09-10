# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **prompt library** containing structured rules and templates for software development, specifically focused on:

- **Rules**: Development standards and architectural patterns (Kotlin, DDD, Hexagonal Architecture, Kotest testing)
- **Templates**: Reusable templates for prompts, todos, and feature documentation

The repository follows a document-centric approach where each rule and template is self-contained with clear guidance for AI assistants.

## Architecture & Structure

### Directory Structure
```
rules/                    # Development rules and standards
├── kotlin.md            # Functional Kotlin with Arrow FP patterns
├── domain-driven-design.md  # DDD tactical and strategic patterns
├── hexagonal-architecture.md  # Ports & adapters implementation
└── kotest.md           # Testing conventions with Kotest/Testcontainers

templates/              # Reusable prompt templates
├── 00-rules-template.md      # Template for creating new rules
├── 00-oneshot_template.md   # Template for feature specifications
└── 00-todo_template.md      # Template for TODO list generation
```

### Key Patterns

**Rules Structure**: All rules files follow the template pattern from `templates/00-rules-template.md`:
- Context section defining scope and audience
- Core principles (the "why" behind rules)
- Tiered rules: Must Have (critical) → Should Have (important) → Could Have (preferred)
- Patterns & Anti-patterns with concrete code examples
- Decision framework for edge cases
- Quality gates and related rules

**Template Structure**: Templates provide scaffolding for:
- Feature specifications with requirements and domain modeling
- TODO list generation with structured task breakdown
- Rules creation with consistent formatting

## Core Development Principles

Based on the rules in this repository:

1. **Functional-First Kotlin**: Never use nullable types (`?`), always use Arrow's `Option<A>` and `Either<E,A>`
2. **Hexagonal Architecture**: Domain logic isolated behind ports, adapters handle external integrations
3. **Domain-Driven Design**: Rich domain models with ubiquitous language and bounded contexts
4. **Outside-In Testing**: Acceptance → Integration → Unit test layers with Kotest and Testcontainers

## Common Development Tasks

Since this is a documentation repository, there are no build commands, tests, or deployment processes. The primary activities are:

- **Creating new rules**: Follow `templates/00-rules-template.md` structure
- **Creating feature specifications**: Use `templates/00-oneshot_template.md` 
- **Generating TODO lists**: Use `templates/00-todo_template.md` workflow
- **Content validation**: Ensure rules follow the established patterns and include all required sections

## Quality Standards

When working with this repository:

- **Consistency**: All rules must follow the template structure exactly
- **Completeness**: Include all sections (Context, Core Principles, Rules, Patterns, etc.)
- **Concrete Examples**: Provide both positive and anti-pattern code examples
- **Cross-References**: Link related rules in the "Related Rules" section
- **Actionable Guidance**: Rules should be specific enough to guide implementation decisions

## Template Usage

**For Rules**: Use the MoSCoW prioritization (Must/Should/Could Have) with numbered rules (RULE-001, RULE-101, RULE-201)

**For Features**: Include measurable requirements, domain modeling, testing considerations, and verification checklists

**For TODOs**: Structure tasks with checkboxes, clear prompts for AI execution, and affected files lists