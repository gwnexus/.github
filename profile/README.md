# Gatewarden Nexus

**Persistent memory and coordination infrastructure for AI agent workflows.**

Nexus is a Supabase-backed platform that gives AI agents a shared, structured source of truth — sessions, decisions, tasks, dispatches, and project knowledge — that survives context windows, model switches, and tool restarts.

---

## What it does

Most AI agent setups lose context when a session ends, a window compacts, or a model switches. Nexus solves this by acting as the durable layer underneath your agent toolstack:

- **Session tracking** — agents write structured entries as they work; resuming agents pick up exactly where the last one left off
- **Decision records** — ADRs with a full review lifecycle, linked to the sessions that produced them
- **Dispatch system** — typed, routed work items between agents, humans, and projects
- **Project knowledge** — ingested documents, research notes, and planning items scoped per project
- **AI Gateway (BYOK)** — bring-your-own-key routing across model providers, with per-project provider bindings and cost attribution
- **MCP-native** — the entire surface is exposed as Model Context Protocol tools, usable from any MCP-compatible agent

---

## Core repos

| Repo | Language | Description |
|------|----------|-------------|
| [nexus-mcp](https://github.com/gwnexus/nexus-mcp) | TypeScript | MCP server — exposes all Nexus tools to your agent |
| [nexus-cli](https://github.com/gwnexus/nexus-cli) | Rust | `nexus init` / `pull` / `run` / `shadow` — local workspace management |
| [nexus-runtime-plugins](https://github.com/gwnexus/nexus-runtime-plugins) | TypeScript | Claude-CLI/OpenCode plugins: session compaction, cost tracking, context compression ... |
| [nexus-docs](https://github.com/gwnexus/nexus-docs) | MDX | Documentation site |
| [nexus-link](https://github.com/gwnexus/nexus-link) | Rust | Hardware telemetry agent — local node registration and pulse |

---

## Getting started

**Add the MCP server to your agent config:**

```json
{
  "mcp": {
    "nexus": {
      "command": ["npx", "--yes", "@gwdn/nexus-mcp@latest"],
      "environment": {
        "NEXUS_API_URL": "https://nexus.gatewarden.eu",
        "NEXUS_PRIVATE_TOKEN": "<your-token>"
      }
    }
  }
}
```

**Install the CLI:**

```bash
cargo install nexus-cli
nexus init    # scaffold .nexus/ in your project
nexus pull    # sync skills, plugins, directives from the platform
nexus run     # launch OpenCode with Nexus env vars pre-loaded
```

**Documentation:** [nexus.gatewarden.eu/docs](https://nexus.gatewarden.eu/docs)

---

## Architecture

```
Your agent (OpenCode / Claude / any MCP client)
    │
    ▼
nexus-mcp  ──────────────────────  Nexus Platform (Supabase)
(MCP server)                       sessions · decisions · tasks
    │                              dispatches · knowledge · ADRs
    ▼
nexus-cli                          per-project:
(local workspace)                  .nexus/  skills · directives
                                   .opencode/  plugins · commands
```

---

<sub>Built by [RELICFROG Consulting](https://relicfrog.com) · Operated by Gatewarden · MIT where noted</sub>
