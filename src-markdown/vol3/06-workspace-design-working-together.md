# Workspace Design: Working Across the Models

*Vol 3 · Workspace Contracts*

---

The individual chapters cover Model A vs Model B, the four tenets, empirical evidence, steering file design, and reference architecture in isolation. This chapter covers the rules that apply across those concerns — decisions and failure modes that arise specifically from how workspace contract choices, steering file design, discoverability mechanisms, and enforcement interact.

---

## Do match the model to the domain before any other decision

Model A for stable codebases with fixed folder semantics. Model B for dynamic run-based workspaces where files change on every run. This is the foundational decision — everything else (what goes in the steering file, whether to build describe-tool, how to handle discoverability) follows from it. Applying Model A to a dynamic workspace produces context that is accurate at initialization and stale within two turns. Applying Model B to a simple codebase produces engineering investment that doesn't earn its keep.

---

## Don't mix Model A and Model B without explicit boundaries

A system that uses per-folder STEERING.md files for some directories and tool-surfaced contracts for others produces inconsistent agent behavior: the agent can't know whether to look for a README or call a tool. If you start with Model A and migrate to Model B, do the migration cleanly. Hybrid systems without clear documented boundaries are harder to debug than either pure model.

---

## Do put policy in code, not in files the agent reads

Write-time guards in `wslib.write()` fire on every write regardless of agent behavior. Per-folder README warnings fire only if the agent reads them and chooses to comply. For anything safety-critical — contamination, data integrity, naming convention enforcement — only hard checks are sufficient. The soft check not only fails to help; the Gloaguen research shows it actively costs performance [Ref 1](../references.md#vol3-ref-1).

---

## Do separate design docs from agent runtime artifacts

Design docs and READMEs are for humans. When they produce new policy, that policy moves into `wslib` code and skill files. Agents read `skel.yaml` and skill files; they don't read design docs. Keeping these two sets disjoint is what preserves the Model B benefit: the agent's runtime context stays minimal and controlled.

---

## Don't build Model B if you haven't built the tools

Model B's guarantee is that the contract lives in code and surfaces through tools. If `describe-tool` doesn't exist, the contract isn't surfacing to anyone — human or agent. Committing to Model B without the tooling is committing to a worse version of Model A: no stamped files and no surfacing tools. Build the tools or use Model A.

---

*→ Next: [Common Misconceptions](07-misconceptions.md)*
*← Previous: [Discoverability, Decision Framework & Reference Architecture](05-reference-architecture.md)*
