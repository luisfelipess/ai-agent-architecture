# Conclusion

*Vol 1 · A Field Guide to AI Agent Integration Patterns*

---

The MCP vs. local tools vs. skills question doesn't have a single universal answer — it depends entirely on the problem you're solving. The framework in this volume collapses the decision to a small set of questions about connectivity requirements, multi-client needs, credential isolation, data dynamism, and scale.

For local AI packages specifically, the practical default is: **start with function calling and skills, reach for MCP only when the abstraction clearly earns its overhead.** The overhead is real — operational complexity, context token cost, network latency, and the cognitive overhead of reasoning about a client-server protocol instead of a function call. That overhead is worth it in the scenarios where MCP shines: external SaaS connectivity, multi-client tool sharing, and enterprise-grade access isolation.

The most important takeaway is architectural clarity: know which layer of the stack you're working in, what problem that layer solves, and what trade-offs it carries. Mixing layers without understanding their contracts leads to systems that are simultaneously over-engineered in some directions and fragile in others.

Build the simplest thing that solves the problem. Add layers when the evidence — context costs, maintenance burden, portability requirements — makes the case for them.

---

## References & Further Reading

→ [Consolidated References](../references.md) — all citations for this volume.

---

## Where to Go Next

- **[Vol 2 — Precision Agents](../vol2/00-overview.md):** Once you've chosen your integration patterns, how do you build the system to be accurate and self-improving? Covers progressive context loading, the accuracy flywheel, and context hygiene in long-running loops.
- **[Vol 3 — Workspace Contracts](../vol3/00-overview.md):** How should the workspace your agent operates in be designed? Covers the Model A / Model B distinction, steering file design, and tool-surfaced contracts.

---

*← Previous: [Recommendations Summary](11-recommendations.md)*
*← [Back to Vol 1 Overview](00-overview.md)*
