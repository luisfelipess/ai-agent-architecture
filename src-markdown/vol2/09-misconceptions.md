# Common Misconceptions

*Vol 2 · Precision Agents*

---

| Misconception | Reality |
|--------------|---------|
| **"More context always means better answers"** | More context means more tokens, higher cost, and more noise for the model to filter. Precise, relevant context outperforms broad context. The goal is the minimum sufficient context for a given query, not the maximum available. |
| **"Multi-pass means slower"** | A well-tuned system where 80%+ of queries succeed on Pass 1 has lower *average* latency than a monolithic system that consistently loads full context. Pass 2 and beyond only add latency for the minority of queries that need them — and those queries were taking longer anyway, just silently. |
| **"Keyword routing is too simple for production"** | Keyword routing handles the majority of production queries in most domains. It is fast, cheap, and deterministic. Complexity should be added only when measurement shows it is needed. Many production systems run efficiently on keyword routing indefinitely. |
| **"The validation gate will catch everything"** | The validation gate catches structural failures: schema violations, missing fields, low confidence scores. It does not catch semantically incorrect responses that are structurally valid. Pair it with domain-specific consistency checks and human review for critical outputs. |
| **"This only works for simple queries"** | Progressive context loading works precisely because it scales: simple queries get simple (cheap, fast) treatment; complex queries get the expanded context they actually need. The architecture handles complexity by escalating deliberately, not by loading everything upfront. |
| **"More context gives the model more to work with"** | Accuracy degrades continuously as context fills — this is a consistent empirical finding across all tested frontier models. A well-structured 8,000-token handoff state outperforms 400,000 tokens of accumulated execution history on accuracy, latency, and cost. More context is more noise to filter, not more signal to use. |

> **Production Evidence:** Prompt engineering can lift accuracy from 40% to 75% quickly, but beyond that, further gains require better context architecture, modular design, and evaluation pipelines — not more wordsmithing. Teams that transition from monolithic to modular prompt architectures consistently report 2–3x accuracy improvements over simple Chain-of-Thought baselines. The gains come from eliminating irrelevant context, not from optimizing instruction phrasing. [Ref 7](../references.md#vol2-ref-7)

---

*→ Next: [Recommendations Summary](10-recommendations.md)*
*← Previous: [Principles Working Together](08-principles-working-together.md)*
