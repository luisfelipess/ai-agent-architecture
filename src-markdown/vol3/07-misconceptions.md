# Common Misconceptions

*Vol 3 · Workspace Contracts*

---

| Misconception | Reality |
|--------------|---------|
| **"Per-folder STEERING.md files are always helpful"** | They are helpful when folder semantics are stable and durable. For dynamic workspaces where files change on every run, they are stale within a few turns and add noise rather than signal. Domain fit determines usefulness. |
| **"Model B is harder, so Model A is the pragmatic default"** | Model A's pragmatism is in the short term. Ongoing maintenance of stamped files across many workspaces, each able to drift independently, has a compounding cost. Model B's upfront investment — build the library, build the tool — is bounded and one-time. |
| **"LLM-generated context files are better than none"** | Empirically false for documented domains. Gloaguen et al. found LLM-generated files reduce task success by 0.5–2% and increase cost 20–23%. They help only when the repository has no existing documentation. For well-documented codebases and stable workspaces, they hurt. |
| **"A minimal pointer file is not enough — agents need full context"** | Vercel's research found a compressed 8KB pointer index achieves 100% pass rate vs. 53% for no docs and 53% for a skill that was never triggered. The pointer file orients the agent without incurring full documentation overhead. Less context, better results. |
| **"Design docs belong in the agent context"** | Design docs are for humans. Their function in agent context is to increase token cost without improving task accuracy — the Gloaguen heavy-context finding applies. When a design doc produces new policy, that policy moves into code and skills. The doc itself stays for humans. |

---

*→ Next: [Recommendations Summary](08-recommendations.md)*
*← Previous: [Workspace Design: Working Across the Models](06-workspace-design-working-together.md)*
