# Logging & Monitoring MCP Servers

Effective logging and monitoring are critical for maintaining reliability, visibility, and security in your MCP server. Since these servers power real-time AI workflows, it's essential to trace what’s happening behind the scenes.

---

## 📋 Why Logging Matters

- 🐛 **Debugging**: Identify broken or slow capabilities
- 🧠 **Understanding behavior**: See how the model is interacting with your system
- 🔐 **Auditing**: Track what data was accessed and when
- 📊 **Performance tuning**: Detect latency or high load in specific endpoints

---

## 📝 What to Log

Here’s what you should consider logging:

| Item                         | Purpose                      |
|------------------------------|------------------------------|
| Capability name              | Which action was called      |
| Input arguments              | What was passed in           |
| Execution timestamp          | When the request occurred    |
| Response or error output     | What was returned or failed  |
| Execution duration           | How long it took             |
| Request origin (optional)    | User, model, or system source|

> 🔒 **Note:** Never log sensitive data unless required and securely masked.

---

## 🟦 Logging Example (TypeScript)

```ts
import { defineServer, capability } from '@mcp/sdk'
import { z } from 'zod'

const log = (name: string, input: any, output: any, start: number) => {
  const duration = Date.now() - start
  console.log(`[${new Date().toISOString()}] ${name} called with`, input)
  console.log(`→ Returned in ${duration}ms:`, output)
}

const getTime = capability({
  input: z.object({}),
  output: z.object({ time: z.string() }),
  handler: async (_, context) => {
    const start = Date.now()
    const output = { time: new Date().toISOString() }
    log("getTime", {}, output, start)
    return output
  }
})

defineServer({
  capabilities: { getTime }
}).listen()
```

---

## 🐍 Logging Example (Python)

```python
import time
from mcp import define_server, capability

@capability()
def get_time(input):
    start = time.time()
    result = { "time": datetime.utcnow().isoformat() }
    duration = round((time.time() - start) * 1000)
    print(f"[get_time] Args: {input}, Result: {result}, Duration: {duration}ms")
    return result

define_server(capabilities=[get_time]).listen()
```

---

## 📈 Monitoring Strategies

Use standard monitoring tools to track server health and metrics:

### ✅ Simple Options

- Logs to file (rotate with logrotate or similar)
- Shell scripts and cron jobs for heartbeat checks

### 🧪 Dev/Testing Tools

- Postman for manual testing
- Custom log viewers for capability calls

### ⚙️ Advanced Stack

- 📊 Prometheus + Grafana for server metrics
- 🔎 ElasticSearch + Kibana for log search and visualization
- 📁 Sentry / Datadog / New Relic for full observability

---

📌 Metrics to Monitor

| Metric | Insight Provided |
|--------|------------------|
| Requests per capability | Popular endpoints |
| Errors per capability | Stability and bugs |
| Avg response time | Performance bottlenecks |
| Server uptime | Infrastructure reliability |
| Model-to-capability ratio | AI usage vs. backend workload |

---

## 🔐 Security & Privacy Considerations

- ✅ Mask sensitive fields (e.g., tokens, PII)
- ✅ Use environment-based logging levels (DEBUG, INFO, ERROR)
- ✅ Rotate logs to avoid disk bloat
- ✅ Encrypt logs if storing sensitive information

## 🧠 Final Thoughts

Logging and monitoring aren’t just for ops — they are crucial for understanding how your MCP server interacts with AI models and how those models perform in real-world scenarios.

---

> 💡 Build observability into your MCP server from day one — you’ll thank yourself in production.
