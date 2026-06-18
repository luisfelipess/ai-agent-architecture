# Recommendations Summary

*Vol 1 · A Field Guide to AI Agent Integration Patterns*

---

| Situation | Recommended Pattern | Rationale |
|-----------|--------------------|-----------| 
| Local package, few tools (<20), single agent | **Local Tools** | Minimal overhead, easiest to reason about and debug |
| Reusable behavioral guidance, workflow encoding | **Skills** | Natural language is right for behavioral guidance; zero infrastructure |
| CLI operation where a good tool already exists | **CLI Tool** | Reuse what works; keeps context lean |
| Third-party SaaS where an official MCP server exists | **MCP (official server)** | Free connector, maintained by vendor, handles auth |
| Capability needed across multiple AI clients/models | **MCP** | Build once, N+M benefit materializes |
| Enterprise credential isolation / multi-tenant access | **MCP (Streamable HTTP)** | Credentials stay server-side; audit trail built in |
| Many tools (30+) for a single-agent application | **Function calling + dynamic tool selection** | Cap schema tokens; retrieve relevant tools by intent |
| Rapidly evolving knowledge domain (APIs, docs) | **MCP (live data source)** | Single source of truth; automatic propagation of changes |
| Stable domain knowledge, rarely changing | **Skills** | Simpler, no server, easier to maintain |
| New local package, first draft | **Local Tools + Skills** | Prove the concept without infrastructure; introduce MCP later if needed |
| Project-wide rules, never-do-this constraints, code style | **Steering File (CLAUDE.md)** | Universal applicability; no trigger logic needed; pay once per session |
| Domain knowledge that applies only to specific queries | **Skill — not Steering File** | Steering files charge you for every query; skills only load when triggered |
| Agent not reliably invoking an available skill | **Add pointer in CLAUDE.md** | Passive context is reliably read; on-demand retrieval fails 56% of the time without explicit direction [Vol1-Ref-I](../references.md#vol1-ref-i) |
| Safety guardrails, audit logging, auto-formatting (must happen on every matching event) | **Hook (PreToolUse / PostToolUse)** | Unconditional enforcement; fires regardless of LLM judgment; cannot be bypassed in permissive modes |
| Verify conditions before Claude stops (e.g., all tests pass) | **Agent hook on Stop event** | Agent hook can run the test suite and return `{"ok": false, "reason": "..."}` to force continuation |

---

*→ Next: [Conclusion](12-conclusion.md)*
*← Previous: [Common Misconceptions](10-misconceptions.md)*
