# Four Rules of Simple Design

A design heuristic, originated by Kent Beck and refined by Martin Fowler, J.B. Rainsberger, and Ron Jeffries, for judging whether code is well-designed. Used to guide refactoring decisions and let good design emerge from incremental change rather than upfront speculation.

## Context

Apply these rules during refactoring, code review, and incremental design. They answer the question "is this design simple enough?" with four prioritized criteria — language and paradigm agnostic.

*Applies to:* Any production codebase practicing TDD, refactoring, or evolutionary design
*Level:* Tactical — applied at the function, class, and module level
*Audience:* Developers, technical leads, and reviewers making design decisions

## Core Principles

1. *Design Emerges:* Good design is the cumulative result of disciplined small decisions, not upfront speculation. Apply these rules continuously and let structure surface from the problem.
2. *Order Matters:* The rules are prioritized. When two pull in opposite directions, the higher-ranked rule wins.
3. *Empathy Over Metrics:* Code is read by people. When in doubt, optimize for the reader, not for a strict mechanical interpretation of the rules.
4. *Mastery Is Mechanical:* Per J.B. Rainsberger — master removing duplication and improving names, and you master modular design. Most other "design skills" reduce to these two practices.

## Rules

### Must Have (Critical)
Non-negotiable. Code that violates these is incomplete.

- *RULE-001:* **Passes all tests.** A design that does not work is not a design — it is a draft. The full suite must be green before any other rule is considered. If a refactor breaks tests, revert and try smaller steps.
- *RULE-002:* **Reveals intent.** Names, structure, and types must communicate the author's purpose to a future reader. If a reviewer needs a comment, a Slack message, or a meeting to understand why the code exists, the code fails this rule.
- *RULE-003:* **Contains no duplication.** Every piece of knowledge — logic, magic number, business rule, formula — lives in exactly one place. Surface duplication (copy-paste) and conceptual duplication (two implementations of the same idea) both count.
- *RULE-004:* **Has the fewest elements possible.** Remove any class, method, parameter, abstraction, or indirection that does not serve rules 001–003. Speculative flexibility ("we might need this later") is a violation.

### Should Have (Important)
Strong recommendations applied during refactoring and review.

- *RULE-101:* **Apply the rules in priority order.** When intent and DRY conflict, prefer the higher one. Beck and Fowler rank intent above DRY; Rainsberger inverts them, arguing structure emerges from removing duplication first. Pick a stance per codebase and stay consistent.
- *RULE-102:* **Evolve names through stages.** Rename progressively: nonsense → accurate-but-vague → precise → intention-revealing. Do not wait for the "perfect" name before extracting; extract first, rename as understanding grows.
- *RULE-103:* **Treat duplication as a design signal.** When you spot duplication, the lesson is rarely "extract a function" — it is "I have not yet found the concept these places share." Let the missing concept name itself.
- *RULE-104:* **Refactor in green.** Make changes while the suite is passing. Return to green after each small step. Never refactor on red.
- *RULE-105:* **Prefer deletion to abstraction.** When in doubt about whether to extract or remove, remove. It is cheaper to re-extract later than to dismantle a wrong abstraction.

### Could Have (Preferred)
Practices that reinforce the rules without being mandatory.

- *RULE-201:* **Pair "rule of three" with judgment.** Two duplications can be a coincidence; three is a pattern. But if the concept is obvious on the second instance, extract immediately.
- *RULE-202:* **Use code review to enforce intent.** Reviewers should ask "what does this say to me?" before "is this correct?" If reviewers consistently misread code, the code fails RULE-002 regardless of test status.
- *RULE-203:* **Track speculative additions in a backlog, not in code.** Capture future needs as tickets, not as unused parameters, abstract base classes, or "just in case" interfaces.

## Patterns & Anti-Patterns

### ✅ Do This

```kotlin
// Intent-revealing name, single responsibility, no speculative parameters
fun applyVatToInvoice(invoice: Invoice): Invoice =
    invoice.copy(total = invoice.subtotal * VAT_RATE)

// Conceptual duplication collapsed by naming the shared idea
fun isWeekend(day: DayOfWeek): Boolean =
    day == SATURDAY || day == SUNDAY
```

### ❌ Don't Do This

```kotlin
// Vague name, speculative flag, hidden intent
fun process(invoice: Invoice, mode: Int = 0): Invoice { ... }

// Same rule expressed in three places — change one and the others rot
fun isOpenOnSaturday() = day == SATURDAY
fun isOpenOnSunday() = day == SUNDAY
// elsewhere: if (day == SATURDAY || day == SUNDAY) ...
```

## Decision Framework

*When rules conflict:*
1. Does the change keep the tests green? If not, stop — RULE-001 wins.
2. Would a new reader understand the code without asking? If not, RULE-002 wins over DRY.
3. If intent is preserved, eliminate duplication next.
4. Then, and only then, remove anything that does not serve the above.

*When facing edge cases:*
- Two pieces of code look duplicated but respond to different reasons to change → keep them separate (Single Responsibility trumps DRY).
- An abstraction "feels right" but has a single caller → delete it. Re-introduce when a second caller appears.
- A name is hard to find → the concept may be wrong, not the name. Reconsider the boundaries.

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Performance-critical hot path where an abstraction has measured cost — document the benchmark.
- Generated code or framework-mandated structure — out of the design's hands.
- Public API stability — preserving an awkward shape to avoid breaking consumers, with a deprecation plan.

*Process for exceptions:*
1. Document the exception in code with a short comment explaining the constraint.
2. Link to the benchmark, ADR, or external constraint that justifies it.
3. Schedule a review when the constraint is lifted.

## Quality Gates

- *Automated checks:* Test suite must be green; static analyzers can flag long methods, large classes, and high cyclomatic complexity as duplication/intent proxies.
- *Code review focus:* Reviewers should ask "what does this code say?" and "where else does this idea live?" before approving.
- *Testing requirements:* Behavior must be covered by tests before refactoring; refactors must not change the tests.

## Related Rules

- rules/domain-driven-design.md — Ubiquitous language is a concrete way to satisfy "reveals intent."
- rules/hexagonal-architecture.md — Ports and adapters reduce structural duplication and clarify boundaries.
- rules/kotest.md — Test-first practice operationalizes RULE-001.
- rules/kotlin.md — Functional Kotlin style supports intention-revealing, fewest-elements code.

## References

- [Beck Design Rules — Martin Fowler](https://martinfowler.com/bliki/BeckDesignRules.html) — Fowler's summary and commentary on the original rules.
- [The Four Elements of Simple Design — J.B. Rainsberger](https://blog.jbrains.ca/permalink/the-four-elements-of-simple-design) — Reordering with emphasis on duplication-before-clarity.
- [Emergent Design — Ron Jeffries](https://ronjeffries.com/xprog/classics/expemergentdesign/) — On letting design emerge from local rule application.

---

## TL;DR

*Key Principles:*
- Good design emerges; it is not specified upfront.
- The four rules are ordered — apply them in priority.
- Master removing duplication and improving names; the rest follows.

*Critical Rules:*
- Must pass all tests before any other rule applies.
- Must reveal the author's intent to a reader.
- Must contain no duplicated knowledge.
- Must remove every element that serves none of the above.

*Quick Decision Guide:*
When in doubt: remove duplication, then improve the names. If neither applies, delete something.
