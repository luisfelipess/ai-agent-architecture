# Consolidated References

*All citations from the three-volume AI Agent Architecture series.*

---

## A Note on Evidence Tiers

Citations in this series fall into two categories, and readers should weight them differently.

**Primary sources** — vendor documentation and peer-reviewable arXiv papers — support the quantified claims stated with precision in the text. These are unambiguous: official API docs from Anthropic, Google, OpenAI, and arXiv papers with methodology sections are airtight.

**Secondary sources** — practitioner blogs and vendor analyses — are cited where primary measurement does not yet exist. The figures they carry (token-overhead percentages, accuracy ceilings, failure-rate breakdowns) are directional. They often trace to a single upstream measurement repeated across many posts and should be read as "the right order of magnitude" rather than established constants. Where a claim depends on a secondary source, the text flags it inline with a sourcing note.

Where a secondary source has a primary alternative (arXiv paper, official documentation), the series cites the primary and lists the secondary as supplementary reading.

---

## Volume 1 References — Integration Patterns

### Vol1-Ref-A
**Vol1-Ref-A: Anthropic — Tool Search Tool Documentation**
**URL:** [https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool) · *verified June 22, 2026*

Primary source for the 30–50 tool degradation threshold and the ~55,000 token multi-server setup figure. Official Anthropic tooling documentation recommending a tool search/discovery mechanism once you exceed 30–50 tools, noting that model accuracy degrades significantly beyond that threshold.

---

