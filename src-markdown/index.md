# AI Agent Architecture: A Field Guide

> *Three volumes on integration patterns, execution architecture, and workspace design for AI-powered software — compiled from hands-on experience building and using AI tools.*

---

## About This Series

I've spent a significant part of the past few years building AI-powered tools and integrating agents into real systems. Along the way, I kept running into the same class of problem: the tooling landscape matured faster than the mental models for using it. Engineers who understood LLMs deeply were still reaching for the wrong abstraction, burning context budgets on schemas that added nothing, and building systems that broke in subtle, hard-to-debug ways.

These three volumes are my attempt to write down what I learned. They grew out of questions I was answering for myself — why does MCP feel heavy for this use case, what's actually happening when a long-running agent starts drifting, when does a CLAUDE.md help and when does it hurt — and from studying the research that explains the mechanisms behind the symptoms.

This is not a tutorial. It doesn't cover any single framework or vendor. The patterns here apply whether you're building on Claude, GPT, Gemini, or an open model, and the examples skew toward Claude Code conventions only where being concrete helps. The goal throughout is to give you a clear enough mental model that you can reason about trade-offs yourself, not just follow a checklist.

If any of this saves you a few hours of confused debugging or prevents an architecture decision you'd later regret, it's done its job.

---

## The Three Questions

Each volume answers one foundational question:

| Volume | Question | Title |
|--------|----------|-------|
| Vol 1 | *Which integration pattern should I use?* | A Field Guide to AI Agent Integration Patterns |
| Vol 2 | *How do I build the system to be accurate and self-improving?* | Precision Agents |
| Vol 3 | *How should the workspace the agent operates in be designed?* | Workspace Contracts |

They compound. A well-chosen integration pattern (Vol 1) running in a well-architected agent system (Vol 2) operating in a well-structured workspace (Vol 3) is qualitatively different from optimizing any one layer in isolation.

---

## The Unifying Thesis

One principle appears in every chapter, in every form:

> **What can be done deterministically in code should be done in code.**

Integration selection, context loading, output validation, policy enforcement — each of these is better served by deterministic mechanisms than by probabilistic ones. The LLM's contribution is bounded reasoning and language generation, not infrastructure management. Every time you delegate a deterministic operation to a language model, you pay in cost, latency, and non-determinism — and get nothing for the overhead.

---

## Volume Summaries

### Volume 1 — A Field Guide to AI Agent Integration Patterns

The AI tooling landscape has five distinct integration patterns: MCP servers, local tools (function calling, with CLI tools as a sub-pattern), skills, steering files, and hooks. They sit at completely different layers of the stack and were designed for different audiences and scenarios. The confusion about when to use which is widespread.

Vol 1 builds a mental model of the full stack, introduces each pattern in depth, and provides a decision framework for choosing between them. The core finding: **MCP is a connectivity protocol, not a general-purpose tooling layer.** For most local-package use cases, local tools are the right default, skills provide behavioral customization, and MCP earns its overhead only for standardized external connectors.

→ [Start reading Vol 1](vol1/00-overview.md)

### Volume 2 — Precision Agents

Once you've chosen your integration patterns, how do you build the system to be accurate, fast, and self-improving? Vol 2 answers this with five interlocking principles: the code-first rule, modular agent design, progressive context loading, context hygiene at handoff boundaries, and the accuracy flywheel.

The core finding: **the shift from monolithic mega-prompts to progressive context architectures produces 25–40% reductions in irrelevant retrievals and 2–3x accuracy improvements.** The gains come from eliminating irrelevant context, not from optimizing instruction phrasing.

→ [Start reading Vol 2](vol2/00-overview.md)

### Volume 3 — Workspace Contracts

How should the environment the agent operates in be structured? Vol 3 introduces the Model A / Model B distinction: is your agent a raw-file traverser that reads whatever it finds, or a tool user that calls deterministic code to access workspace state? The answer has consequences that ripple through everything: what lives in STEERING.md vs. a tool, how dynamic workspaces are structured, and when per-folder context files help vs. hurt.

The core finding: **for dynamic workspaces where files change on every run, policy belongs in code and the agent accesses state through tools.** Mixing the two models without understanding the domain-fit boundary is where systems fail.

→ [Start reading Vol 3](vol3/00-overview.md)

---

## Reading Guide

**If you're new to the space**, read Vol 1 front-to-back first. It establishes the vocabulary and mental model that Vols 2 and 3 build on.

**If you're debugging an existing agent system**, go to Vol 2, Chapter 1 (The Mega-Prompt Trap) — it diagnoses the most common failure modes with precision.

**If you're designing workspace structure for an agentic pipeline**, start with Vol 3, Chapter 1 (Two Mental Models) and use the decision framework in Chapter 5.

**For ongoing reference**, the [consolidated references](references.md) list every cited source with annotations.

---

## A Note on Vocabulary

The word "tool" is overloaded across this space. This series uses the following conventions:

- **tool** (lowercase) — a function-calling tool: an in-process JSON schema the LLM can invoke
- **Tool** (MCP) — one of the three MCP server primitives (Tools, Resources, Prompts)
- **skill** — a Markdown behavioral instruction file loaded into agent context on demand
- **hook** — a lifecycle callback that executes deterministically on agent events

When "tool" appears ambiguously in a source being quoted, context will clarify which meaning applies.

---

## About the Author

*This is a personal publication, not affiliated with Amazon Web Services or Amazon. The views, opinions, and architectural recommendations expressed here are my own and do not represent the positions or endorsed practices of my employer. This series was researched and written in my personal time.*

**Luis Felipe Silveira da Silva** is a Principal Solutions Architect at Amazon Web Services, based in Dublin, Ireland. He has spent a decade working at the intersection of cloud infrastructure and enterprise architecture — first as a Solutions Engineer, then as a Solutions Architect — translating complex technical systems into clear, actionable guidance for engineering teams and decision makers. More recently, that work has centered on how AI agents fit into real production systems: not as a novelty, but as a new layer of the stack that needs its own architectural thinking.

This series is a distillation of what he's learned building and integrating AI tools — lessons earned through a combination of production experience, close reading of the research, and a lot of time staring at context window budgets.

- **LinkedIn:** [linkedin.com/in/luifelss](https://www.linkedin.com/in/luifelss/)
- **GitHub:** [github.com/luisfelipess](https://github.com/luisfelipess)

---

*Series compiled: May–June 2026.*
