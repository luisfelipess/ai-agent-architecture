# Volume 3: Workspace Contracts

> *Steering Files, Tool-Surfaced Contracts, and the Model A / Model B Decision*

**Research Paper — Volume 3 of 3**
*May 2026 · Luis Felipe Silveira da Silva*

*Builds on: [Vol 1](../vol1/00-overview.md) · [Vol 2](../vol2/00-overview.md)*

---

> **Nomenclature used in this paper:** Throughout this paper, **STEERING.md** is used as the canonical term for project-level AI context files — the persistent, plain-text file an AI agent reads at the start of every session to understand the project it is working in. In practice, each tool names this file differently: `CLAUDE.md` (Anthropic Claude Code), `AGENTS.md` (OpenAI Codex — now the Linux Foundation AAIF cross-vendor standard), `GEMINI.md` (Google Gemini CLI), `.github/copilot-instructions.md` (GitHub Copilot), `.cursor/rules/*.mdc` (Cursor), `.windsurfrules` (Windsurf), `CONVENTIONS.md` (Aider). Section 7.0 maps the full naming landscape. Wherever you see STEERING.md in this paper, substitute the name your toolchain uses.

---

## Executive Summary

Volumes 1 and 2 answered two questions: which integration type should I use (MCP, local tools, skills, or steering files), and how should I build my agent system to be accurate and self-improving? This volume asks a third, equally consequential question: **how should the environment the agent operates in be designed?**

The answer depends on a fundamental architectural choice: is your agent a **raw-file traverser** that reads whatever it finds and reasons over it — or is it a **tool user** that calls deterministic code to access workspace state? This is the **Model A / Model B distinction**, and it has consequences that ripple through everything: what lives in STEERING.md vs. a tool, how dynamic workspaces are structured, and when per-folder context files help vs. hurt.

The paper is organized around four architectural tenets and the empirical evidence that supports them, culminating in a decision framework for workspace design. The core finding: **for stable codebases with fixed folder semantics, stamped context files (Model A) are appropriate and low-maintenance. For dynamic workspaces where files change on every run, policy belongs in code and the agent accesses state through tools (Model B).** Mixing the two models without understanding the domain-fit boundary is where systems fail.

---

## Table of Contents

| | Title |
|--|-------|
| [01](01-two-mental-models.md) | The Core Problem: Two Mental Models |
| [02](02-four-tenets.md) | The Four Architectural Tenets |
| [03](03-evidence-and-tradeoffs.md) | The Empirical Evidence & Tradeoffs |
| [04](04-steering-file-design.md) | What This Means for Steering File Design (STEERING.md) |
| [05](05-reference-architecture.md) | Discoverability, Decision Framework & Reference Architecture |
| [06](06-workspace-design-working-together.md) | Workspace Design: Working Across the Models |
| [07](07-misconceptions.md) | Common Misconceptions |
| [08](08-recommendations.md) | Recommendations Summary |
| [09](09-conclusion.md) | How the Series Fits Together |

---

## How Vol 3 Fits in the Series

This volume addresses the environment level of the three-volume stack:

- **Vol 1** answers "which integration?" — the building blocks
- **Vol 2** answers "how do I build the system?" — the execution architecture
- **Vol 3** answers "how do I design the workspace?" — the environment the system operates in

Each level is independent enough to implement incrementally, but they compound: a well-chosen integration pattern (Vol 1) running in a well-architected agent system (Vol 2) operating in a well-structured workspace (Vol 3) is qualitatively different from optimizing any one layer in isolation.

---

*→ Start reading: [Chapter 1 — Two Mental Models](01-two-mental-models.md)*
