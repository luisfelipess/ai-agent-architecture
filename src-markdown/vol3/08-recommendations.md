# Recommendations Summary

*Vol 3 · Workspace Contracts*

---

| When You Have... | Do This | Rationale |
|-----------------|---------|-----------|
| A stable codebase with fixed folder roles | **Model A:** minimal per-folder STEERING.md, human-written | Durable content, low maintenance, helps both humans and agents |
| A dynamic run-based workspace (new files every run) | **Model B:** policy in code, contract through tools | Stamped content goes stale within turns; tools are always current |
| Policy shared across many workspaces | Put in library code (`skel.yaml` / `wslib`) | Single source of truth; no duplication; automatic propagation |
| Agent not finding the right workspace tool | Add minimal pointer file (5–20 lines) | Closes discoverability gap; validated by Vercel research |
| Safety-critical contamination risk | Hard write-time guard in `wslib.write()` | Soft check (README) is probabilistic; hard check is deterministic |
| LLM-generated context files that reduce performance | Remove them | Net negative for documented domains per Gloaguen et al. |
| Design docs accumulating in agent context | Keep docs for humans; move policy to code and skills | Heavy context reduces accuracy; policy belongs in code |
| Model B without the tooling built | Build the tools first, or use Model A | Model B without tools is a worse Model A: no files, no surfacing |

---

*→ Next: [How the Series Fits Together](09-conclusion.md)*
*← Previous: [Common Misconceptions](07-misconceptions.md)*
