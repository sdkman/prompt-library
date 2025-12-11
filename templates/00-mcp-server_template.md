# [MCP Server Name]

*Write a brief paragraph describing the high-level purpose and context of this MCP server. What problem does it solve? What capabilities does it provide to LLM applications? What external systems or data sources does it integrate with? Keep this concise and focused on the "why" rather than the "how".*

## Requirements

*List the specific, measurable acceptance criteria that define when this MCP server feature/tool is complete. These should be testable and unambiguous. Think of these as your definition of done.*

- Requirement 1
- Requirement 2
- Requirement 3

## Rules

*Specify any rules files that should be included when working on this feature. This includes any rules files that might be relevant for this slice to be implemented. The rules files are usually found under the `rules/` directory of this project.*

- rules/my-rules-1.md
- rules/my-rules-2.md
- rules/my-rules-3.md

## MCP Primitives

*Define which MCP primitives this feature implements and their high-level purpose. Remove sections that don't apply to your feature.*

### Tools

*List the tools this feature provides. Tools are functions that the LLM can call to perform actions or retrieve dynamic data.*

- **tool_name_1**: Brief description of what this tool does
- **tool_name_2**: Brief description of what this tool does

### Resources

*List the resources this feature exposes. Resources are data sources that the LLM can read, typically with URI-based access patterns.*

- **resource_uri_pattern**: Brief description of what data this resource provides
- **resource_uri_pattern**: Brief description of what data this resource provides

### Prompts

*List any prompt templates this feature provides. Prompts are reusable prompt templates that can be invoked with arguments.*

- **prompt_name**: Brief description of what this prompt template does

## Server Metadata

*Define the server's metadata and capabilities.*

```json
{
  "name": "server-name",
  "version": "0.1.0",
  "description": "Brief server description",
  "capabilities": {
    "tools": {},
    "resources": {},
    "prompts": {}
  }
}
```

## Tool Schemas

*For each tool, provide the complete JSON Schema for inputs and expected outputs. This defines the contract between the LLM and your server.*

### tool_name

**Input Schema:**
```json
{
  "type": "object",
  "properties": {
    "param1": {
      "type": "string",
      "description": "Description of parameter"
    }
  },
  "required": ["param1"]
}
```

**Output Schema:**
```json
{
  "type": "object",
  "properties": {
    "result": {
      "type": "string",
      "description": "Description of result"
    }
  }
}
```

## Resource Schemas

*For each resource, define the URI pattern, MIME types, and structure of returned data.*

### resource_name

**URI Pattern:** `resource://namespace/path/{id}`

**MIME Types:** `application/json`, `text/plain`

**Response Structure:**
```json
{
  "uri": "resource://namespace/path/123",
  "mimeType": "application/json",
  "text": "string content or JSON stringified data"
}
```

## Error Handling

*Define the error handling strategy for this feature. MCP uses standardized JSON-RPC error codes.*

### Error Codes

- **-32600** (Invalid Request): When request is malformed
- **-32601** (Method Not Found): When tool/resource doesn't exist
- **-32602** (Invalid Params): When parameters don't match schema
- **-32603** (Internal Error): When server encounters unexpected error
- **Custom errors**: Define any domain-specific error codes

### Error Response Format

```json
{
  "code": -32602,
  "message": "Invalid parameters",
  "data": {
    "details": "Additional context about the error"
  }
}
```

## Domain

*If applicable, describe the core domain model using pseudo-code in your implementation language (Rust, TypeScript, Python, etc.). Focus on the key entities, types, and business logic that power the MCP tools/resources.*

```rust
// Core domain representation
struct Entity {
    field1: String,
    field2: Option<Value>,
}

impl Entity {
    fn business_logic(&self) -> Result<Output, Error> {
        // Domain logic here
    }
}
```

## Transport Considerations

*Document any transport-specific requirements or considerations.*

- **stdio**: Default transport, best for local CLI tools
- **SSE (Server-Sent Events)**: For web-based integrations
- **Custom**: Any specialized transport needs

## Security Considerations

*Document security boundaries and authentication/authorization requirements.*

- API key handling (if applicable)
- Rate limiting requirements
- Input validation and sanitization
- Sensitive data handling
- Privilege requirements (file system access, network access, etc.)

## Extra Considerations

*List important factors that need special attention during implementation. This often grows as you discover edge cases or constraints during development.*

- Consideration 1
- Consideration 2
- Consideration 3

## Testing Considerations

*Describe how you want this feature to be tested. What types of tests are needed? What scenarios should be covered?*

### Unit Tests
- Test tool/resource handlers in isolation
- Test input validation and error cases
- Test domain logic

### Integration Tests
- Test full MCP request/response cycle
- Test with mock transport layer
- Test error propagation

### End-to-End Tests
- Test with actual MCP client (if applicable)
- Test against real external dependencies (with test doubles)
- Test common LLM interaction patterns

## Implementation Notes

*Document your preferences for how this should be built. This might include language-specific patterns, dependency choices, or architectural decisions.*

**For Rust:**
- Use `mcp-server-sdk` or similar MCP library
- Error handling with `Result<T, Error>` and thiserror
- Async runtime (tokio/async-std)
- Serialization with serde

**For TypeScript:**
- Use `@modelcontextprotocol/sdk`
- Type safety with Zod schemas
- Async/await patterns

**For Python:**
- Use `mcp` SDK
- Type hints with Pydantic models
- Async with asyncio

## Specification by Example

*Provide concrete examples of MCP request/response flows. Show the actual JSON-RPC messages that would be exchanged.*

### Example: Calling a Tool

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "tool_name",
    "arguments": {
      "param1": "value1"
    }
  }
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "content": [
      {
        "type": "text",
        "text": "Tool execution result"
      }
    ]
  }
}
```

### Example: Reading a Resource

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "resources/read",
  "params": {
    "uri": "resource://namespace/path/123"
  }
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": {
    "contents": [
      {
        "uri": "resource://namespace/path/123",
        "mimeType": "application/json",
        "text": "{\"data\": \"value\"}"
      }
    ]
  }
}
```

### Example: Error Response

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": 3,
  "error": {
    "code": -32602,
    "message": "Invalid parameters",
    "data": {
      "field": "param1",
      "reason": "Must be a non-empty string"
    }
  }
}
```

## Usage Example

*Show how an LLM application would interact with this MCP server in practice. Include initialization, typical interaction flows, and common patterns.*

```typescript
// Example client usage
const client = new MCPClient({
  serverName: "my-mcp-server",
  transport: "stdio"
});

await client.connect();

// List available tools
const tools = await client.listTools();

// Call a tool
const result = await client.callTool({
  name: "tool_name",
  arguments: { param1: "value" }
});

// Read a resource
const resource = await client.readResource({
  uri: "resource://namespace/path/123"
});
```

## Verification

*Create a checklist to verify that the feature is complete and working correctly. These should be actionable items that can be checked off systematically.*

- [ ] Server exposes correct metadata and capabilities
- [ ] All tools implement required JSON schemas
- [ ] All resources follow URI pattern conventions
- [ ] Error handling returns proper JSON-RPC error codes
- [ ] Input validation rejects invalid requests
- [ ] Integration tests pass with MCP client
- [ ] Documentation includes request/response examples
- [ ] Security considerations are addressed
- [ ] Server handles graceful shutdown
- [ ] Logging provides adequate debugging information
