# Creating Custom Capabilities

In the MCP ecosystem, **capabilities** are the core building blocks that allow your AI assistant to access data or trigger actions. A custom capability is a specific function or service that you define, exposing your business logic to the AI in a controlled, secure, and structured way.

---

## 🧩 What is a Capability?

A capability is

- A named action (e.g., `getUserById`, `getWeather`, `createInvoice`)
- Executed by the MCP Server
- Invoked by the MCP Client on behalf of an AI model
- Defined with strict **input and output schemas**

---

## 🛠️ Anatomy of a Capability

### 🟦 TypeScript Example

```ts
import { defineServer, capability } from '@mcp/sdk'
import { z } from 'zod'

const getTime = capability({
  input: z.object({}),
  output: z.object({ time: z.string() }),
  handler: async () => {
    return { time: new Date().toISOString() }
  }
})

defineServer({
  capabilities: {
    getTime
  }
}).listen()
```

### 🐍 Python Example

```python
from mcp import define_server, capability
from datetime import datetime

@capability()
def get_time(input):
    return { "time": datetime.utcnow().isoformat() }

define_server(capabilities=[get_time]).listen()
```

---

## 📦 Best Practices for Custom Capabilities

| Practice | Why It Matters |
| --- | --- |
| Use strict input validation | Prevent bad data or malformed model requests |
| Limit access to safe data | Avoid exposing sensitive or unrestricted APIs |
| Keep handlers stateless | Easier to debug, test, and scale |
| Log input/output securely | For auditing and debugging |
| Modularize by domain | Easier to manage capabilities as your app grows |

---

## 🧠 Naming Your Capabilities

Use action-based, descriptive names:

- ✅ getOrderStatus
- ✅ createSupportTicket
- ✅ fetchUserProfile
- ❌ doSomething
- ❌ handlerOne

---

## 🧪 Capability Example: Get User by ID

### 🟦 TypeScript

```ts
const getUserById = capability({
  input: z.object({
    userId: z.string()
  }),
  output: z.object({
    name: z.string(),
    email: z.string()
  }),
  handler: async ({ userId }) => {
    const user = await db.users.findById(userId)
    return { name: user.name, email: user.email }
  }
})
```

### 🐍 Python

```python
@capability()
def get_user_by_id(input):
    user = db.find_user_by_id(input['userId'])
    return { "name": user.name, "email": user.email }
```

---

## 📁 Recommended Project Structure

Organize your capabilities by domain or use case:

```md
mcp-server/
├── capabilities/
│   ├── user/
│   │   ├── get_user_by_id.py
│   │   └── update_user_profile.py
│   ├── orders/
│   │   ├── get_order_status.py
│   │   └── create_order.py
├── main.py (or index.ts)
└── requirements.txt / package.json
```

---

## 🔐 Securing Capabilities

- Authenticate requests before executing sensitive capabilities
- Log usage with timestamps and user context
- Rate-limit or scope access via middleware or per-capability logic

---

## 🚀 Deployment Tips

- Bundle capabilities as individual modules for reuse
- Document expected input/output for frontend or LLM developers
- Use environment variables to connect to live vs. test systems

---

## 🧠 Final Thoughts

Custom capabilities are what make your AI intelligent — not just in conversation, but in action. They let you embed your app’s logic directly into the LLM-powered flow without compromising safety or structure.

---

> 💡 Think of capabilities as secure, LLM-safe APIs with strict contracts and business context built in.
