# Rust Best Practices 2025

Modern Rust development practices emphasizing safety, performance, and idiomatic code patterns. These rules reflect the mature Rust ecosystem of 2025, including async/await patterns, error handling, and modern tooling.

## Context

Provide guidance for writing idiomatic, safe, and performant Rust code in 2025, covering language features, ecosystem best practices, and architectural patterns.

*Applies to:* All Rust projects including CLI tools, web services, systems programming, and embedded systems
*Level:* Tactical/Operational - day-to-day coding and architecture decisions
*Audience:* Rust developers at all levels, from beginners learning idiomatic patterns to experienced developers maintaining consistency

## Core Principles

1. *Zero-Cost Abstractions:* Leverage Rust's type system and ownership model to achieve safety without runtime overhead. Prefer compile-time guarantees over runtime checks.
2. *Explicit Error Handling:* Make errors visible in type signatures using `Result<T, E>` and `Option<T>`. Never use `unwrap()` or `expect()` in production code paths without strong justification.
3. *Ownership-First Design:* Design APIs around ownership and borrowing rather than fighting the borrow checker. Use lifetimes explicitly when necessary but prefer owned types when feasible.
4. *Composition Over Inheritance:* Use traits, generics, and composition to build abstractions. Leverage trait objects (`dyn Trait`) and enum-based patterns over complex inheritance hierarchies.
5. *Async-First for I/O:* Default to async/await for I/O-bound operations. Use Tokio or async-std as runtime foundations, and design APIs to be runtime-agnostic when building libraries.

## Rules

### Must Have (Critical)

- *RULE-001:* Never use `.unwrap()` or `.expect()` in production code except during prototyping or in test code. Use proper error propagation with `?` operator or explicit pattern matching.
- *RULE-002:* Always implement `Error` trait for custom error types. Use `thiserror` crate for error type boilerplate or `anyhow` for application-level error handling.
- *RULE-003:* Make all public API functions `const` when possible to enable compile-time evaluation. Use `const fn` for constructors and pure functions.
- *RULE-004:* Never use `unsafe` without comprehensive documentation explaining safety invariants and why it's necessary. Every `unsafe` block must have a `SAFETY:` comment.
- *RULE-005:* Always run `cargo clippy` with `#![warn(clippy::all, clippy::pedantic)]` and address warnings. Configure project-specific allow lists in `Cargo.toml` only with justification.
- *RULE-006:* Use `#[must_use]` attribute on functions that return `Result`, `Option`, or other values that should not be ignored.

### Should Have (Important)

- *RULE-101:* Prefer `&str` over `&String` and `&[T]` over `&Vec<T>` in function parameters. Use the most general borrowed type that works.
- *RULE-102:* Use `impl Trait` for return types when returning iterators, futures, or other complex types. Only use boxed trait objects when necessary for type erasure.
- *RULE-103:* Implement `From` and `TryFrom` traits for type conversions instead of custom conversion methods. Use `Into` in generic bounds but implement `From`.
- *RULE-104:* Use `#[derive(Debug)]` on all types unless there's a specific reason not to. Implement `Display` for user-facing output.
- *RULE-105:* Prefer `&mut self` methods over consuming `self` unless the operation genuinely consumes the value. Use builder patterns for complex initialization.
- *RULE-106:* Use structured logging with `tracing` crate instead of `log` for new projects. Include span contexts for async operations.
- *RULE-107:* Pin dependencies with `cargo update` and commit `Cargo.lock` for applications. Do not commit lock files for libraries.
- *RULE-108:* Use workspace dependencies in `Cargo.toml` for monorepos to ensure version consistency across crates.

### Could Have (Preferred)

- *RULE-201:* Use `todo!()`, `unimplemented!()`, and `unreachable!()` macros appropriately. `todo!()` for planned code, `unreachable!()` for logically impossible branches with an explanation.
- *RULE-202:* Prefer `if let` and `let else` for single-pattern matches. Use full `match` when handling multiple patterns or when exhaustiveness checking is valuable.
- *RULE-203:* Use `Cow<str>` or `Cow<[T]>` when you might need to return either borrowed or owned data based on conditions.
- *RULE-204:* Leverage `#[non_exhaustive]` on enums and structs in public APIs to allow future additions without breaking changes.
- *RULE-205:* Use `cargo fmt` with default settings and commit `.rustfmt.toml` only for project-specific overrides. Run formatting in CI.
- *RULE-206:* Prefer `cargo nextest` over built-in `cargo test` for faster test execution and better output formatting.

## Patterns & Anti-Patterns

### ✅ Do This

