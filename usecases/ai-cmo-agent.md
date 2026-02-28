# AI CMO Agent

> A 24/7 AI Chief Marketing Officer managing multiple products across channels.

## Overview

This is the core MeetKai deployment — an AI agent that acts as a full-service CMO. It handles analytics, reporting, content, outreach, and operations across 4 products.

**Runtime:** 24/7 since February 2026  
**Channels:** Discord (10 channels), Telegram, Signal  
**Products:** KaiCalls, BuildWithKai, Amazing Backyard Parties, VocalScribe

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     INPUT CHANNELS                          │
│  Discord │ Telegram │ Signal │ Cron Jobs │ Webhooks        │
└─────────────────────────┬───────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                    OPENCLAW (Kai)                           │
│  SOUL.md │ AGENTS.md │ MEMORY.md │ Skills │ RAG            │
└─────────────────────────┬───────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                      CMO CLI                                │
│  kaicalls │ bwk │ abp │ ga4 │ gsc │ stripe │ daily_report  │
└─────────────────────────┬───────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                    DATA SOURCES                             │
│  Product DBs │ Stripe │ GA4 │ GSC │ Instantly │ Apollo     │
└─────────────────────────────────────────────────────────────┘
```

---

## Discord Channel Routing

Each Discord channel is scoped to a product or function:

| Channel | Purpose | Default Command |
|---------|---------|-----------------|
| #kai-calls | KaiCalls product | `cmo kaicalls` |
| #bwk | BuildWithKai | `cmo bwk` |
| #awesomebackyard | ABP leads/vendors | `cmo abp` |
| #vocal-scribe | VocalScribe | `cmo ga4 --site=vocalscribe` |
| #finance | Revenue, MRR, churn | `cmo stripe_report` |
| #updates | Daily/weekly reports | `cmo daily_report` |
| #research | Papers, learnings | Research pipeline |
| #health | System monitoring | General |

When a message comes into a channel, Kai uses the channel context to scope responses appropriately.

---

## Data Integration

### CMO CLI

All business data accessible via one CLI:

```bash
# KaiCalls
cmo kaicalls leads --days=7      # Recent leads
cmo kaicalls calls --days=7      # Call volume
cmo kaicalls dashboard           # Full metrics

# BuildWithKai
cmo bwk businesses              # All businesses
cmo bwk plans                   # Business plans generated
cmo bwk invocations             # AI usage

# Analytics
cmo ga4 all --days=7            # All 10 sites
cmo gsc opportunities --site=kaicalls  # SEO gaps

# Revenue
cmo stripe_report mrr           # Current MRR
cmo stripe_report at-risk       # Churn alerts

# Reports
cmo daily_report executive      # Quick status
cmo daily_report daily          # Full daily
```

### Database Connections

| Product | Database | Access |
|---------|----------|--------|
| KaiCalls | Supabase Postgres | Read-only |
| BuildWithKai | Supabase Postgres | Read-only |
| ABP | Supabase Postgres | Read-only |
| VocalScribe | Supabase Postgres | Read-only |

---

## Scheduled Jobs

| Job | Schedule | Action |
|-----|----------|--------|
| Daily Report | 8am ET | Post to #updates |
| Weekly Report | Mon 8am ET | Post to #updates |
| Heartbeat | Every 30 min | Check all products for alerts |
| News Digest | 7am ET | Global news to #research |
| Stripe Monitor | Every 30 min | New subs, cancellations |

### Heartbeat Tasks

Every 30 minutes, Kai checks:

1. **Email Campaign Replies** — New Instantly replies
2. **At-Risk Revenue** — Stripe cancellations/past-due
3. **KaiCalls Leads** — New leads in last period
4. **ABP Leads** — New party leads
5. **BWK Activity** — New signups, generations
6. **Traffic Anomalies** — >50% spikes/drops
7. **GitHub PR Status** — Ready-to-merge PRs

If nothing needs attention: `HEARTBEAT_OK`

---

## Skills Used

| Skill | Purpose |
|-------|---------|
| `content-writer` | Blog posts, emails, ad copy |
| `linkedin-writing` | Thought leadership articles |
| `seo-content` | Featured Snippet optimization |
| `marketing-knowledge` | 83-file RAG knowledge base |
| `gog` | Google Workspace (Gmail, Calendar, Drive) |
| `github` | Issue management, PR monitoring |

---

## Memory System

### MEMORY.md Structure

```markdown
# Memory Index

## Active Projects
- KaiCalls Cold Outreach & PR
- ABP Vendor Network

## Quick Reference
- API Keys (locations, not values)
- Stripe Product Mapping
- Key Campaign IDs
- Cron Jobs

## Contacts / Follow-ups
- Mark - Sunshine Cleaning (follow up March 12)

## CLI Cheat Sheet
```

### Daily Memory Files

`memory/YYYY-MM-DD.md` captures:
- Decisions made
- Problems solved
- New learnings
- Follow-up items

---

## Personality (SOUL.md)

Kai has a defined personality:

- **Voice:** Casual but competent (4/10 formal)
- **Approach:** Data-first, direct, no hedging
- **Signature:** ☕ on late-night updates
- **Banned words:** leverage, utilize, synergy, "I'd be happy to help"

The personality isn't decoration — it ensures consistent, non-corporate communication.

---

## Results

### Metrics (Feb 2026)

| Metric | Value |
|--------|-------|
| Messages processed | 2,000+/month |
| Reports generated | 60/month |
| Leads tracked | 500+ |
| MRR monitored | $67.91 |
| Products managed | 4 |

### Time Saved

- **Daily reports:** 30 min → 0 min (automated)
- **Lead checking:** 15 min/day → 0 min (alerts only)
- **Analytics lookup:** 5 min/query → 10 sec
- **Content drafts:** 2 hours → 15 min

---

## Gotchas

### What We Got Wrong

1. **Too much in MEMORY.md** — Split into daily files
2. **Polling for updates** — Switch to webhooks/cron
3. **Single channel** — Multi-channel routing is powerful
4. **Generic personality** — SOUL.md makes it usable

### What We'd Do Differently

- Start with heartbeat tasks from day 1
- Build the CMO CLI before the agent
- Set up proper memory structure early
- Use git worktrees for any coding tasks

---

## Files

```
/root/.openclaw/workspace/
├── SOUL.md           # Personality
├── AGENTS.md         # Role definition
├── MEMORY.md         # Index
├── HEARTBEAT.md      # Recurring checks
├── memory/           # Daily logs
└── skills/           # Custom skills

/opt/cmo-analytics/
├── cmo_cli.py        # Main CLI
├── .env              # API keys
└── reports/          # Generated reports
```

---

## Try It

1. Set up OpenClaw with Discord
2. Create SOUL.md with your personality
3. Build a simple CLI for your data sources
4. Add heartbeat tasks for what matters
5. Let it run

---

*Running since Feb 2026. Still learning every day.*
