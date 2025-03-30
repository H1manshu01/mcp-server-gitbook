# Integrating MCP Servers with Internal Systems

The true power of an MCP server lies in its ability to **bridge AI with your internal tools and data sources** — securely and contextually.

This guide walks you through the common patterns and best practices for integrating MCP capabilities with internal systems like databases, REST APIs, ERPs, CRMs, and more.

---

## 🧩 What Counts as an “Internal System”?

- Relational databases (MySQL, PostgreSQL, MSSQL)
- NoSQL databases (MongoDB, Firebase, DynamoDB)
- REST or GraphQL APIs
- File systems or shared storage
- SaaS tools (e.g., Salesforce, Zendesk, Jira)
- Legacy systems with custom interfaces

---

## 🔌 Integration Patterns

### 1. **Database Integration**

Most common when fetching user data, product catalogs, orders, etc.

#### Example (Python - MongoDB)

```python
@capability()
def get_user_by_id(input):
    user = db.users.find_one({ "id": input["userId"] })
    return { "name": user["name"], "email": user["email"] }
```

**Tips:**

- Use parameterized queries to prevent injection.
- Mask any sensitive fields before returning.
- Add indexes to improve performance for common lookups.

---

### 2. API Integration

Call internal or external APIs from within a capability.

Example (Node.js - REST API):

```ts
import axios from "axios"

const getWeather = capability({
  input: z.object({ city: z.string() }),
  handler: async ({ city }) => {
    const res = await axios.get(`https://internal-api/weather?city=${city}`)
    return { weather: res.data.summary }
  }
})
```

**Tips:**

- Use API keys or service tokens securely.
- Implement retries with exponential backoff for resilience.
- Handle and sanitize errors from upstream systems.

---

### 3. File System / Local Storage

Useful for reading logs, static documents, templates, etc.

Example:

```python
@capability()
def read_template(input):
    with open(f"templates/{input['filename']}", "r") as file:
        content = file.read()
    return { "content": content }
```

**Tips:**

- Whitelist valid filenames to prevent path traversal attacks.
- Avoid writing to disk unless absolutely necessary.

---

### 4. SaaS Tools or External Systems

Ideal for workflow automation (e.g., Slack messages, creating support tickets, triggering CI/CD jobs).

Example: Slack Notification (Node.js)

```ts
await axios.post("https://slack.com/api/chat.postMessage", {
  channel: "#ops-alerts",
  text: "New order placed!",
}, {
  headers: {
    Authorization: `Bearer ${process.env.SLACK_TOKEN}`
  }
})
```

---

## 🔐 Security Tips for Internal Integrations

- ✅ Use service accounts or scoped API tokens.
- ✅ Validate all inbound and outbound data.
- ✅ Never expose sensitive internal URLs to the AI model.
- ✅ Store all secrets in environment variables or a vault.

---

## 🧪 Testing Integrations

- Use Postman or curl to simulate MCP requests.
- Mock your external services in local/staging environments.
- Log all capability inputs/outputs (with redaction).

---

## ⚙️ Environment-Aware Design

Use environment variables to separate dev, staging, and production:

```bash
ENV=production
DB_URL=https://prod-db.example.com
INTERNAL_API_KEY=xyz123
```

---

## 📦 Modular Code Structure

Organize integrations by service or domain:

```bash
mcp-server/
├── capabilities/
│   ├── user/
│   │   └── get_user_by_id.py
│   ├── slack/
│   │   └── send_notification.py
├── utils/
│   └── api_client.py
│   └── db.py
```

This keeps capabilities clean and promotes reuse.

---

## 🧠 Final Thoughts

MCP capabilities are only as smart as the systems they connect to. By carefully integrating with your internal systems, you give your AI model superpowers — safely and intelligently.

---

> 💡 Think of each capability as an AI-friendly API layer that reflects your business logic in real-time.
