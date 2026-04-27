# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

A documentation-only library of **rules** and **templates** consumed by AI coding assistants. There is no build, no test suite, and nothing to run — every artefact is self-contained markdown intended to be referenced from other projects (typically as a git submodule named `prompts/`).

## Layout

- `rules/` — opinionated standards an assistant should follow when writing code (Kotlin, Rust, DDD, Hexagonal Architecture, Simple Design, Kotest, MCP best practices). The library is polyglot; rules apply where their `Applies to:` context says they apply.
- `templates/` — prompt scaffolds for common AI development tasks (feature specs, implementation plans, backend/frontend slice prompts, MCP server prompts, ralph API specs, TODO lists, and a meta-template for authoring new rules). All templates are prefixed `00-`.

## Authoring Conventions

### Rules must follow `templates/00-rules-template.md` exactly

This is the most important convention in the repo. When adding or editing a file in `rules/`:

- Keep the file under ~200 lines to preserve context-window budget for consumers.
- Use the prescribed sections: Context (`Applies to` / `Level` / `Audience`), Core Principles, Rules, Patterns & Anti-Patterns, Decision Framework, Exceptions & Waivers, Quality Gates, Related Rules, References, TL;DR. Omit a section only when it genuinely doesn't apply (e.g. Patterns & Anti-Patterns for a non-code rule).
- Number rules `RULE-XXX` with the MoSCoW range: `001–099` Must Have, `101–199` Should Have, `201–299` Could Have.
- Cross-link sibling rules in the **Related Rules** section.
- The TL;DR must be readable in under 30 seconds.

### Templates contain author-guidance in italics

Files in `templates/` are scaffolds, not finished documents. Italicised paragraphs (`*...*`) are instructions to the author and **must be replaced** when the template is used for a real task — they are not content to preserve. Headings and structural lists are the load-bearing parts.

### Naming

Rules use plain hyphenated names (`mcp-best-practices.md`). Templates are prefixed `00-` and otherwise mix underscores and hyphens — match the style of the closest existing template rather than imposing a new convention.

## When the Parent CLAUDE.md Applies

The parent `/home/marco/src/sdkman/CLAUDE.md` covers SDKMAN-wide concerns (NixOS, SDKMAN JDK selection, Docker for local DBs, MCP tooling preferences, secrets policy). Those rules apply to **other** SDKMAN projects but are largely irrelevant here — this repo has no JVM, no Docker, no databases, and no SBT. The git/filesystem/GitHub MCP preferences from the parent still apply when operating on this repo.
