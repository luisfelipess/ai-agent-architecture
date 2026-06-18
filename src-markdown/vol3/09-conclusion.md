# How the Series Fits Together

*Vol 3 · Workspace Contracts*

---

Volume 1 mapped the integration taxonomy: when to use MCP, local tools, skills, or steering files for a given capability. Volume 2 addressed agent execution architecture: how to build systems that are accurate and self-improving through progressive context loading, modular design, and the accuracy flywheel. Volume 3 addresses the workspace: how the environment the agent operates in should be structured so that the agent can trust what it finds, access policy reliably, and avoid operating on stale context.

The three volumes form a stack:

```
┌─────────────────────────────────────────────────────────────────┐
│  Vol 3: Workspace Design                                        │
│  "How do I design the environment the system operates in?"      │
│  → Model A vs Model B, policy in code, tool-surfaced contracts  │
├─────────────────────────────────────────────────────────────────┤
│  Vol 2: Execution Architecture                                  │
│  "How do I build the system to be accurate and self-improving?" │
│  → Progressive loading, validation loops, accuracy flywheel     │
├─────────────────────────────────────────────────────────────────┤
│  Vol 1: Integration Patterns                                    │
│  "Which integration type should I use?"                         │
│  → MCP, local tools, skills, steering files, hooks              │
└─────────────────────────────────────────────────────────────────┘
```

Each level is independent enough to implement incrementally, but they compound. A well-chosen integration pattern (Vol 1) running in a well-architected agent system (Vol 2) operating in a well-structured workspace (Vol 3) is qualitatively different from optimizing any one layer in isolation.

The unifying thread is the same principle stated across all three volumes: **what can be done deterministically in code should be done in code.** Integration selection, context loading, output validation, policy enforcement — all of these are better served by deterministic mechanisms than by probabilistic ones. The LLM's contribution is bounded reasoning and language generation, not infrastructure management.

---

## Conclusion

The Model A / Model B question is ultimately about trust: what can the agent trust to be current and accurate without verifying it? In a stable codebase, the agent can trust per-folder context files — they don't change much, and they describe a workspace that still looks the same. In a dynamic pipeline workspace, the agent can trust a surfacing tool call — the tool reads the actual filesystem and returns what's actually there.

Neither model is universally superior. Both have legitimate domains. The mistake is applying either without understanding the domain-fit constraint.

Build the simplest workspace contract that your domain requires. For static domains, that's a compact pointer file and well-maintained skills. For dynamic domains, that's a consuming library with enforced write-time policy, a surfacing tool for reads, and a minimal anchor for discoverability.

In both cases, the steering file's job is the same: orient the agent, not document the world.

---

## References

→ [Consolidated References](../references.md) — all citations for this volume.

---

*← Previous: [Recommendations Summary](08-recommendations.md)*
*← [Back to Vol 3 Overview](00-overview.md)*
*← [Back to Series Index](../index.md)*
