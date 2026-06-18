# Volume 1: A Field Guide to AI Agent Integration Patterns

> *MCP, Local Tools, Skills, Steering Files, and Hooks — when to use each, and why the choice matters more than you think.*

**Research Paper — Volume 1 of 3**
*May 2026 · Luis Felipe Silveira da Silva*

---

## Executive Summary

The AI tooling landscape has fractured into five distinct integration patterns — MCP servers, local tools (function calling, including CLI wrappers as a sub-pattern), skills, steering files, and hooks — each with fundamentally different trade-offs in setup complexity, context window cost, determinism, portability, and latency. The confusion about when to use which pattern is widespread, even among experienced engineers.

This volume provides a rigorous breakdown of each pattern, a direct comparison matrix, and a decision framework specifically oriented toward building local AI packages: software that runs on a developer's machine, reads information from various places, and calls an AI model to do something useful with it.

**The core thesis:** MCP is a connectivity protocol, not a general-purpose tooling layer. For most local-package use cases, local tools (function calling) are the right default. Skills are the right layer for reusable behavioral knowledge. Steering files set the agent's standing behavioral contract. Hooks provide deterministic, event-driven enforcement. MCP earns its overhead only when you need standardized external connectors, multi-model portability, or enterprise-grade access isolation.

---

## Table of Contents

| | Title |
|--|-------|
| [01](01-foundations.md) | The Confusion Problem · Architecture Stack · Decision Framework |
| [02](02-mcp.md) | Model Context Protocol (MCP) |
| [03](03-local-tools-and-cli.md) | Local Tools (Function Calling + CLI) |
| [04](04-skills.md) | Skills (Behavioral Instructions) |
| [05](05-steering-files.md) | Steering Files (Behavioral Contract Layer) |
| [06](06-hooks.md) | Hooks (Event-Driven Automation) |
| [07](07-comparison-and-economics.md) | Head-to-Head Comparison & Context Economics |
| [08](08-local-package-guide.md) | The Local AI Package Context & Real-World Scenarios |
| [09](09-patterns-working-together.md) | Patterns Working Together |
| [10](10-misconceptions.md) | Common Misconceptions |
| [11](11-recommendations.md) | Recommendations Summary |
| [12](12-conclusion.md) | Conclusion |

---

## How Vol 1 Fits in the Series

This volume answers: **which integration pattern should I use?**

Once you have that foundation, [Vol 2 — Precision Agents](../vol2/00-overview.md) answers how to build the agent system to be accurate and self-improving. [Vol 3 — Workspace Contracts](../vol3/00-overview.md) answers how to design the environment the agent operates in.

---

*→ Start reading: [Chapter 1 — Foundations](01-foundations.md)*
