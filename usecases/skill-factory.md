# Skill Factory

> Overnight skill generation based on trending AI news.

## Overview

Every night at 5am ET, this pipeline scrapes AI news, identifies trending topics, generates skill ideas, and builds them automatically.

**Goal:** Wake up to fresh, useful OpenClaw skills every morning.

**Status:** Live since Feb 28, 2026

---

## Pipeline

```
┌─────────────────────────────────────────────────────────────┐
│                    1. NEWS SCRAPING (12am)                  │
│  X/Twitter │ YouTube │ Hacker News │ arXiv │ GitHub        │
└─────────────────────────┬───────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                 2. TOPIC EXTRACTION (1am)                   │
│  ChromaDB embeddings → Cluster → Rank by mentions           │
└─────────────────────────┬───────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                  3. IDEATION (2am)                          │
│  Top topics → GPT-4o → Skill specs with test cases          │
└─────────────────────────┬───────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│               4. BUILD & TEST (3-6am)                       │
│  Codex/Claude Code → Iterative development → Tests pass     │
└─────────────────────────┬───────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                  5. PUBLISH (7am)                           │
│  GitHub → Website → Discord announcement                    │
└─────────────────────────────────────────────────────────────┘
```

---

## News Sources

| Source | Method | Focus |
|--------|--------|-------|
| X/Twitter | Nitter RSS | AI accounts, OpenClaw community |
| YouTube | yt-dlp + transcripts | AI tutorials, news channels |
| Hacker News | API | Front page AI/ML stories |
| arXiv | RSS | cs.AI, cs.MA papers |
| GitHub Trending | Scrape | AI repos |

---

## Topic Extraction

Uses keyword matching + optional ChromaDB vector clustering:

```python
# Extract from scraped content
patterns = [
    r'\b(Claude|GPT|Gemini|Llama)\s*[\d.]*\b',
    r'\b(MCP|RAG|CoT|ReAct)\b',
    r'\b(agent|agents|agentic)\b',
    # ... more patterns
]

# Rank by mention count
topics = [
    {"topic": "mcp", "mentions": 7, "score": 0.70},
    {"topic": "agents", "mentions": 6, "score": 0.60},
    {"topic": "reasoning", "mentions": 5, "score": 0.50},
]
```

---

## Ideation Prompt

```markdown
You are an OpenClaw skill ideator.

Today's top AI news topics:
{{topics}}

Generate 3-5 skill ideas that:
1. Directly capitalize on this news
2. Provide immediate utility
3. Are buildable in <4 hours
4. Have clear test criteria

For each: name, description, features, test_cases, complexity
```

---

## Example Output

From Feb 28, 2026 news (MCP Registry launch, Moonshine STT):

| Skill | Description | Complexity |
|-------|-------------|------------|
| context-tracker | Track context window usage | Low |
| mcp-browser | Browse/install MCP servers | Medium |
| stt-moonshine | Local speech-to-text | Medium |
| agent-contracts | Behavioral contracts for reliability | High |

---

## CLI

```bash
skill-factory run          # Full pipeline
skill-factory scrape       # Just news
skill-factory topics       # Extract topics
skill-factory ideate       # Generate ideas
skill-factory ideas        # Show today's ideas
skill-factory publish <name>  # Publish a skill
skill-factory status       # Check pipeline
```

---

## Files

```
/opt/cmo-analytics/skill-factory/
├── run_pipeline.py       # Main orchestrator
├── scrapers/
│   ├── scrape_x.py       # Twitter/X via Nitter
│   ├── scrape_youtube.py # YouTube transcripts
│   └── scrape_all.py     # Master scraper
├── ideation/
│   ├── extract_topics.py # Topic clustering
│   └── generate_ideas.py # Skill ideation
├── publish/
│   └── publish_skill.py  # Website + GitHub
└── data/
    ├── news/             # Scraped content
    ├── topics/           # Extracted topics
    └── ideas/            # Generated ideas
```

---

## Website Integration

Skills page auto-updates: https://meetkai.xyz/skills/

- Shows built skills with GitHub links
- Shows queued ideas (tonight's build)
- Pipeline status dashboard

---

## Gotchas

1. **Nitter instances go down** — Rotate through multiple
2. **yt-dlp rate limits** — Add delays, don't hammer
3. **Fallback ideation** — Have a template-based backup if OpenAI fails
4. **Test before publish** — Automated tests catch most issues

---

## Results

- **Day 1:** Pipeline built and running
- **Ideas generated:** 5 per day
- **Skills shipped:** (measuring starts now)

The goal isn't volume — it's staying ahead of the AI news cycle with usable tools.

---

*Pipeline runs while you sleep. ☕*
