# MCP SDKs

To make building MCP Servers faster and more consistent, the Model Context Protocol offers official SDKs in multiple programming languages.

These SDKs help you:

- Define and register **capabilities**
- Handle request/response formatting
- Interact with the MCP client in a standardized way

---

## 📦 Available SDKs

| Language      | SDK Package                         | Repo / Docs                                      |
|---------------|-------------------------------------|--------------------------------------------------|
| **TypeScript**| `@mcp/sdk`                          | [GitHub](https://github.com/modelcontextprotocol/mcp-ts-sdk) |
| **Python**    | `mcp-sdk`                           | [GitHub](https://github.com/modelcontextprotocol/mcp-python-sdk) |
| **Java**      | `dev.mcp:mcp-sdk`                   | [GitHub](https://github.com/modelcontextprotocol/mcp-java-sdk) |
| **Kotlin**    | `dev.mcp:mcp-sdk`                   | Shared with Java SDK                            |
| **C# (.NET)** | `MCP.SDK`                           | [GitHub](https://github.com/modelcontextprotocol/mcp-dotnet-sdk) |

> 🔗 Explore all SDKs: [https://github.com/modelcontextprotocol](https://github.com/modelcontextprotocol)

---

## 🧑‍💻 Getting Started

### TypeScript SDK

#### Installation

```bash
npm install @mcp/sdk
```

---

#### Basic Example

```typescript
import { defineServer, capability } from '@mcp/sdk'

defineServer({
  capabilities: {
    getTime: capability({
      input: z.object({}),
      handler: async () => {
        return { time: new Date().toISOString() }
      }
    })
  }
}).listen()
```

### Python SDK

#### Installation

```python
pip install mcp-sdk
```

#### Basic Example

```python
from mcp import define_server, capability

@capability()
def get_time(input):
    return { "time": datetime.utcnow().isoformat() }

define_server(capabilities=[get_time]).listen()
```

## 💡 Choosing the Right SDK

| Your Stack          | Recommended SDK |
|---------------------|-------------------------|
| Full-stack JS/TS    | `@mcp/sdk`              |
| Backend in Python   | `mcp-sdk`               |
| JVM-based systems   | `dev.mcp:mcp-sdk`       |
| Microsoft stack     | `dev.mcp:mcp-sdk`       |

---

## 📁 Project Structure Tip

When using SDKs, structure your project like this

```markdown
mcp-server/
├── capabilities/
│   ├── getOrderStatus.py
│   └── fetchCustomerInfo.py
├── main.py (or index.ts)
└── requirements.txt / package.json
```

This keeps your capabilities modular and testable.

## 📚 More Resources

- [MCP Protocol Docs](https://modelcontextprotocol.io/introduction)
- [Anthropic’s Blog on MCP](https://www.anthropic.com/news/model-context-protocol)
- [MCP SDK Repositories](https://github.com/modelcontextprotocol)

---

> 🧠 The SDKs do the heavy lifting — you just define capabilities and focus on your business logic.
