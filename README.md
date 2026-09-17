# find-oss

**From need → vetted repo → working install → first success. Open-source only.**

An agent skill that finds, vets, installs, and teaches free open-source tools and AI projects from
GitHub — in Arabic and English. Plus three modes nobody else combines in one skill:

| Mode | What it does |
|---|---|
| **Need → tool** | "I need something to manage my clipboard" → ranked, vetted picks → installed & configured → first real use |
| **Paid → OSS alternative** | "free alternative to Notion / Zapier / Manus" → mapped via community vocabulary, license catches disclosed |
| **Trending radar** | Daily/weekly/monthly open-source reports: full inventory, two-level categorization with no catch-all bucket, strategic insights, MD + optional HTML |
| **Vet a URL** | Paste any GitHub URL → six-check vetting + security smell tests + README summary |
| **Deep-dive inspect** | Open any repo safely: cached read-only clones, pinned refs, isolated worktrees, inspected-commit reporting |
| **Starter tour** | Six verified starter projects when you just want to see what's possible |

## Install

```bash
npx skills add https://github.com/aboalrejal-ai/find-oss --skill find-oss
```

Works with Claude Code, opencode, Codex CLI, Cursor, Copilot, Gemini CLI, and any agent that reads
`SKILL.md`. Add `-g` for a global install.

## Why it exists

Free tools go unfound every day because people describe the job ("I keep losing things I copy"),
not the category ("clipboard manager"). And most finder-style skills stop at a list. `find-oss`
stays with you until the tool is **running on your machine and you've had one "oh, nice" moment
with it**, and it tells you the catch *before* install (API key, VRAM, fair-code license,
experimental Windows build), not after.

Honesty rules baked in: never invent a repo, never quote stars from memory, say when open-source
is the wrong answer, and treat dead repos as dead no matter how many stars they collected.

## Structure

```
SKILL.md                     six modes + the pipeline + honesty rules
references/discovery.md      search syntax, vocabularies, channels, trending spans, alternative mapping
references/vetting.md        six checks, smell tests, deep-dive protocol, README summaries
references/install-methods.md  per-OS / per-project-type install matrix
references/reports.md        radar mode: spans, categorization, insights, MD/HTML output
references/starter-examples.md six verified starter projects
references/troubleshooting.md  the ten install failures that happen over and over
```

## License

MIT
