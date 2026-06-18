# The Local AI Package Context

*Vol 1 · A Field Guide to AI Agent Integration Patterns*

---

## Why Local Packages Have Different Constraints

The question that motivated this series is specifically about **local AI packages**: software that a developer runs on their own machine, which reads information from local or remote sources and uses an AI model to process, transform, or act on that information.

Examples include:
- Local code analysis tools that scan a codebase and generate a report
- Document processors that extract structured data from PDFs or spreadsheets
- Personal knowledge assistants that query local notes and files
- Developer productivity scripts that correlate information from multiple sources

These packages share a common profile: **one client, a small well-defined set of data sources, no multi-tenant access control requirements, and a developer who controls both the agent and the integrations**. This profile is meaningfully different from the enterprise connectivity scenario that MCP was designed to solve, and it changes the architecture trade-offs significantly.

---

## Why MCP Is Often Overkill for Local Packages

MCP was designed for the N×M integration problem — many AI clients needing to connect to many data sources in a standardized, secure, maintainable way. Most local packages don't have this problem. Adding MCP to a local package that doesn't need it introduces:

- A server process to start, manage, monitor, and restart
- A network hop (even local STDIO has process-communication overhead vs. in-process)
- JSON-RPC serialization/deserialization on every call
- Context bloat from tool schemas loading even for simple invocations
- Operational complexity multiplied for every developer who uses the package

Compare this to a simple local tool: define a Python or TypeScript function, register it with the LLM, and call it. One file, no servers, full stack traces, trivial to debug.

---

## When MCP Earns Its Place in Local Packages

There are specific cases where MCP is the right choice even for a local package:

| Scenario | Why MCP Helps | Example |
|----------|--------------|---------|
| The package needs to connect to a third-party SaaS that already has an official MCP server | You get a battle-tested, maintained connector for free. No reinventing the wheel. | Connecting to GitHub, Sentry, Linear, or Notion via their official MCP servers |
| You're building a tool that should work with multiple LLM clients (Claude Desktop + Cursor + your own app) | One MCP server, many clients. Build once, reuse everywhere. | A codebase intelligence server that both Claude Desktop and VS Code connect to |
| The capability requires credential isolation (API keys you don't want in the agent runtime) | MCP server holds the keys server-side. Client never sees them. | An analytics connector with read-only API credentials |
| You're contributing to a shared platform used by multiple teams | MCP turns your connector into a reusable organizational asset. | Internal MCP server for a company's proprietary database or data warehouse |

The key word is "earns." The overhead is real. It needs to be justified by genuine benefits you couldn't achieve more simply.

---

## Real-World Decision Scenarios

### Scenario A: A local script that reads a codebase and generates a summary report

The script runs locally, reads files from the filesystem, calls an LLM, and writes output. No external SaaS. Single user. One client.

| Capability | Right Pattern | Reason |
|------------|--------------|--------|
| File reading | Local Tool (`read_file`, `list_directory`) | In-process, no infrastructure needed |
| Text summarization | Inline LLM call | No tool needed — the LLM is the operation |
| Output formatting guidance | Skill (`SKILL.md` describing the report format) | Behavioral guidance, not deterministic logic |
| Safe output path enforcement | Hook (`PreToolUse: Write`) | Must fire unconditionally |
| MCP | Not warranted | Adding an MCP filesystem server adds zero value over `fs.readFileSync()` |

---

### Scenario B: A developer tool that connects to GitHub, Jira, and Slack

The tool correlates issues, code changes, and messages across three external SaaS systems. Potentially used with multiple AI clients in the future.

| Capability | Right Pattern | Reason |
|------------|--------------|--------|
| GitHub, Jira, Slack | MCP (official servers) | Vendor-maintained, handles OAuth and rate limits |
| Cross-source correlation | Local Tool or in-memory agent orchestration | Single-application logic, no server needed |
| Report generation guidance | Skill | Behavioral guidance |
| MCP earns its place | ✓ | Official servers exist; multi-client portability is plausible |

---

### Scenario C: A local CLI that reads a database, applies a transformation, and emails a report

Database is local (SQLite or Postgres on localhost). Email is via SMTP. Single user.

| Capability | Right Pattern | Reason |
|------------|--------------|--------|
| Database queries | Local Tool (`query_database` function) | A local database does not need an MCP server |
| Email sending | Local Tool (`send_email` via smtplib or nodemailer) | Simple in-process function |
| Transformation rules | Skill | Behavioral guidance |
| If the tool later moves to a server serving multiple teams | Consider MCP for the database | Access isolation becomes relevant |

The key insight: this tool is correctly built with local tools. If requirements change later — multiple teams, shared server — wrap the database in an MCP server then. Don't pre-optimize for portability you don't have.

---

### Scenario D: A general-purpose AI coding assistant

This is the scenario that pushed MCP into the mainstream (Claude Code, Cursor, Cline). The assistant needs to work with arbitrary codebases, connect to various external services, and be extensible by users.

| Capability | Right Pattern | Reason |
|------------|--------------|--------|
| Filesystem operations | Local Tools | Built into the assistant |
| Shell execution | CLI Tools (bash subprocess) | Wrapping existing shell capabilities |
| External service connectors | MCP | Users install and configure; assistant discovers at runtime via `tools/list` |
| Task-specific behavior | Skills (`SKILL.md` for code review, refactoring, documentation) | Behavioral customization |
| Universal constraints | Steering file | Project conventions, standing rules |
| Safety enforcement | Hooks | Auto-formatting, dangerous command blocking |

This is where MCP's N+M benefit is clearest. The assistant can support unlimited external connectors without writing any connector code — it speaks the MCP protocol and the ecosystem provides the rest.

---

## The Diagnostic: Is My Use Case Actually a Local Package?

If your package has:
- **One primary AI client** (your application)
- **A fixed, well-defined set of data sources** (not an open-ended extensibility requirement)
- **No multi-tenant access control** (one user or one team, not thousands of independent users)
- **Full control** over both the agent and the integrations

Then start with local tools + skills + a minimal steering file. Reach for MCP only when the evidence — in the form of official servers that already exist, or genuine multi-client portability requirements — makes the case.

The most common mistake isn't choosing the wrong integration pattern — it's choosing the most sophisticated-sounding one first and then discovering the overhead wasn't worth it.

---

*→ Next: [Chapter 9 — Patterns Working Together](09-patterns-working-together.md)*
*← Previous: [Chapter 7 — Comparison & Economics](07-comparison-and-economics.md)*
