# Awesome MeetKai

> A collection of real-world OpenClaw deployments, skills, and automation patterns from the MeetKai team.

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![Use Cases](https://img.shields.io/badge/use%20cases-12-blue?style=flat-square)](https://github.com/meetkai/awesome-meetkai)
[![Skills](https://img.shields.io/badge/skills-10-green?style=flat-square)](https://github.com/meetkai/awesome-meetkai)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Not tutorials. Not concepts. Real systems running in production.**

Every use case here is deployed and running. We share the architecture, the skills, the cron jobs, and the gotchas.

🌐 [meetkai.xyz](https://meetkai.xyz) • 🐦 [@meetkai_ai](https://x.com/meetkai_ai) • 💬 [Discord](https://discord.gg/openclaw)

---

## Table of Contents

- [What is This?](#what-is-this)
- [Use Cases](#use-cases)
  - [AI CMO Agent](#ai-cmo-agent)
  - [Skill Factory](#skill-factory)
  - [Global News Aggregator](#global-news-aggregator)
  - [Research Pipeline](#research-pipeline)
  - [Multi-Channel Analytics](#multi-channel-analytics)
  - [Cold Outreach Automation](#cold-outreach-automation)
  - [Demo Video Generator](#demo-video-generator)
  - [Memory Layer](#memory-layer)
- [Skills](#skills)
- [Patterns](#patterns)
- [Contributing](#contributing)

---

## What is This?

We run OpenClaw as the backbone of a marketing automation agency. This repo documents everything we've built — the wins, the failures, and the patterns that emerged.

**Our setup:**
- Hetzner VPS (16GB RAM, Ubuntu 24.04)
- OpenClaw as orchestrator
- 10+ custom skills
- Multi-channel (Discord, Telegram, Signal)
- 30+ cron jobs running daily

**What we learned:**
- OpenClaw is the orchestrator, not the executor
- Specialization through context, not models
- Deterministic monitoring beats LLM polling
- Skills compound — build once, use forever

---

## Use Cases

### AI CMO Agent

**The core deployment.** An AI Chief Marketing Officer that runs 24/7 across multiple products.

| Component | Description |
|-----------|-------------|
| **Products** | KaiCalls, BuildWithKai, Amazing Backyard Parties, VocalScribe |
| **Channels** | Discord (10 channels), Telegram, Signal |
| **Data Sources** | Stripe, GA4, GSC, Instantly, product databases |
| **Skills** | Analytics, content writing, SEO, cold outreach, reporting |

**What it does:**
- Daily/weekly business reports at 8am ET
- Real-time Stripe alerts (new customers, churn risk)
- Lead tracking across products
- Content generation on demand
- Cold email campaign management

**Architecture:**
```
Discord channels (by product)
        ↓
    OpenClaw (Kai)
        ↓
   Skills + Memory + RAG
        ↓
   cmo CLI → Product DBs, APIs
```

📁 [Full documentation →](usecases/ai-cmo-agent.md)

---

### Skill Factory

**Overnight skill generation.** AI scrapes trending news, generates ideas, builds and tests skills automatically.

| Stage | Description |
|-------|-------------|
| **Scrape** | X/Twitter, YouTube, HN, arXiv, GitHub Trending |
| **Extract** | Topic clustering via ChromaDB |
| **Ideate** | GPT-4o generates skill specs from trends |
| **Build** | Codex/Claude Code builds the skill |
| **Test** | Automated test suite |
| **Publish** | GitHub + website + Discord announcement |

**Schedule:** 5am ET daily

**Output:** Fresh skills every morning based on what's trending in AI.

📁 [Full documentation →](usecases/skill-factory.md)

---

### Global News Aggregator

**40+ sources, 12 regions.** Avoid the Western news bubble.

| Region | Sources |
|--------|---------|
| 🇨🇳 China | Global Times, Caixin, CGTN, Xinhua |
| 🇷🇺 Russia | TASS, RT, Moscow Times |
| 🇯🇵 Japan | NHK, Japan Times, Nikkei Asia |
| 🇩🇪 Germany | Der Spiegel, DW |
| 🇫🇷 France | France24, RFI, Le Monde |
| + 7 more | India, Brazil, Israel, Gulf, Korea, Africa, LatAm |

**Commands:**
```bash
news global      # All regions
news country japan   # Deep dive
news brief       # Quick headlines
```

📁 [Full documentation →](usecases/global-news-aggregator.md)

---

### Research Pipeline

**Automated paper discovery and summarization.**

- arXiv cs.AI/cs.MA daily scan
- Semantic Scholar integration
- Auto-save learnings to `/research/learnings/`
- RAG-indexed knowledge base (83 marketing files)

**What it catches:**
- Agent architectures (multi-agent, hierarchical)
- New model capabilities
- Safety research
- Relevant papers for our products

📁 [Full documentation →](usecases/research-pipeline.md)

---

### Multi-Channel Analytics

**One CLI for all products.**

```bash
cmo kaicalls leads --days=7     # KaiCalls leads
cmo bwk dashboard               # BuildWithKai metrics
cmo ga4 all --days=7            # All sites traffic
cmo stripe_report mrr           # Combined MRR
cmo daily_report executive      # Quick status
```

Connects to:
- 4 product databases (Supabase, Postgres)
- Google Analytics 4 (10 sites)
- Google Search Console (9 sites)
- Stripe (unified account)

📁 [Full documentation →](usecases/multi-channel-analytics.md)

---

### Cold Outreach Automation

**Instantly + Apollo + AI personalization.**

| Component | Purpose |
|-----------|---------|
| **Apollo** | Lead sourcing by industry/title |
| **Instantly** | Email delivery + warmup |
| **OpenClaw** | Personalization, follow-up logic |
| **Campaigns** | Law firms, home services, PR |

**Results:**
- 8,800+ leads loaded
- Automated follow-up sequences
- AI-generated `{{pain_point}}` variables

📁 [Full documentation →](usecases/cold-outreach-automation.md)

---

### Demo Video Generator

**Remotion-based video pipeline.**

- Config-driven industry templates (plumber, electrician, HVAC)
- ElevenLabs voice synthesis
- Cinematic captions
- Auto-render and publish

**Output:** 30-second vertical demo videos for each industry.

📁 [Full documentation →](usecases/demo-video-generator.md)

---

### Memory Layer

**Hybrid retrieval system.**

- Vector search (ChromaDB + OpenAI embeddings)
- BM25 keyword search
- Knowledge graph (entities + relationships)
- Automatic memory consolidation
- Wisdom extraction from repeated patterns

📁 [Full documentation →](usecases/memory-layer.md)

---

## Skills

| Skill | Description | Status |
|-------|-------------|--------|
| `content-writer` | Blog posts, emails, ad copy | ✅ Live |
| `linkedin-writing` | LinkedIn articles (7-phase workflow) | ✅ Live |
| `seo-content` | Featured Snippets, AI Overviews | ✅ Live |
| `marketing-knowledge` | 83-file knowledge base RAG | ✅ Live |
| `emoji-generator` | On-brand Discord/Slack emojis | ✅ Live |
| `weather` | Forecasts via wttr.in | ✅ Live |
| `github` | gh CLI integration | ✅ Live |
| `gog` | Google Workspace CLI | ✅ Live |
| `video-frames` | ffmpeg frame extraction | ✅ Live |
| `session-logs` | Search past conversations | ✅ Live |

📁 [All skills →](skills/)

---

## Patterns

Lessons learned from running OpenClaw in production.

### Specialization Through Context

Don't use different models for different tasks. Use different *contexts*.

| Orchestrator Context | Coder Context |
|---------------------|---------------|
| Customer CRM | Repo conventions |
| Meeting notes | Code style guide |
| Competitor intel | API schemas |
| MEMORY.md | src/components/ |

Same model, different capabilities.

### Deterministic Monitoring

Don't poll agents with LLMs — it's expensive and unreliable.

```bash
# Bad: Ask the LLM every 10 minutes
# Good: Run a shell script
.clawdbot/check-agents.sh
```

Check tmux sessions, git status, CI results. Only invoke the LLM when something needs attention.

### The Ralph Loop V2

Standard: memory → generate → evaluate → save learnings.

Improved: When an agent fails, **rewrite the prompt** before respawning. Include failure context. Maybe switch agent type.

### Git Worktrees for Parallel Agents

Each coding agent gets:
- Own git worktree (isolated branch)
- Own tmux session
- Entry in task registry JSON

No conflicts. Parallel development.

📁 [All patterns →](patterns/)

---

## Contributing

We welcome contributions!

1. **Add a use case** — Share how you're using OpenClaw
2. **Improve docs** — Fix errors, add clarity
3. **Share a pattern** — What worked for you?

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

**Rules:**
- Must be real and tested (not hypothetical)
- Include gotchas and failures
- No crypto/trading use cases

---

## License

MIT © [MeetKai](https://meetkai.xyz)

---

*Built with ☕ by the night shift.*

## Related links

- [MeetKai](https://meetkai.xyz) — the operator layer behind Kai CMO workflows.
- [KaiCalls](https://kaicalls.com) — AI voice agents for small-business phone answering and lead capture.
- [Connor Gallic](https://connorgallic.com) — founder building Kai, KaiCalls, and AI automation systems.
