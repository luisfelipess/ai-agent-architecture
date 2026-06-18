# Common Misconceptions

*Vol 1 · A Field Guide to AI Agent Integration Patterns*

---

| Misconception | Reality |
|--------------|---------|
| **"MCP replaces function calling"** | MCP is a transport and discovery layer built on top of function calling. An MCP tool call still ultimately becomes a function call inside the server. They work together, not instead of each other. |
| **"Skills are just system prompts"** | Skills are structured, reusable, scoped instructions that get injected contextually for specific tasks. A system prompt is always present. A skill fires only when the agent determines it's relevant. |
| **"More tools = better agent"** | More tools increases context cost and can degrade reasoning quality. All major LLM providers agree: Anthropic recommends capping at ~30 before adding dynamic tool selection [Vol1-Ref-A](../references.md#vol1-ref-a), Google's Gemini docs advise capping at 10–20 [Vol1-Ref-F](../references.md#vol1-ref-f), and OpenAI's Agents SDK ships built-in tool filtering for the same reason [Vol1-Ref-G](../references.md#vol1-ref-g). Quality and relevance over quantity. |
| **"MCP is only for enterprise"** | MCP is useful for any scenario needing cross-client portability or official vendor connectors. And it is no longer Anthropic's protocol: Anthropic donated MCP to the Linux Foundation in 2025, with OpenAI, Google, and Microsoft as co-sponsors. All now support it natively. The enterprise benefit is specifically the multi-tenant credential isolation and audit trail story. |
| **"Skills are non-deterministic so they're bad"** | Skills are appropriately non-deterministic for behavioral guidance tasks. They are the wrong choice for operations needing exact API call semantics. Matching the tool to the task eliminates the problem. |
| **"Local tools are just for prototyping"** | Local tools are the right permanent choice for simple, single-agent, in-process operations. Many production systems run entirely on function calling. The prototype-vs-production distinction is about architecture fit, not tool maturity. |
| **"CLAUDE.md can grow indefinitely — more context is better"** | Steering file content is paid on every query. Research shows LLM-generated context files reduce task success by 0.5–2% and increase cost 20–23%; even human-written files only improve ~4% at ~19% cost overhead [Vol1-Ref-H](../references.md#vol1-ref-h). Keep steering files minimal: universal rules, project identity, and compact index pointers. Domain knowledge belongs in skills, where it is only loaded when relevant. |
| **"Skills fire automatically — I just need to create the file"** | Skills require a trigger mechanism. Research shows agents fail to invoke available skills 56% of the time without explicit instructions directing them to do so [Vol1-Ref-I](../references.md#vol1-ref-i). If you need the agent to reliably use certain knowledge, either place a pointer in CLAUDE.md or add explicit trigger instructions. A skill that is never triggered is the same as no skill. |
| **"Hooks are just tools that run automatically"** | Hooks and tools are fundamentally different execution models. A tool is invoked by the LLM when it decides the tool is appropriate. A hook fires unconditionally when its lifecycle event occurs, with no LLM decision involved. Hooks can also do things tools cannot: block actions before they execute, modify tool inputs, control permissions, and inject context — all without consuming any context window tokens. |

---

*→ Next: [Recommendations Summary](11-recommendations.md)*
*← Previous: [Patterns Working Together](09-patterns-working-together.md)*
