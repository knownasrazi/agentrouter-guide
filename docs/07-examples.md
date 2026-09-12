# 07 — Practical Code Examples

## Python SDK (openai)

```python
from openai import OpenAI
import os, json
client = OpenAI(api_key=os.environ["AGENTROUTER_API_KEY"], base_url="https://agentrouter.org/v1")

# Basic
r = client.chat.completions.create(
    model="claude-sonnet-4-5-20250929",
    messages=[{"role":"system","content":"You are an expert architect."},{"role":"user","content":"Design a microservice for e-commerce"}],
    temperature=0.5, max_tokens=2000
)
print(r.choices[0].message.content)

# Streaming
with client.chat.completions.stream(model="gpt-4o", messages=[{"role":"user","content":"Explain WebSockets"}]) as stream:
    for chunk in stream:
        if chunk.choices[0].delta.content:
            print(chunk.choices[0].delta.content, end="", flush=True)

# JSON mode
r = client.chat.completions.create(
    model="gpt-4o", response_format={"type":"json_object"},
    messages=[{"role":"system","content":"Return only valid JSON."},{"role":"user","content":"3 Python libs + use cases as JSON."}]
)
print(json.loads(r.choices[0].message.content))

# Compare models
def compare(prompt, models):
    return {m: client.chat.completions.create(model=m, messages=[{"role":"user","content":prompt}], max_tokens=500).choices[0].message.content[:200] for m in models}
print(compare("Time complexity of balanced BST lookup?", ["claude-sonnet-4-5-20250929","gpt-4o","deepseek-r1"]))
```

## Node / TypeScript SDK

```typescript
import OpenAI from "openai";
const client = new OpenAI({ apiKey: process.env.AGENTROUTER_API_KEY!, baseURL: "https://agentrouter.org/v1" });

// Basic
export async function complete(prompt: string) {
  const r = await client.chat.completions.create({ model: "claude-sonnet-4-5-20250929", messages: [{role:"user", content: prompt}], max_tokens: 1000 });
  return r.choices[0].message.content;
}

// Next.js streaming route — app/api/chat/route.ts
export async function POST(req: Request) {
  const { message } = await req.json();
  const stream = await client.chat.completions.create({ model: "gpt-4o", stream: true, messages: [{role:"user", content: message}] });
  const encoder = new TextEncoder();
  return new Response(new ReadableStream({
    async start(controller) {
      for await (const chunk of stream) controller.enqueue(encoder.encode(chunk.choices[0]?.delta?.content ?? ""));
      controller.close();
    }
  }), { headers: {"Content-Type":"text/plain; charset=utf-8"} });
}

// Tool calling
const weatherTool: OpenAI.ChatCompletionTool = {
  type: "function",
  function: { name: "get_weather", description: "Fetches weather for a city", parameters: { type:"object", properties:{ city:{type:"string"}, unit:{type:"string", enum:["celsius","fahrenheit"]} }, required:["city"] } }
};
const r = await client.chat.completions.create({ model:"claude-sonnet-4-5-20250929", tools:[weatherTool], tool_choice:"auto", messages:[{role:"user", content:"Weather in Tokyo?"}] });
if (r.choices[0].finish_reason==="tool_calls") console.log(JSON.parse(r.choices[0].message.tool_calls![0].function.arguments));
```

## LangChain / LangGraph

```python
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage, SystemMessage
llm = ChatOpenAI(model="claude-sonnet-4-5-20250929", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
print(llm.invoke([SystemMessage(content="You are a senior Python engineer."), HumanMessage(content="Refactor to async/await: ...")]).content)

# Multi-agent graph — different models per node (planner=Opus, executor=Sonnet, reviewer=DeepSeek free)
from langgraph.graph import StateGraph, END
from typing import TypedDict
planner = ChatOpenAI(model="claude-opus-4-5-20250929", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
executor = ChatOpenAI(model="claude-sonnet-4-5-20250929", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
reviewer = ChatOpenAI(model="deepseek-r1", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
class S(TypedDict): task:str; plan:str; result:str; review:str
g=StateGraph(S)
g.add_node("plan", lambda s: {"plan": planner.invoke(f"Plan: {s['task']}").content})
g.add_node("execute", lambda s: {"result": executor.invoke(f"Execute: {s['plan']}").content})
g.add_node("review", lambda s: {"review": reviewer.invoke(f"Review: {s['result']}").content})
g.set_entry_point("plan"); g.add_edge("plan","execute"); g.add_edge("execute","review"); g.add_edge("review", END)
app=g.compile()
print(app.invoke({"task":"Write a FastAPI CRUD for a blog"}))
```

