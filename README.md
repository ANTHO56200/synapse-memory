<div align="center">

# 🧠 SYNAPSE

**Perpetual memory & shared brain for Claude Code — and any AI coding agent.**

One markdown file. Zero dependencies. Drag it into a conversation, and your agent never forgets.

![Version](https://img.shields.io/badge/version-3.1-2dd4bf) ![License](https://img.shields.io/badge/license-MIT-blue) ![Platforms](https://img.shields.io/badge/platforms-macOS%20·%20Linux%20·%20Windows-555) ![Works with](https://img.shields.io/badge/works%20with-Claude%20Code%20·%20Cursor%20·%20Codex%20·%20Gemini-8b5cf6)

[Install](#install) · [What you get](#what-you-get) · [How it works](#how-it-works) · [The cognitive engine](#the-cognitive-engine) · [Other agents](#beyond-claude-code) · [FAQ](#faq)

</div>

---

## The problem

Every AI coding session starts from zero. Your agent forgets yesterday's decisions, re-researches what it already learned last month, contradicts choices it made last week, and repeats mistakes it already fixed in another project. Context compacts; knowledge evaporates.

## The fix

SYNAPSE is a **complete memory system in a single markdown file**. Give the file to your agent — that's the whole install. It builds:

- 📁 **Local memory** per project — working state, dated decisions, session archives
- 🧠 **Global brain** shared across ALL your projects — knowledge learned once, available everywhere
- 😴 A nightly **sleep cycle** — compresses, deduplicates, detects contradictions, promotes reusable learnings
- ⏳ **Freshness dates** — aging knowledge gets flagged for re-verification instead of silently rotting
- 🔗 **Reinforcement** — notes that actually get used grow stronger; unused ones fade. *What fires, wires.*

Everything is plain markdown on your machine. No cloud. No database. No MCP server. No telemetry. No lock-in.

## Install

### Claude Code — one gesture

Download **[`SKILL.md`](./SKILL.md)** and drag it into any Claude Code conversation. Say nothing else. Claude installs everything itself (~40 seconds) and reports back.

Or from the terminal:

```bash
mkdir -p ~/.claude/skills/synapse && curl -fsSL https://raw.githubusercontent.com/ANTHO56200/synapse-memory/main/SKILL.md -o ~/.claude/skills/synapse/SKILL.md
```

then open Claude Code in any project and say: *"set up SYNAPSE"*.

### Any other AI agent — portable mode

Give the file to Cursor, Codex CLI, Gemini CLI, Windsurf… and say: *"Read this file and set up SYNAPSE portable mode (Part 7)."*

All agents share **the same brain** — knowledge learned in Claude is available in Cursor, and vice versa.

## What you get

| Command | What it does |
|---|---|
| `/brief` | Where were we? 5-line project status on demand |
| `/brief full` | Complete State of the Union of both memory levels |
| `/recall <topic>` | Search ALL memory across ALL projects, with freshness warnings |
| `/decision <what>` | Log a dated decision, auto-checked for contradictions |
| `/learn <insight>` | Promote reusable knowledge to the global brain |
| `/forget <topic>` | Clean removal — soft by default, restorable from the attic |
| `/memory-map` | Visual HTML dashboard of everything your agent knows |
| `/consolidate` | Run the sleep cycle right now |
| `/memory-doctor` | Memory health score /100 + auto-fixes |

Plus the automatic part: memory auto-loads at every session start, a pre-compaction hook saves state before context loss, and the session brief warns you about unarbitrated conflicts and stale knowledge.

**The `/memory-map` dashboard** ([live demo](https://claude.ai/code/artifact/3e486f24-d565-401d-bdaf-27e6a9ae5251), [source](./demo/memory-map-demo.html)):

![SYNAPSE memory map dashboard](demo/memory-map.png)

## How it works

```
~/.claude/brain/                     GLOBAL BRAIN (all projects share it)
├── BRAIN.md                         tiny index — auto-loaded every session
├── knowledge/<domain>/*.md          semantic memory (facts, research) w/ freshness
├── practices/*.md                   procedural memory (how-tos, patterns)
├── projects/                        registry + one card per project
└── episodic/*.md                    heavy archives + attic (soft-forgotten)

<project>/.claude/memory/            LOCAL MEMORY (this project only)
├── primer.md                        working memory — auto-loaded, ≤150 lines
├── warm/                            on-demand depth (architecture, decision log)
├── cold/hindsight/                  dated session archives
└── scratch/                         ephemeral (pre-compaction dumps)
```

Memory lives on a **compression gradient**, like human memory: L0 full detail → L1 one-page summary → L2 one index line. Only L2 loads by default (≈1–2k tokens per session); depth is one file-read away. Hard limits (index ≤60 lines, primer ≤150) force compression so the hot context never bloats.

Every night (cron / Task Scheduler — or `/consolidate` manually), a headless consolidation pass runs on your existing subscription: it writes the day's hindsight, refreshes the primer surgically, flags contradictions (never arbitrates them — you own the truth), rotates old decisions to cold storage, promotes reusable learnings to the global brain, and re-ranks the index.

## The cognitive engine

SYNAPSE borrows the mechanisms that make biological memory work:

- **Compression gradient (L0/L1/L2)** — detail decays into summaries, summaries into index lines; nothing useful is lost, nothing bloated stays hot.
- **Sleep consolidation** — memory maintenance happens nightly, off your working sessions.
- **Hebbian reinforcement** — every note tracks `uses` / `last_used`. Knowledge that answers real questions earns its place in the index; dormant notes are eviction candidates (their files remain, greppable).
- **Spaced repetition** — a note re-verified *unchanged* doubles its `stale_after` (max 365d); a note that *changed* halves it (min 30d). The memory learns its own decay rate, like flashcards.
- **Freshness decay** — every fact carries `verified` + `stale_after`. Past due → flagged ⏳ in the session brief. An agent that says "let me re-check" beats one confidently citing March pricing in July.

## Beyond Claude Code

The memory itself is agent-agnostic — plain files plus a protocol. Platform support differs only in automation depth:

| Capability | Claude Code | Other agents (portable mode) |
|---|---|---|
| Two-level memory, all formats | ✅ | ✅ same files, same brain |
| Auto-load at session start | ✅ imports | ✅ via instruction file (`AGENTS.md`, rules…) |
| Commands | ✅ slash commands | ✅ trigger phrases (`synapse brief`…) |
| Session hooks (auto-brief, pre-compaction) | ✅ | where the platform supports hooks |
| Nightly consolidation | ✅ headless (`claude -p`) | headless CLI if available, else on demand |

## Privacy

- Your memory is plain markdown **on your machine**. Nothing is sent anywhere by SYNAPSE — ever.
- **Secrets, tokens, .env values and personal identifiers are never stored** — it's a hard rule in the protocol, not a setting.
- Cross-project observation is **read-only** by design.
- The consolidation LLM runs with file tools only — **no shell access**.

## FAQ

**Where exactly is my data?** `~/.claude/brain/` (global) + `<project>/.claude/memory/` (local). Delete them and SYNAPSE is gone.

**What does it cost per session?** ≈1–2k tokens of context (index + primer + brief). The hard limits exist precisely to keep this bounded.

**Multiple machines?** Make `~/.claude/brain/` a **private** git repo — the file tells your agent how (Part 4).

**Teams?** Memory is per-human by default; the installer offers a gitignore option so teammates don't inherit yours.

**Windows?** First-class and battle-tested: hooks run through Git Bash, the scheduler uses Task Scheduler, and the installer knows the platform's traps (like `@~/` imports silently failing — it uses absolute paths instead).

**How do I uninstall?** Ask your agent to "uninstall SYNAPSE" — the file documents complete removal (Part 5), with an export offered first.

## Contributing

Issues and PRs welcome — especially portable-mode reports from other agents, and platform quirks you've verified empirically. Keep the invariants (Part 9 of the file): plain files, hard limits, flagged-not-arbitrated conflicts, no secrets.

If SYNAPSE saved your context, a ⭐ helps others find it.

## License & author

MIT © 2026 [Anthony Martinez](https://github.com/ANTHO56200) — founder, [The Planet Tools](https://theplanettools.ai).

*SYNAPSE is part of the [theplanettools.ai](https://theplanettools.ai) toolbox — tools reviewed and built for the AI-first web.*
