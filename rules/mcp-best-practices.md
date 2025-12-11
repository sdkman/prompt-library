# MCP Best Practices

Rules for designing, implementing, and maintaining Model Context Protocol (MCP) servers and clients. These guidelines ensure secure, maintainable, and interoperable MCP integrations that provide reliable context to AI assistants.

## Context

Provide the situational context where these rules apply. This helps the LLM understand when and why to apply these rules.

**Applies to:** MCP server development, MCP client integrations, tool design, resource management, and protocol communication
**Level:** Tactical/Operational
**Audience:** Developers building MCP servers, integrating MCP clients, and designing AI assistant tools

## Core Principles

1. **Security First:** MCP servers must validate all inputs, enforce proper authentication/authorization, and never expose sensitive data or allow arbitrary code execution without explicit safeguards.
2. **Clear Contracts:** Tools and resources should have explicit, well-documented schemas with clear descriptions of parameters, return types, and behavior. Ambiguity leads to misuse.
3. **Idempotency & Safety:** Read operations (resources, prompts) must be side-effect free. Write operations (tools) should be idempotent where possible and clearly indicate their mutating nature.
4. **Progressive Disclosure:** Start with simple, focused tools and resources. Avoid exposing complex APIs with dozens of optional parameters. Composition beats complexity.
5. **Graceful Degradation:** Servers should handle errors gracefully, return meaningful error messages, and fail safely. Never crash or hang on invalid input.

## Rules

### Must Have (Critical)

- **RULE-001:** All tool parameters MUST have clear JSON schemas with descriptions. Never accept untyped or loosely-typed inputs.
- **RULE-002:** MCP servers MUST validate all input parameters before executing operations. Never trust client input.
- **RULE-003:** Tools that perform destructive operations (delete, write, execute) MUST clearly indicate this in their description and name.
- **RULE-004:** MCP servers MUST NOT expose file system access without explicit path restrictions. Always validate and sanitize paths.
- **RULE-005:** All errors MUST return structured error responses with actionable messages, not generic failures or stack traces.
- **RULE-006:** MCP servers MUST NOT execute arbitrary code or shell commands provided by clients without explicit sandboxing.
- **RULE-007:** Resources MUST be read-only and side-effect free. Never perform mutations in resource handlers.
- **RULE-008:** Authentication credentials MUST be handled securely through environment variables or secure configuration, never hardcoded or passed as tool parameters.

### Should Have (Important)

- **RULE-101:** Tool names SHOULD follow verb-noun convention (e.g., `create_file`, `search_repositories`, `get_user_data`).
- **RULE-102:** MCP servers SHOULD implement rate limiting and resource quotas to prevent abuse and resource exhaustion.
- **RULE-103:** Tools SHOULD be atomic and focused on a single responsibility. Avoid "god tools" that do multiple unrelated things.
- **RULE-104:** Resource URIs SHOULD follow a consistent hierarchical naming scheme (e.g., `repo://owner/name/file/path`).
- **RULE-105:** MCP servers SHOULD provide meaningful progress updates for long-running operations rather than blocking indefinitely.
- **RULE-106:** Error messages SHOULD include context about what went wrong and how to fix it, not just error codes.
- **RULE-107:** Tools SHOULD declare required vs optional parameters explicitly in their schema. Use `required` array in JSON schema.
- **RULE-108:** MCP servers SHOULD log operations for debugging but MUST NOT log sensitive data (tokens, passwords, PII).

### Could Have (Preferred)

- **RULE-201:** Tool descriptions COULD include usage examples to clarify expected behavior and parameter formats.
- **RULE-202:** MCP servers COULD implement caching for expensive read operations with appropriate TTLs.
- **RULE-203:** Resources COULD support pagination for large datasets to avoid memory exhaustion and timeouts.
- **RULE-204:** Tools COULD return structured data rather than plain strings to enable better post-processing by clients.
- **RULE-205:** MCP servers COULD expose a health check or status tool for monitoring and diagnostics.
- **RULE-206:** Prompts COULD include version numbers or timestamps to track changes over time.

## Patterns & Anti-Patterns

### ✅ Do This