## LlamaIndex (RAG)

```python
from llama_index.llms.openai import OpenAI
from llama_index.core import Settings, VectorStoreIndex, SimpleDirectoryReader
Settings.llm = OpenAI(model="gpt-4o", api_key="sk-...", api_base="https://agentrouter.org/v1")
docs = SimpleDirectoryReader("./docs").load_data()
print(VectorStoreIndex.from_documents(docs).as_query_engine().query("Key findings in Q3?"))
```

## Continue.dev (VS Code)

`~/.continue/config.json`:
```json
{
  "models": [
    {"title": "Claude Sonnet (AgentRouter)","provider": "openai","model": "claude-sonnet-4-5-20250929","apiBase": "https://agentrouter.org/v1","apiKey": "sk-..."},
    {"title": "DeepSeek R1 (Free)","provider": "openai","model": "deepseek-r1","apiBase": "https://agentrouter.org/v1","apiKey": "sk-..."}
  ],
  "tabAutocompleteModel": {"title": "GLM-4.5 Air (Free)","provider": "openai","model": "glm-4.5-air","apiBase": "https://agentrouter.org/v1","apiKey": "sk-..."}
}
```

## n8n (HTTP Request node)

```
Method: POST
URL: https://agentrouter.org/v1/chat/completions
Header: Authorization: Bearer sk-...
Body: {"model":"claude-sonnet-4-5-20250929","messages":[{"role":"system","content":"You are an email classifier. Respond JSON only."},{"role":"user","content":"Classify: {{$json.email_body}}"}],"response_format":{"type":"json_object"}}
```

## Code review bot

```python
import subprocess
from openai import OpenAI
client = OpenAI(api_key="sk-...", base_url="https://agentrouter.org/v1")
def get_diff(): return subprocess.run(["git","diff","--cached"], capture_output=True, text=True).stdout
def review(diff):
    return client.chat.completions.create(
        model="claude-sonnet-4-5-20250929",
        messages=[
            {"role":"system","content":"Senior engineer review. Sections: CRITICAL/MAJOR/MINOR/SUGGESTION."},
            {"role":"user","content":f"Review diff:\n```diff\n{diff}\n```"}
        ], max_tokens=2000
    ).choices[0].message.content
diff=get_diff()
print(review(diff) if diff else "No staged changes.")
```

## Fallback pattern (prod recommended)

```python
from openai import OpenAI
clients = {
    "primary": OpenAI(api_key="sk-agentrouter", base_url="https://agentrouter.org/v1"),
    "fallback": OpenAI(api_key="sk-anthropic-direct", base_url="https://api.anthropic.com/v1"),
}
def robust_complete(prompt):
    for name, c in clients.items():
        try:
            return c.chat.completions.create(model="claude-sonnet-4-5-20250929", messages=[{"role":"user","content":prompt}], timeout=30).choices[0].message.content
        except Exception as e:
            print(f"[{name}] failed: {e}")
    raise RuntimeError("All providers failed")
```

## Cost-optimized document processor

```python
from openai import OpenAI
from pathlib import Path
client = OpenAI(api_key="sk-...", base_url="https://agentrouter.org/v1")
def process(path):
    text = Path(path).read_text()
    kind = client.chat.completions.create(model="glm-4.5-air", messages=[{"role":"system","content":"Classify in one word: contract/report/email/invoice/other"},{"role":"user","content":text[:2000]}], max_tokens=10).choices[0].message.content.strip().lower()
    entities = client.chat.completions.create(model="gpt-4o-mini", response_format={"type":"json_object"}, messages=[{"role":"system","content":"Extract JSON {parties,dates,amounts,obligations}"},{"role":"user","content":text[:4000]}], max_tokens=500).choices[0].message.content
    analysis = client.chat.completions.create(model="claude-opus-4-5-20250929", messages=[{"role":"system","content":"Legal analyst: risks/obligations/red flags"},{"role":"user","content":text}], max_tokens=3000).choices[0].message.content if kind=="contract" else None
    return {"type":kind,"entities":entities,"analysis":analysis}
```

Next → [08 Troubleshooting](./08-troubleshooting.md)
