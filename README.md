# Prompt Library

A curated collection of development rules and prompt templates for AI-assisted software development. Use these structured prompts to maintain consistency and quality when working with AI coding assistants.

## Quick Start

Add this library to your project as a git submodule:

```bash
# Add the submodule to your project
git submodule add https://github.com/yourusername/prompt-library.git prompts

# Initialize and update the submodule
git submodule update --init --recursive

# In the future, update to latest rules
git submodule update --remote prompts
```

## What's Included

### Rules (`rules/`)
Development standards and architectural patterns that AI assistants should follow:

- **kotlin.md** - Functional Kotlin with Arrow FP patterns
- **domain-driven-design.md** - DDD tactical and strategic patterns  
- **hexagonal-architecture.md** - Ports & adapters implementation
- **kotest.md** - Testing conventions with Kotest/Testcontainers

Rules provide structured guidance with:
- Core principles and context
- Tiered requirements (Must/Should/Could Have)
- Code examples and anti-patterns
- Decision frameworks for edge cases

### Templates (`templates/`)
Meta-prompting templates for common AI development tasks:

- **00-rules-template.md** - Create new development rules
- **00-oneshot_template.md** - Specify features for single-shot implementation
- **00-todo_template.md** - Generate structured TODO lists from code comments

Templates help you craft effective prompts by providing proven structures and ensuring you include all necessary context for AI assistants.

## Usage

Reference rules in your prompts:
```
Consider the following rules when implementing this feature:
- prompts/rules/kotlin.md
- prompts/rules/hexagonal-architecture.md
```

Use templates to structure complex requests:
```
Using the template from prompts/templates/00-oneshot_template.md, 
implement a user authentication system with the following requirements...
```

## Benefits

- **Consistency** - Standardized patterns across projects and team members
- **Quality** - Battle-tested rules based on industry best practices  
- **Efficiency** - Reusable templates reduce prompt engineering overhead
- **Maintainability** - Centralized rules that evolve with your practices