# Patterns Working Together

*Vol 1 · A Field Guide to AI Agent Integration Patterns*

---

The individual pattern chapters cover when to use each integration type and the rules that govern it in isolation. This chapter covers the rules that apply across patterns — decisions and failure modes that arise specifically when combining local tools, MCP, skills, steering files, and hooks in the same system.

---

## Do match the integration to the question you're answering

Before picking a pattern, ask: *what question am I answering?*

- "How should the agent behave?" → **Skill** (or **Steering File** if universal)
- "What exact function should I call?" → **Local Tool**
- "Does a battle-tested shell tool already do this?" → **CLI Tool**
- "How do I connect to an external service portably?" → **MCP**
- "What must happen unconditionally at lifecycle events?" → **Hook**

Mixing answers to different questions into the same artifact is the root cause of most agent instability.

---

## Do make tool selection decisions unconditional

Every tool should have a single, unambiguous trigger condition. If two tools could plausibly handle the same input, one of them should not exist. Ambiguous tool selection forces the LLM to choose between near-equivalent options, causing inconsistent behavior across runs and generating unnecessary generation cycles as the agent second-guesses itself.

---

## Don't create ambiguous fallback chains

The pattern "use MCP tool X; if it fails, try local tool Y" is an anti-pattern. It forces the agent to attempt one path, observe failure, reason about which fallback applies, and re-invoke — burning 2–4 extra generation cycles for every failure. The agent's behavior also becomes dependent on the failure mode of the first tool, which is non-deterministic in networked systems.

> **❌ Anti-pattern: ambiguous fallback**
> "If the MCP database server is unavailable, fall back to the local SQLite tool. If that also fails, try constructing the query manually from the cached results file."
> → Three tools, three failure modes, undefined selection logic. Agent will retry, hallucinate, or give up inconsistently.

> **✅ Fix: unconditional architecture**
> Have one canonical data tool. Build resilience inside that tool (with proper error handling and fallback logic in code), not across multiple tools that the LLM must orchestrate.

---

## Don't rely on retries to fix tool design problems

If your agent is frequently retrying tool calls — calling the same tool multiple times, switching between similar tools, or regenerating prompts because results look wrong — this is almost always a design problem, not a model problem.

Common causes: ambiguous tool descriptions, tools that return results the LLM can't easily interpret, skills that give behavioral guidance conflicting with available tools, or fallback logic the LLM interprets as optional.

Retries fix transient failures. Escalations fix structural failures (insufficient context). Build them as separate mechanisms with separate triggers.

---

## Don't mix behavioral guidance and operational instructions in the same artifact

Keep each artifact answering exactly one question:

- `SKILL.md` → "how should the agent approach this class of problem?"
- Local tool → "what exact operation should be performed?"
- Hook → "what must happen at this lifecycle event?"
- Steering file → "what are my standing constraints?"

When these concerns bleed into each other — a skill that lists exact command sequences, a tool that contains reasoning guidance, a hook that encodes domain business rules — you get artifacts that are neither reliable behavioral guides nor deterministic tools. The boundary violations compound over time and are hard to debug because no single artifact is obviously wrong.

---

*→ Next: [Common Misconceptions](10-misconceptions.md)*
*← Previous: [The Local AI Package Context](08-local-package-guide.md)*
