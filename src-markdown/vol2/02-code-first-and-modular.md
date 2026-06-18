# The Code-First Rule & Modular Agent Architecture

*Vol 2 · Precision Agents*

---

## - The Code-First Rule

Before designing any agentic component, ask one question: **could this operation be implemented as a deterministic function?** If yes, implement it as a function. Do not use an LLM.

This principle seems obvious, but it is violated constantly in practice. Developers use LLMs to extract a field from a JSON response that a dictionary lookup would handle. They use LLMs to format dates that a one-line function could format. They route requests through AI reasoning when the routing logic is a simple pattern match. Each instance is a source of latency, cost, and non-determinism that adds no value.

The formal grounding is in "Blueprint First, Model Second" (arXiv:2508.02721), which establishes that encoding workflow logic as deterministic execution blueprints and invoking the LLM only for bounded, non-deterministic sub-tasks within those blueprints consistently outperforms end-to-end LLM reasoning. [Ref 4](../references.md#vol2-ref-4)

---

## What Belongs in Code vs. AI

**Operations that belong in code** (if you could write a unit test for the exact expected output given a specific input, it belongs here):

- Fetching data from an API with a fixed URL and schema
- Computing values from known inputs (date arithmetic, metric calculations, unit conversions)
- Formatting outputs to a known schema (JSON serialization, CSV generation, template substitution)
- Routing to a subagent or skill when the routing signal is a clear keyword or pattern match
- Parsing and validating structured AI output — if the AI returns JSON, your code validates it
- Caching and deduplication logic

**Operations that belong in AI** (where the value comes specifically from language understanding or contextual judgment):

- Classifying or interpreting ambiguous natural-language input
- Generating text, summaries, or explanations that require contextual reasoning
- Deciding which path to take when the signal is genuinely ambiguous (not when it's obvious)
- Extracting structured data from unstructured text (invoices, emails, free-form reports)
- Synthesizing information from multiple sources into a coherent response

**The practical split in a local AI package:**

| Operation | Where It Lives | Why |
|-----------|---------------|-----|
| Fetch metrics from a monitoring API | Code (HTTP client) | Fixed URL, known schema, deterministic response |
| Parse response JSON into structured objects | Code (JSON parser) | Exact, predictable transformation |
| Match incoming query to relevant skill | Code or lightweight router | Keyword matching or embedding similarity — no LLM needed |
| Interpret ambiguous user intent | LLM | Requires language understanding |
| Summarize the fetched data for the user | LLM | Requires synthesis and judgment |
| Validate that the AI output meets a schema | Code | Deterministic schema check |
| Format the final response to a template | Code | Template substitution, no judgment required |
| Decide whether to escalate to a second pass | Code (confidence check) | Measurable, deterministic gate |

---

## Modular Agent Design

A modular agent system replaces one large, general-purpose agent with a set of smaller, focused agents (or subagents) — each with a specific domain, skill set, and responsibility. The analogy is software engineering: monolithic applications are easier to start but harder to maintain, test, and improve. Modular architectures require more upfront design but produce systems that are easier to debug, optimize, and evolve.

### The Monolith Problem

A single agent loaded with all available context exhibits several failure modes:
- It must choose between overlapping tools and skills
- Accuracy on any given task is dragged down by irrelevant context
- When it fails, it's hard to know whether the problem is the routing logic, the skill content, or the base model capability
- Every fix risks breaking an unrelated workflow

### Subagent Design

Modular agent design assigns each subagent a narrow, well-defined job. A subagent for processing billing data loads only billing-relevant skills and tools. A subagent for analyzing logs loads only log-analysis context. A routing layer matches incoming queries to the appropriate subagent — based on keywords, embeddings, or simple classifiers — before any LLM invocation happens.

The benefits compound:

- Each subagent has a shorter context window, reducing both cost and lost-in-the-middle risk
- Failures are localized — if the billing subagent breaks, the log analysis subagent keeps working
- Each subagent can be independently improved, tested, and versioned
- The routing layer is code, not LLM reasoning — deterministic, fast, and measurable

### The Skill-Per-Domain Pattern

Within each subagent, further modularity comes from the skill architecture. Rather than a single monolithic skill covering everything the agent knows, each domain area gets its own focused skill. The subagent loads only the skills relevant to the current query.

Research quantifies this benefit: a modular skill library with 50 skills costs approximately 5,000 tokens for discovery (100 tokens per skill name and description) versus 50,000+ tokens for an equivalent monolithic prompt — a **10x reduction on discovery alone** [Ref 1](../references.md#vol2-ref-1). When a skill is activated, its full instructions add another 3,000–5,000 tokens. Most queries need one or two skills, not all fifty.

---

## The Routing Layer Is Code

One of the most important design decisions in modular agent systems is where routing lives. The answer is: **in code, not in the LLM**.

An LLM-based router adds latency, cost, and non-determinism to an operation that should be fast and predictable. The exception is narrow: very large skill libraries (hundreds of skills) with highly overlapping domains where a classifier model genuinely adds accuracy that keyword matching cannot match. Start with code routing; reach for a model only when measurement shows you need it.

Three routing approaches, in order of complexity:

**1. Keyword routing** (start here): Inspect the incoming text for domain-specific signals — product names, error codes, technical terminology, command patterns — and use these to select the relevant skill subset. Fast, deterministic, easy to debug. Works for the majority of production queries.

**2. Semantic routing** (when keyword recall falls short): Use embedding-based similarity to match queries to skills by meaning rather than exact text. Captures paraphrases and synonyms. Requires an embedding model and vector store. Appropriate when keyword routing leaves a material recall gap.

**3. LLM-based routing** (at scale only): For skill libraries in the hundreds with highly overlapping domains. SkillRouter (arXiv:2603.22455, March 2026) achieves 74% top-1 routing accuracy at 80,000-skill scale using a 1.2B parameter retrieve-and-rerank pipeline. Key finding: routing accuracy improves when the routing model can access the full skill body, because the skill body contains the most discriminative signals for overlapping domains. [Ref 2](../references.md#vol2-ref-2)

---

## Dos and Don'ts

**Do start with keyword routing, even if you plan to replace it.** Keyword routing is fast to build, easy to debug, and sufficient for the majority of production queries. Build it first, measure it, and only invest in semantic or LLM-based routing when you have evidence that keyword matching is leaving a material accuracy gap. Premature sophistication adds infrastructure cost before you know you need it.

**Don't use an LLM to route to a skill when code can do it.** Routing is almost always code. An LLM-based router adds latency, cost, and non-determinism to an operation that should be fast and predictable. The exceptions are narrow: very large skill libraries (hundreds of skills) with highly overlapping domains. Start with code; reach for a model only when measurement shows you need it.

**Don't write skills that cover multiple independent domains.** One skill, one domain. A skill covering both "billing inquiries" and "technical troubleshooting" is two skills in one file — the irrelevant half costs tokens and creates confusion on every query in the active domain. Split aggressively.

---

*→ Next: [Progressive Context Loading](03-progressive-context-loading.md)*
*← Previous: [The Problem: Why Mega-Prompts Break](01-mega-prompt-trap.md)*