### Vol1-Ref-B
**Vol1-Ref-B: Bertelli & Çelik — Skills vs MCP Tools for Agents**
**Source:** LlamaIndex Blog, Feb 3 2026
**URL:** [https://www.llamaindex.ai/blog/skills-vs-mcp-tools-for-agents-when-to-use-what](https://www.llamaindex.ai/blog/skills-vs-mcp-tools-for-agents-when-to-use-what) · *verified June 22, 2026*

Source for the LlamaAgents observation that the MCP documentation server alone was sufficient for correct code generation most of the time, and that skills were "rarely invoked" in ways that substantially improved results. Key finding: MCP wins for rapidly evolving knowledge domains; skills are simpler and sufficient for stable domains.

---

### Vol1-Ref-C
**Vol1-Ref-C: Apideck — MCP Server Eating Your Context Window** *(single-origin; vendor measurement)*
**URL:** [https://www.apideck.com/blog/mcp-server-eating-context-window-cli-alternative](https://www.apideck.com/blog/mcp-server-eating-context-window-cli-alternative) · *verified June 22, 2026*

Measured 3 MCP servers (GitHub, Slack, Sentry) consuming 143,000 of 200,000 tokens (72% of context window) before any conversation began.

*Sourcing note: this figure has been widely reproduced across the industry (Milvus, Roadie, and others all cite it), but every instance traces back to this single Apideck post. Apideck is a vendor selling a CLI alternative to MCP. The measurement has not been independently reproduced by a neutral party. It is a representative data point and directional signal — not an independently verified constant.*

---

### Vol1-Ref-D
**Vol1-Ref-D: ByteIota — Perplexity Ditches MCP** *(secondary aggregator)*
**URL:** [https://byteiota.com/perplexity-ditches-mcp-72-context-waste-kills-protocol/](https://byteiota.com/perplexity-ditches-mcp-72-context-waste-kills-protocol/) · *verified June 22, 2026*

Reports Denis Yarats (Perplexity CTO) announcing at the Ask 2026 conference (March 11, 2026) that Perplexity moved away from MCP due to context window overhead.

*Sourcing note: the underlying event (Yarats at Ask 2026, March 11) is broadly corroborated by multiple independent write-ups from Cerebras, Descope, Sourcegraph, Milvus, and others. The fact is sound. However, this source is a low-quality aggregator blog. For attribution, any of the first-tier conference coverage or the named company write-ups is a stronger citation.*

---

### Vol1-Ref-E
**Vol1-Ref-E: Carey — Cloudflare Code Mode MCP**
**Source:** Cloudflare Blog, Feb 20 2026
**URL:** [https://blog.cloudflare.com/code-mode-mcp/](https://blog.cloudflare.com/code-mode-mcp/) · *verified June 22, 2026*

Documents the 244× token reduction (1,000 tokens vs. 244,000 tokens) achieved by Cloudflare's Code Mode by having models write code against pre-authorized API clients rather than exposing individual endpoints as MCP tools. Notes that full Cloudflare API exposure as MCP tools would require 1.17 million tokens.

---

### Vol1-Ref-F
**Vol1-Ref-F: Google — Function Calling with the Gemini API**
**URL:** [https://ai.google.dev/gemini-api/docs/function-calling](https://ai.google.dev/gemini-api/docs/function-calling) · *verified June 22, 2026*

Official Gemini function calling documentation. Key quote: *"While the model can use an arbitrary number of tools, providing too many can increase the risk of selecting an incorrect or suboptimal tool. For best results, aim to provide only the relevant tools for the context or task, ideally keeping the active set to a maximum of 10–20."* Also notes: *"Function descriptions and parameters count towards your input token limit."*

---

### Vol1-Ref-G
**Vol1-Ref-G: OpenAI — Model Context Protocol (MCP), Agents SDK**
**URL:** [https://openai.github.io/openai-agents-python/mcp/](https://openai.github.io/openai-agents-python/mcp/) · *verified June 22, 2026*

Official OpenAI Agents SDK MCP documentation. Covers all four transport types (Hosted, Streamable HTTP, SSE, stdio), tool filtering (static and dynamic), and the `cache_tools_list` option: *"Every agent run calls `list_tools()` on each MCP server. Remote servers can introduce noticeable latency"* — documenting why tool list caching is necessary.

---

### Vol1-Ref-H
**Vol1-Ref-H: Gloaguen et al. — "Evaluating AGENTS.md"**
**Source:** arXiv:2602.11988, Feb 2026
**URL:** [https://arxiv.org/abs/2602.11988](https://arxiv.org/abs/2602.11988) · *verified June 22, 2026*

Empirical study across 300+ coding tasks testing CLAUDE.md and AGENTS.md context files on multiple frontier models. LLM-generated context files reduce task success by 0.5–2% and increase inference cost 20–23%. Human-written context files improve ~4% at ~19% cost overhead. Recommendation: *"describe only minimal requirements."* Context files help when documentation is absent; hurt when redundant with existing docs.

---

### Vol1-Ref-I
**Vol1-Ref-I: Gao, J. (Vercel) — "AGENTS.md outperforms skills in our agent evals"**
**Source:** Vercel Blog, Jan 27 2026
**URL:** [https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals](https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals) · *verified June 22, 2026*

Eval across Next.js 16 tasks. Skills without explicit trigger instructions: 0% improvement (not invoked 56% of the time). AGENTS.md with compressed 8KB docs index: 100% pass rate vs 53% baseline. Key finding: *"no decision point"* — passive context outperforms on-demand retrieval for knowledge needed reliably on every turn. Winning format: a compact pointer map to retrievable files, not full documentation.

---

### Vol1-Ref-Hooks
**Vol1-Ref-Hooks: Hooks Cross-Framework References**

**Vol1-Ref-J: Anthropic — Automate Workflows with Hooks (Claude Code Docs)**
**URL:** [https://code.claude.com/docs/en/hooks-guide](https://code.claude.com/docs/en/hooks-guide) · *verified June 22, 2026*
Official guide covering all 29 lifecycle events, four handler types (command, http, prompt, agent), matcher patterns, input/output format, and production examples (file protection, auto-formatting, context re-injection, permission automation).

**Vol1-Ref-K: OpenAI — Lifecycle Hooks (Agents SDK)**
**URL:** [https://openai.github.io/openai-agents-python/ref/lifecycle/](https://openai.github.io/openai-agents-python/ref/lifecycle/) · *verified June 22, 2026*
`RunHooksBase` and `AgentHooksBase` API reference. Defines `on_llm_start/end`, `on_agent_start/end`, `on_tool_start/end`, `on_handoff`. Hooks are observation-only; they cannot block execution.

**Vol1-Ref-L: OpenAI — Guardrails (Agents SDK)**
**URL:** [https://openai.github.io/openai-agents-python/guardrails/](https://openai.github.io/openai-agents-python/guardrails/) · *verified June 22, 2026*
Enforcement layer separate from lifecycle hooks. Input guardrails run on first-agent input; output guardrails run on final-agent output; tool guardrails run per tool invocation. A `tripwire_triggered` flag halts execution.

**Vol1-Ref-M: LangChain — Callbacks**
**URL:** [https://python.langchain.com/docs/modules/callbacks/](https://python.langchain.com/docs/modules/callbacks/) · *verified June 22, 2026*
Event-driven callback system built around `BaseCallbackHandler`. Events include `on_llm_start`, `on_llm_end`, `on_tool_start`, `on_tool_end`, `on_chain_start`, `on_chain_end`, `on_agent_action`, `on_agent_finish`. Raise an exception to block.

**Vol1-Ref-N: Google — Callbacks and Plugins (Agent Development Kit)**
**URL:** [https://google.github.io/adk-docs/types-of-callbacks/](https://google.github.io/adk-docs/types-of-callbacks/) · *verified June 22, 2026*
ADK callback hooks: `before/after_agent_callback`, `before/after_model_callback`, `before/after_tool_callback`. Returning a value from a `before_` callback short-circuits the operation. Plugins (registered globally on the Runner) extend callbacks across all agents.

---

### Additional Background Reading (Vol 1)

- [Official MCP Specification and Architecture](https://modelcontextprotocol.io/docs/learn/architecture) · *verified June 22, 2026*
- [Portkey AI — MCP vs Function Calling](https://portkey.ai/blog/mcp-vs-function-calling/) · *verified June 22, 2026*
- [Zilliz — Function Calling vs MCP vs A2A Developer Guide](https://zilliz.com/blog/function-calling-vs-mcp-vs-a2a-developers-guide-to-ai-agent-protocols) · *verified June 22, 2026*
- [LlamaIndex — Skills vs MCP Tools for Agents](https://www.llamaindex.ai/blog/skills-vs-mcp-tools-for-agents-when-to-use-what) · *verified June 22, 2026*
- [Lunar.dev — Preventing MCP Tool Overload](https://www.lunar.dev/post/why-is-there-mcp-tool-overload-and-how-to-solve-it-for-your-ai-agents) · *verified June 22, 2026*
- [Jannik Reinhard — CLI Tools vs MCP](https://jannikreinhard.com/2026/02/22/why-cli-tools-are-beating-mcp-for-ai-agents/) · *verified June 22, 2026*
- [Improving.com — When MCP Is Not The Right Choice](https://www.improving.com/thoughts/when-mcp-is-not-the-right-choice/) · *verified June 22, 2026*
- [Dev.to — Agent vs Skill vs MCP vs Tool (4-Layer Stack)](https://dev.to/mininglamp/agent-vs-skill-vs-mcp-vs-tool-the-4-layer-stack-every-ai-developer-should-know-17no) · *verified June 22, 2026*
- [Fast.io — Function Calling vs MCP Best Architecture 2026](https://fast.io/resources/function-calling-vs-mcp/) · *verified June 22, 2026*
- [MindStudio — Reducing Token Usage in AI Agents](https://www.mindstudio.ai/blog/reduce-token-usage-ai-agents-mcp-optimization) · *verified June 22, 2026*
- [Mervin Praison — MCP vs A2A vs Function Calling Production Guide](https://mer.vin/2026/05/mcp-vs-a2a-vs-function-calling-a-practical-layered-guide-for-production-ai-agents/) · *verified June 22, 2026*

---

## Volume 2 References — Precision Agents

*Sources cited as Vol1-Ref-A through Vol1-Ref-N in Vol 2 refer to Vol 1 citations above. Citations Vol2-Ref-1 through Vol2-Ref-7 cover progressive context loading, routing, and accuracy flywheel topics. Citations Vol2-Ref-8 through Vol2-Ref-11 cover context hygiene and long-running loop management.*

---

### Vol2-Ref-1a
**Vol2-Ref-1a: Liu, N.F. et al. — "Lost in the Middle: How Language Models Use Long Contexts"**
**Source:** arXiv:2307.03172, July 2023
**URL:** [https://arxiv.org/abs/2307.03172](https://arxiv.org/abs/2307.03172) · *verified June 22, 2026*

The canonical primary source for the U-shaped attention curve (lost-in-the-middle effect). Demonstrates empirically that language models perform best when relevant information appears at the very beginning or very end of the input context; performance degrades substantially for information in the middle. Evaluated across multiple models and task types. **This is the paper to cite for the lost-in-the-middle effect — not secondary blog summaries of it.**

---

### Vol2-Ref-1
**Vol2-Ref-1: Lanham, M. — "5 Skills Every AI Agent Needs (And Why Your Mega-Prompt Is Holding You Back)"** *(secondary; practitioner blog)*
**Source:** Medium, Feb 2026
**URL:** [https://medium.com/@Micheal-Lanham/5-skills-every-ai-agent-needs-and-why-your-mega-prompt-is-holding-you-back-4b4ab2471c0e](https://medium.com/@Micheal-Lanham/5-skills-every-ai-agent-needs-and-why-your-mega-prompt-is-holding-you-back-4b4ab2471c0e) · *verified June 22, 2026*

Covers the three-level progressive disclosure model, the lost-in-the-middle effect, 90%+ activation accuracy targets, and the 50K vs 10K token comparison between monolithic and modular prompt architectures.

*Sourcing note: this is a practitioner blog post, not primary research. The lost-in-the-middle effect it describes has a canonical primary source (Liu et al. 2023, Vol2-Ref-1a above). The "recurring tax" framing and the practical modular-vs-monolithic comparison are useful secondary commentary; the empirical claims should be traced to primary sources.*

---

### Vol2-Ref-2
**Vol2-Ref-2: SkillRouter: Skill Routing for LLM Agents at Scale**
**Source:** arXiv:2603.22455, March 2026
**URL:** [https://arxiv.org/abs/2603.22455](https://arxiv.org/abs/2603.22455) · *verified June 22, 2026*

Retrieve-and-rerank skill routing pipeline achieving 74% top-1 accuracy at 80,000-skill scale. Key finding: the full skill body, not just metadata, is the decisive routing signal in large overlapping skill libraries.

---

### Vol2-Ref-3
**Vol2-Ref-3: Corrective Retrieval Augmented Generation (CRAG)**
**Source:** arXiv:2401.15884
**URL:** [https://arxiv.org/abs/2401.15884](https://arxiv.org/abs/2401.15884) · *verified June 22, 2026*

Introduces a lightweight retrieval evaluator that assigns confidence scores to retrieved context and triggers corrective actions at three confidence levels (high/medium/low). CRAG modules recover 2–3 F1 points over baseline RAG in production evaluations.

---

### Vol2-Ref-4
**Vol2-Ref-4: Blueprint First, Model Second**
**Source:** arXiv:2508.02721
**URL:** [https://arxiv.org/pdf/2508.02721](https://arxiv.org/pdf/2508.02721) · *verified June 22, 2026*

Formalizes the code-first principle for agentic systems: encode workflow logic as deterministic execution blueprints; invoke the LLM only for bounded, non-deterministic sub-tasks within those blueprints.

---

### Vol2-Ref-5
**Vol2-Ref-5: Agentic Retrieval-Augmented Generation: A Survey**
**Source:** arXiv:2501.09136
**URL:** [https://arxiv.org/html/2501.09136v4](https://arxiv.org/html/2501.09136v4) · *verified June 22, 2026*

Comprehensive survey of agentic RAG architectures. Documents progressive discovery patterns, multi-agent hierarchical orchestration, and production results including 25–40% reductions in irrelevant retrievals.

---

### Vol2-Ref-6
**Vol2-Ref-6: A-RAG: Keyword, Semantic, and Chunk-Level Retrieval Framework**
**Source:** arXiv:2603.19415, Feb 2026
**URL:** [https://arxiv.org/pdf/2603.19415](https://arxiv.org/pdf/2603.19415) · *verified June 22, 2026*

Exposes keyword, semantic, and chunk-level retrieval tools directly to the agent. Reports 5–13% improvement in QA accuracy over flat retrieval baselines.

---

### Vol2-Ref-7
**Vol2-Ref-7: Softcery — "The AI Agent Prompt Engineering Trap: Diminishing Returns and Real Solutions"** *(secondary; vendor analysis blog)*
**URL:** [https://softcery.com/lab/the-ai-agent-prompt-engineering-trap-diminishing-returns-and-real-solutions](https://softcery.com/lab/the-ai-agent-prompt-engineering-trap-diminishing-returns-and-real-solutions) · *verified June 22, 2026*

Documents the accuracy ceiling of prompt engineering (typically ~75%) and the evidence that further gains require modular design, evaluation pipelines, and architectural changes rather than further wordsmithing.

*Sourcing note: this is a vendor analysis blog. The ~75% ceiling figure is directional and should not be cited as a precise measured constant — it varies by domain, task type, and model. The directional claim (further wordsmithing yields diminishing returns; architectural changes are required for improvement) is well-supported by the structural failure modes described in Vol 2 and by the primary sources (Liu et al., ContextEvolve, E-mem). Use this source for color commentary; rely on primary sources for the underlying claims.*

---

### Vol2-Ref-8
**Vol2-Ref-8: Zylos AI Research — "AI Agent Context Compression: Strategies for Long-Running Sessions"** *(secondary; vendor research blog)*
**Source:** Zylos AI, Feb 2026
**URL:** [https://zylos.ai/research/2026-02-28-ai-agent-context-compression-strategies](https://zylos.ai/research/2026-02-28-ai-agent-context-compression-strategies) · *verified June 22, 2026*

Documents context drift as the dominant multi-agent failure mode. Context degradation is a continuous process beginning at token one across all tested frontier models; nearly 65% of enterprise AI failures in 2025 were attributed to context drift, not exhaustion.

*Sourcing note: this is a vendor research blog. The "65%" figure is a round number with no traceable upstream methodology — it is the kind of statistic that circulates without a primary source. The context-drift-as-dominant-failure claim is directionally supported by the primary sources (ContextEvolve arXiv:2602.02597, E-mem arXiv:2601.21714). Cite those for the substantive claim; treat this figure as illustrative only.*

---

### Vol2-Ref-9
**Vol2-Ref-9: ContextEvolve: Multi-Agent Context Compression for Systems Code Optimization**
**Source:** arXiv:2602.02597, Feb 2026
**URL:** [https://arxiv.org/abs/2602.02597](https://arxiv.org/abs/2602.02597) · *verified June 22, 2026*

Three-agent framework (Summarizer, Navigator, Sampler) for long-horizon context management. The Summarizer Agent compresses semantic state via code-to-language abstraction, achieving a 29% reduction in token consumption and 33.3% performance improvement over prior methods.

---

### Vol2-Ref-10
**Vol2-Ref-10: E-mem: Multi-Agent Based Episodic Context Reconstruction for LLM Agent Memory**
**Source:** arXiv:2601.21714, Jan 2026
**URL:** [https://arxiv.org/abs/2601.21714](https://arxiv.org/abs/2601.21714) · *verified June 22, 2026*

Hierarchical architecture where assistant agents maintain uncompressed per-episode contexts and a master agent orchestrates global planning. Achieves 54% F1 on LoCoMo, 7.75% above prior state-of-the-art, while reducing token cost by over 70% vs. flat context accumulation.

---

### Vol2-Ref-11
**Vol2-Ref-11: Artifacts as Memory Beyond the Agent Boundary**
**Source:** arXiv:2604.08756, April 2026
**URL:** [https://arxiv.org/abs/2604.08756](https://arxiv.org/abs/2604.08756) · *verified June 22, 2026*

Formal proof that when agents can observe environmental artifacts rather than carrying them in-context, the internal memory required to represent history is fundamentally reduced. Provides theoretical grounding for artifact externalization: the environment itself is memory.

---

## Volume 3 References — Workspace Contracts

### Vol3-Ref-1
**Vol3-Ref-1: Gloaguen et al. — "Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?"**
**Source:** arXiv:2602.11988, Feb 2026
**URL:** [https://arxiv.org/abs/2602.11988](https://arxiv.org/abs/2602.11988) · *verified June 22, 2026*

*See Vol1-Ref-H above for full annotation.* Also cited in Vol 3 for the finding that LLM-generated context files reduce task success and the concrete nuance that context files help for underdocumented repositories and hurt for popular/well-documented ones.

---

### Vol3-Ref-2
**Vol3-Ref-2: Gao, J. (Vercel) — "AGENTS.md outperforms skills in our agent evals"**
**Source:** Vercel Blog, Jan 27 2026
**URL:** [https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals](https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals) · *verified June 22, 2026*

*See Vol1-Ref-I above for full annotation.* Cited in Vol 3 for the "no decision point" mechanism and the validated pointer-file pattern for agent discoverability.

---

### Vol3-Ref-3
**Vol3-Ref-3: claude-mem.ai — Folder Context Files Documentation**
**URL:** [https://docs.claude-mem.ai/usage/folder-context](https://docs.claude-mem.ai/usage/folder-context) · *verified June 22, 2026*

Documents automated per-folder CLAUDE.md generation with activity timelines. Feature is disabled by default; explicitly scoped to "developers working on stable codebases." Preserves user-written content outside auto-generated tags. Used in Vol 3 as a real-world Model A implementation reference.

---

### Vol3-Ref-4
**Vol3-Ref-4: Linux Foundation — Agentic AI Foundation (AAIF) Formation**
**Source:** Linux Foundation Press Release, December 2025
**URL:** [https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) · *verified June 22, 2026*

AGENTS.md contributed as a founding project alongside MCP and Goose. 49 member organizations including Anthropic, OpenAI, Google, Microsoft, Amazon, Block, Bloomberg, and Cloudflare. AGENTS.md had been adopted by 60,000+ open-source repositories before the announcement.

---

### Vol3-Ref-5
**Vol3-Ref-5: llmstxt.org — "The /llms.txt File" Specification**
**URL:** [https://llmstxt.org/](https://llmstxt.org/) · *verified June 22, 2026*

Proposed standard (2025) for a Markdown file at a website root providing LLM-readable context about the site's contents. Analogous to AGENTS.md for code repositories. Adopted by 600+ sites including Anthropic, Perplexity, Stripe, Cloudflare, and Zapier as of mid-2025.

---

*← [Back to Series Index](index.md)*
