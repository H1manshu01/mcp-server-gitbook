# Setting Up an MCP Server

Let’s get your MCP Server up and running! This guide walks you through setting up a minimal, working MCP server using either **Python** or **TypeScript**, using official SDKs.

---

## ⚙️ Step 1: Choose Your Stack

You can build your MCP server using:

- ✅ **Python** (`mcp-sdk`)
- ✅ **TypeScript/Node.js** (`@mcp/sdk`)

Pick the language you're most comfortable with — both SDKs follow a similar capability-based structure.

---

## 🐍 Python Setup

### 🔧 1. Create a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 📦 2. Install MCP SDK

```bash
pip install mcp-sdk
```

### 🛠️ 3. Create Your Server File

Create a file named main.py

```python
from mcp import define_server, capability
from datetime import datetime

@capability()
def get_time(input):
    return { "time": datetime.utcnow().isoformat() }

define_server(capabilities=[get_time]).listen()
```

### 🚀 4. Run the Server

```bash
python main.py
```

You should see something like:

```bash
✅ MCP Server is listening on http://localhost:5003
```

## 🟦 TypeScript/Node.js Setup

### 🔧 1. Initialize a Project

```bash
mkdir mcp-ts-server && cd mcp-ts-server
npm init -y
```

### 📦 2. Install MCP SDK

```bash
npm install @mcp/sdk zod
```

### 🛠️ 3. Create Your Server File

Create a file named index.ts:

```typescript
import { defineServer, capability } from '@mcp/sdk'
import { z } from 'zod'

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

> Note: You’ll need ts-node or a TypeScript compiler to run this.

### 🚀 4. Run the Server

```bash
npx ts-node index.ts
```

Expected output:

```bash
✅ MCP Server listening at http://localhost:5003
```

---

## 🔁 Testing Your Server

Use curl, Postman, or connect it to a local MCP Client.
Example curl test (replace the payload with your capability):

```bash
curl -X POST http://localhost:5003/capabilities/getTime \
  -H "Content-Type: application/json" \
  -d '{}'
```

## 🧪 Recommended Next Steps

- Define more capabilities
- Secure your server
- Log and monitor usage

---

## 🧠 Pro Tip

You can run multiple capabilities from the same server — just add them to the capabilities array or object when defining your server.

---

> ✅ That’s it! You’ve got an MCP Server up and running — ready to power your AI agents with real-time data and actions.