```typescript
// Good: Clear schema with validation and focused responsibility
server.tool({
  name: "create_issue",
  description: "Creates a new issue in a GitHub repository",
  inputSchema: {
    type: "object",
    properties: {
      owner: { type: "string", description: "Repository owner" },
      repo: { type: "string", description: "Repository name" },
      title: { type: "string", description: "Issue title" },
      body: { type: "string", description: "Issue body (optional)" }
    },
    required: ["owner", "repo", "title"]
  },
  handler: async ({ owner, repo, title, body }) => {
    // Validate inputs
    if (!owner.match(/^[a-zA-Z0-9-]+$/)) {
      throw new Error("Invalid owner format");
    }
    // Execute focused operation
    const issue = await github.createIssue(owner, repo, title, body);
    return { issueNumber: issue.number, url: issue.html_url };
  }
});
```

```typescript
// Good: Read-only resource with clear URI scheme
server.resource({
  uri: "file:///path/to/config.json",
  name: "Configuration file",
  mimeType: "application/json",
  handler: async () => {
    const content = await readFile("/path/to/config.json");
    return { content };
  }
});
```

### ❌ Don't Do This

```typescript
// Bad: Vague naming, no schema, accepts arbitrary commands
server.tool({
  name: "execute",
  description: "Executes something",
  inputSchema: { type: "object" }, // No properties defined!
  handler: async ({ command }) => {
    // DANGEROUS: Arbitrary code execution
    return execSync(command).toString();
  }
});
```

```typescript
// Bad: Resource with side effects
server.resource({
  uri: "action://delete-all-files",
  handler: async () => {
    // WRONG: Resources must be read-only
    await deleteAllFiles();
    return { content: "Files deleted" };
  }
});
```

```typescript
// Bad: God tool that does too many things
server.tool({
  name: "github_do_everything",
  description: "Creates repos, issues, PRs, manages users, etc.",
  inputSchema: {
    properties: {
      action: { type: "string" }, // Too generic
      params: { type: "object" }  // Unstructured
    }
  }
});
```

## Decision Framework

**When rules conflict:**
1. Security always takes precedence over convenience
2. Explicit validation beats implicit trust
3. Simple, focused tools beat complex multi-purpose tools

**When facing edge cases:**
- If a tool could be destructive, make it explicit in naming and description
- If input validation is complex, fail safely and return clear error messages
- If an operation is expensive, consider caching, pagination, or async patterns
- When unsure about exposing functionality, start restrictive and expand based on need

## Exceptions & Waivers

**Valid reasons for exceptions:**
- **Trusted environments:** Internal-only MCP servers in controlled environments may relax some validation rules (but must document this clearly)
- **Performance-critical paths:** Caching or batching may require more complex tool designs (must still maintain security)
- **Legacy integrations:** Adapting existing APIs may require some schema flexibility (must validate at boundaries)

**Process for exceptions:**
1. Document the exception and specific rationale in code comments
2. Add security review for any relaxed validation or authentication rules
3. Include monitoring/logging to detect misuse

## Quality Gates

- **Automated checks:**
  - Schema validation for all tools and resources
  - Security scanning for command injection, path traversal patterns
  - Type checking for TypeScript/Python MCP implementations
  - Unit tests covering error cases and edge cases

- **Code review focus:**
  - Input validation completeness
  - Error handling and graceful degradation
  - Clear naming and documentation
  - No hardcoded credentials or sensitive data

- **Testing requirements:**
  - Unit tests for each tool with valid and invalid inputs
  - Integration tests for MCP server initialization and tool execution
  - Security tests for injection attacks and unauthorized access
  - Load tests for resource-intensive operations

## Related Rules

- `rules/rust.md` - When implementing MCP servers in Rust
- `rules/domain-driven-design.md` - Tool design should reflect domain language

## References

- [MCP Specification](https://spec.modelcontextprotocol.io/) - Official protocol specification
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk) - Reference implementation
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) - Python implementation
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/) - Security guidelines for APIs

---

## TL;DR

**Key Principles:**
- Security first: validate all inputs, never execute arbitrary code, sanitize paths
- Clear contracts: explicit schemas with descriptions for every tool and resource
- Simple and focused: each tool does one thing well, resources are read-only

**Critical Rules:**
- Must validate all input parameters before execution
- Must use clear JSON schemas for all tools
- Must never expose arbitrary code execution or unrestricted file system access
- Must return structured errors with actionable messages

**Quick Decision Guide:**
When in doubt: Make it explicit, validate thoroughly, and fail safely. If a tool could be dangerous, it probably is—add safeguards first.
