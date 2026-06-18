# Recommendations Summary

*Vol 2 · Precision Agents*

---

| When You Have... | Do This | Rationale |
|-----------------|---------|-----------|
| An operation with predictable I/O | Implement in code, not AI | Determinism, zero latency, no token cost |
| A new agentic system to build | Start with keyword routing + 1 skill per domain | Measure baseline before adding complexity |
| A single-pass success rate below 80% | Root-cause each escalation category, fix the top-3 causes | Targeted improvement over wholesale rewrites |
| Skills that are firing on wrong queries | Tighten the description, add negative examples, split if overlapping | Routing precision is the highest-leverage fix |
| Long skills (5,000+ tokens) | Move detail to reference files, keep instructions behavioral | Reduce default context cost, load depth on demand |
| Invisible failures (no metrics) | Instrument every pass, log escalation root causes immediately | Cannot improve what you cannot measure |
| Growing complexity in routing logic | Test keyword recall, consider semantic routing only if gap is material | Code routing first; add complexity only when data demands it |
| Queries that consistently require Pass 3+ | Promote that domain to a dedicated subagent with focused context | Structural fix for structurally hard queries |

---

*→ Next: [Conclusion](11-conclusion.md)*
*← Previous: [Common Misconceptions](09-misconceptions.md)*
