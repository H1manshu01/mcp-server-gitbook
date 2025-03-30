# Testing & Debugging MCP Servers

Once your MCP server is up and running, it's important to test each capability thoroughly, ensure correct inputs/outputs, and identify any issues early. This guide covers the tools and methods you can use to confidently test and debug your MCP integrations.

---

## 🧪 Why Testing Matters

- ✅ Verify that capabilities work as expected
- ✅ Prevent miscommunication between AI models and backends
- ✅ Improve reliability and trust in AI outputs
- ✅ Catch bugs, security issues, and logic errors early

---

## 🛠️ Tools for Testing MCP Servers

### 1. **Postman** or **cURL**

Manually test each capability endpoint by simulating client requests.

#### Example (using `curl`):

```bash
curl -X POST http://localhost:5003/capabilities/getTime \
  -H "Content-Type: application/json" \
  -d '{}'
 ```

Example (Postman setup):

- Method: POST
- URL: [http://localhost:5003/capabilities/yourCapabilityName](http://localhost:5003/capabilities/yourCapabilityName)
- Body: Raw JSON with required inputs

---

### 2. Local MCP Client

Use a local MCP Client (e.g., the one Claude or your LLM is configured to talk to) to simulate full request cycles.

This lets you see:

- How the model initiates the request
- What gets routed to the MCP server
- The response flow

---

### 3. Unit Testing (Python/Node)

Write unit tests for each capability’s logic — independent of the server runtime.

TypeScript Example (Jest):

```ts
test('getTime returns valid timestamp', async () => {
  const result = await getTime.handler({})
  expect(result.time).toMatch(/\d{4}-\d{2}-\d{2}T/)
})
```

Python Example (pytest):

```python
def test_get_time():
    result = get_time({})
    assert "time" in result
```

---

### 4. Logging (for Debugging)

Use console.log() or print() with timestamps, inputs, outputs, and durations.

```ts
console.log(`[getUserById] called with userId=${userId}, returned in ${duration}ms`)
```

```python
print(f"[get_order_status] input: {input}, output: {response}")
```

Add log levels (INFO, DEBUG, ERROR) if needed for filtering.

---

### 5. Error Simulation

Intentionally send bad data or omit required fields to test:

- Input validation
- Error messages
- Security response (e.g., auth failures, rate limits)

This helps you harden your server before going live.

---

### 6. Live Model Testing

Once the server is connected to your AI model (e.g., Claude via MCP Client), run end-to-end tests using real prompts like:

“What’s the status of order #123?”

Then verify:

- Which capability the model triggers
- What input it sends
- What response is returned

Use logging or tracing tools to monitor the full flow.

---

### 🔄 Tips for Effective Debugging

- ✅ Log both request and response bodies
- ✅ Trace request latency per capability
- ✅ Use correlation IDs for multi-step debugging
- ✅ Always validate input before executing logic

---

### 🧰 Optional Dev Tools

| Tool | Purpose |
| --- | --- |
| Postman | Manual API testing |
| Insomnia | Lightweight Postman alternative |
| VS Code Debugger | Step-through capability logic |
| Jest / Pytest | Unit testing |
| Docker | Run test environments in isolation |
| ngrok | Expose your local MCP server for remote testing |

---

### 🧠 Final Thoughts

Testing and debugging is not a one-time task — it’s ongoing. Your MCP server is a live part of your AI infrastructure, and it deserves the same rigor as your APIs.

> 💡 Treat every capability like a microservice endpoint: test it, log it, and secure it.

## ✅ Next Step

> → Once you’re confident everything works, move on to [Logging & Monitoring](../advanced-topics/logging-monitoring.md) for observability in production.
