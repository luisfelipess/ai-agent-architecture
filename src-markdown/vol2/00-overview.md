# Volume 2: Precision Agents

> *Progressive Context Loading, Modular Architecture, and the Accuracy Flywheel*

**Research Paper — Volume 2 of 3**
*May 2026 · Luis Felipe Silveira da Silva*

*Builds on: [Vol 1 — A Field Guide to AI Agent Integration Patterns](../vol1/00-overview.md)*

---

## Executive Summary

Volume 1 answered the question: which integration type should I use — MCP, local tools, or skills? This volume asks the harder, more consequential question: **once you have chosen your integration patterns, how do you build the system to be accurate, fast, and self-improving?**

The answer is an architecture built on five interlocking principles:

1. **The Code-First Rule** — do in code what code can do deterministically; never use an LLM for operations where the output is predictable and the logic is expressible as a function.
2. **Modular Agent Design** — each component has a single focused responsibility; failures are localized and improvements are independent.
3. **Progressive Context Loading** — start with the minimum context needed to answer the query, then expand only when the first pass fails.
4. **Context Hygiene at Handoff Boundaries** — compress execution history into typed structured state, externalize non-summarizable artifacts to storage with reference pointers, and discard scaffolding. A focused 8,000-token context consistently outperforms 400,000 tokens of accumulated history.
5. **The Accuracy Flywheel** — a measurement loop that turns observed failures into targeted improvements in routing logic, skill content, and escalation thresholds.

These principles are not theoretical. The shift from monolithic mega-prompts to progressive context architectures has been independently documented across multiple production deployments, with research confirming **25–40% reductions in irrelevant retrievals** (from progressive retrieval architectures that load domain context on-demand rather than front-loading it [Ref 5](../references.md#vol2-ref-5)) and **2–3x accuracy improvements** over monolithic baselines (from the structural shift to modular, skill-routed agents rather than single-pass mega-prompts [Ref 7](../references.md#vol2-ref-7)). These are complementary findings from independent research streams, not a single system producing both simultaneously. Single-pass success rates are separately a function of routing quality and can be systematically improved from 80% toward 95% and beyond.

---

## Table of Contents

| | Title |
|--|-------|
| [01](01-mega-prompt-trap.md) | The Problem: Why Mega-Prompts Break |
| [02](02-code-first-and-modular.md) | The Code-First Rule & Modular Agent Architecture |
| [03](03-progressive-context-loading.md) | Progressive Context Loading |
| [04](04-validation-loops.md) | Structured Output as a Validation Gate |
| [05](05-context-hygiene.md) | Context Hygiene in Long-Running Loops |
| [06](06-accuracy-flywheel.md) | The Accuracy Flywheel |
| [07](07-precision-skills-and-architecture.md) | Writing Precision Skills & Reference Architecture |
| [08](08-principles-working-together.md) | Principles Working Together |
| [09](09-misconceptions.md) | Common Misconceptions |
| [10](10-recommendations.md) | Recommendations Summary |
| [11](11-conclusion.md) | Conclusion |

---

## Citation Notes

Sources cited as **Vol1-Ref-A** through **Vol1-Ref-N** in this volume refer to citations in [Volume 1](../vol1/00-overview.md) of this series. Citations **Vol2-Ref-1** through **Vol2-Ref-11** are Volume 2-specific sources covering progressive context loading, routing, accuracy flywheel, and context hygiene topics. All references are listed in the [shared references file](../references.md).

---

## How Vol 2 Fits in the Series

This volume answers: **how do I build the system to be accurate and self-improving?**

It presupposes the integration vocabulary established in [Vol 1](../vol1/00-overview.md) — particularly the distinction between local tools, skills, and MCP, and the code-first principle introduced there. References to specific patterns from Vol 1 are noted inline.

[Vol 3 — Workspace Contracts](../vol3/00-overview.md) addresses how the environment the agent operates in should be structured. The two volumes are complementary: Vol 2 covers agent execution architecture; Vol 3 covers workspace design.

---

*→ Start reading: [Chapter 1 — Why Mega-Prompts Break](01-mega-prompt-trap.md)*