```rust
// Explicit error handling with type-safe propagation
use thiserror::Error;

#[derive(Error, Debug)]
pub enum ConfigError {
    #[error("Failed to read config file: {0}")]
    Io(#[from] std::io::Error),
    #[error("Invalid TOML format: {0}")]
    Parse(#[from] toml::de::Error),
}

pub fn load_config(path: &str) -> Result<Config, ConfigError> {
    let content = std::fs::read_to_string(path)?;
    let config = toml::from_str(&content)?;
    Ok(config)
}

// Proper use of borrowing in function signatures
pub fn process_data(data: &[u8]) -> Vec<u8> {
    data.iter().map(|b| b.wrapping_add(1)).collect()
}

// Builder pattern for complex initialization
pub struct ClientBuilder {
    timeout: Duration,
    retries: u32,
}

impl ClientBuilder {
    pub fn timeout(mut self, timeout: Duration) -> Self {
        self.timeout = timeout;
        self
    }

    pub fn build(self) -> Client {
        Client { timeout: self.timeout, retries: self.retries }
    }
}
```

### ❌ Don't Do This

```rust
// Unwrap in production code - crashes on error
pub fn load_config(path: &str) -> Config {
    let content = std::fs::read_to_string(path).unwrap();
    toml::from_str(&content).unwrap()
}

// Taking owned String when borrowed would work
pub fn process_name(name: String) -> String {
    name.to_uppercase()
}

// Should be: pub fn process_name(name: &str) -> String

// Ignoring Result without #[must_use]
pub fn save_data(data: &Data) -> Result<(), Error> {
    // ... save operation
}
// Caller might accidentally do: save_data(&data);

// Unsafe without safety documentation
pub fn cast_bytes(data: &[u8]) -> &[u32] {
    unsafe { std::slice::from_raw_parts(data.as_ptr() as *const u32, data.len() / 4) }
}
```

## Decision Framework

*When rules conflict:*
1. Safety and correctness take precedence over performance optimizations
2. Consult Clippy lints and community RFC decisions for idiomatic patterns
3. Measure performance with `criterion` before making code less idiomatic for performance

*When facing edge cases:*
- Consult the Rust API guidelines: https://rust-lang.github.io/api-guidelines/
- Check standard library implementations for similar patterns
- Prefer explicitness over cleverness - code should be obvious to reviewers

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Performance-critical code where benchmarks prove unsafe necessary (document with benchmarks and SAFETY comments)
- FFI boundaries where C interop requires specific patterns (isolate to thin wrapper modules)
- Embedded systems with specific constraints (document hardware requirements)

*Process for exceptions:*
1. Document the exception with code comments explaining rationale
2. Add a `#[allow(clippy::...)]` attribute with comment for Clippy warnings
3. Include benchmarks or evidence for performance-related exceptions
4. Review in PR with at least one experienced Rust developer

## Quality Gates

- *Automated checks:*
  - `cargo clippy --all-targets -- -D warnings` must pass
  - `cargo fmt --check` must pass
  - `cargo test --all-features` must pass
  - `cargo audit` for dependency security vulnerabilities
  - Consider `cargo-deny` for license and supply chain verification

- *Code review focus:*
  - All `unsafe` blocks have clear SAFETY comments
  - Public APIs use appropriate borrowing (`&str` vs `String`)
  - Error types implement `Error` trait and use proper error propagation
  - No `.unwrap()` or `.expect()` in production paths

- *Testing requirements:*
  - Property-based testing with `proptest` for complex algorithms
  - Integration tests in `tests/` directory, unit tests in source files
  - Consider `cargo-mutants` for mutation testing coverage

## Related Rules

- rules/hexagonal-architecture.md - Applies to Rust web services using ports/adapters pattern
- rules/domain-driven-design.md - Type-driven domain modeling with Rust's type system

## References

- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/) - Official API design patterns
- [The Rust Performance Book](https://nnethercote.github.io/perf-book/) - Optimization techniques
- [Async Rust Book](https://rust-lang.github.io/async-book/) - Async/await patterns and runtimes
- [Effective Rust](https://www.lurklurk.org/effective-rust/) - Best practices and common pitfalls
- [Rust for Rustaceans](https://nostarch.com/rust-rustaceans) - Advanced patterns and internals

---

## TL;DR

*Key Principles:*
- Leverage ownership and type system for zero-cost safety guarantees
- Make errors explicit with Result/Option, never unwrap in production
- Design APIs around borrowing - prefer `&str` over `&String`, `&[T]` over `&Vec<T>`

*Critical Rules:*
- Must handle errors with `?` operator or pattern matching, never `.unwrap()`
- Must implement `Error` trait for custom errors using `thiserror` or `anyhow`
- Must document all `unsafe` blocks with SAFETY comments explaining invariants
- Must use `#[must_use]` on functions returning Result or Option
- Must run clippy with pedantic warnings and address issues

*Quick Decision Guide:*
When in doubt: Choose the more explicit, type-safe option that makes errors visible and impossible states unrepresentable. Run `cargo clippy` and follow its suggestions.
