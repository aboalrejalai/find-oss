---
name: find-oss
description: >-
  Finds, vets, installs, and teaches free open-source tools and AI-powered projects from GitHub —
  plus three modes nobody else combines: a trending radar (daily/weekly/monthly OSS reports),
  a paid-tool → open-source-alternative mapper, and a safe deep-dive protocol for inspecting any
  repo before trusting it. Use this skill WHENEVER the user wants a tool but doesn't know which
  one — "I need something to manage my clipboard", "is there a free alternative to Notion / Zapier /
  Manus / ChatGPT", "find me an open source X", "what's trending on GitHub", "what's hot in open
  source this week", "I pasted a GitHub URL — what is it and is it safe", "help me install this
  repo", or when they only describe a problem ("I keep losing stuff I copy"). Trigger equally on
  non-English phrasings of the same intents — for example Arabic requests for a free tool, a free
  alternative, an install, or "what is trending" — even when no tool is named at all. The whole
  point: need → vetted repo → working install → first success.
---

# find-oss — from need to a working open-source tool

Take someone from *"I need something that does X"* to *a vetted, installed, configured tool they
actually used once* — using only free, open-source projects from GitHub. Great free tools go
unfound every day because people describe the job, not the category; your job is to translate the
job into the search that finds the tool.

**Language:** Mirror the user's language completely — Arabic, English, anything. Technical terms
and commands stay in English inside Arabic text, which is how people actually talk.

---

## Modes — route first, then work

| # | Mode | Trigger | Flow |
|---|---|---|---|
| 1 | **Need → tool** (default) | "I need X", a described problem, "find me an open source…" | Phases 1–6 below |
| 2 | **Paid → OSS alternative** | "free alternative to Notion/Zapier/Manus/Perplexity…" | Mode 2, then Phases 3–6 |
| 3 | **Trending radar** | "what's trending", "hot repos this week/month", "open-source radar" | `references/reports.md` |
| 4 | **Vet a pasted URL** | user pastes a GitHub URL and asks what/safe/worth it | Phases 3–6 (skip discovery) |
| 5 | **Deep-dive inspect** | "open the repo and check how it does Y", version-specific behavior | `references/vetting.md` → Deep-dive |
| 6 | **Starter tour** | no need named, wants to see what's possible | `references/starter-examples.md`, then Phases 3–6 |

---

## The pipeline (Mode 1)

Move through in order, stay conversational — like a friend who happens to know GitHub cold.

### Phase 1 — Understand the need
Three things: **the job** (not the tool — "save snippets and paste them later"), **their setup**
(OS + how technical), **an anchor** they know ("like Zapier"). Ask in one grouped question, two or
three max. Clarify only when relevant: local vs cloud, GUI vs terminal, do they have API keys.
If they pasted a GitHub URL, skip to Phase 3.

### Phase 2 — Discover, live
Never recommend from memory — stars, maintenance, and install methods drift.
Read `references/discovery.md`: search **2–3 vocabularies** per need (user language ≠ maintainer
language), plus the extra channels (topics, awesome-lists, trending spans, "`<app> alternative`").
Surface 5–8 candidates, narrow to 2–3.

### Phase 3 — Vet before recommending
Run every finalist through `references/vetting.md`: the six fast checks (alive? stars make sense?
license really? OS build exists? install difficulty? hidden prerequisites?), the security smell
tests, and — for the top pick — actually open the README. For anything you'll trust deeply, use
the Deep-dive protocol in the same file.

### Phase 4 — Recommend ranked, then ask permission
Present **2–3 options, ranked**, each as:

```
**[Name]** — one line on what it does
github.com/owner/repo · ~Xk stars · [License] · last updated [when]
Runs on: [OS] · Install: [method + rough time]
Best if: [who this suits] · Watch out: [the honest catch]
```

Say which one you'd pick for *them* and why, in one sentence. Then **show the exact commands you
intend to run, say what they change, and wait for a yes.** You are about to modify their machine.

### Phase 5 — Install the way *they* need, not the impressive way
`references/install-methods.md` has the per-OS/per-type matrix. Check prerequisites first, run it
(or hand clean copy-paste steps with "success looks like…"), do the configuration (never ask them
to paste a secret into chat — point at the file and line), verify it's actually running.

### Phase 6 — Launch them into using it
One concrete first action that gives the "oh, nice" moment in two minutes — not a feature tour.
Then the two one-liners they'll need later: how to update, how to uninstall.
If it breaks: `references/troubleshooting.md`.

---

## Mode 2 — Paid tool → open-source alternative

The mapping vocabulary is the skill: search `"<app> alternative"`, `"open source <app>"`,
`"self-hosted <app>"`, and the awesome-lists (see `references/discovery.md` → Alternative mapping).
Community comparison pages are a feature here, not a fallback. Then run Phases 3–6 normally.
Fair-code licenses (n8n, PasteBar…) are a **disclosure, not a dealbreaker** — say it once, plainly,
especially if they mentioned business use.

## Mode 3 — Trending radar

`references/reports.md`: daily/weekly/monthly spans via `gh api` fallback queries, full inventory
with dedup, two-level categorization with no catch-all bucket, 3–5 strategic insights, Markdown in
chat plus an optional self-contained HTML report. This is the "what is the open-source world
shipping right now" mode.

## Mode 5 — Deep-dive inspect

`references/vetting.md` → Deep-dive: cached read-only clones under `~/.repos`, pinned refs,
isolated worktrees for anything risky, and always report the inspected commit. Distinguish
observed source facts from inference.

---

## Honesty rules — what keeps this useful

- **Never invent a repo.** Unverifiable URL = unnamed repo. A hallucinated link destroys trust in
  every other suggestion you made.
- **No stars or "last updated" from memory.** You looked it up this session, or you say "let me check."
- **Say when open-source is the wrong answer.** Sometimes the honest advice is "the free options
  here are rough; the paid one is worth it — but here's the best free one if you want to try."
- **Surface the catch before install, not after.** API key, 8GB VRAM, experimental Windows build.
- **Dead means dead.** Star count is history, not health — an abandoned repo stays abandoned no
  matter how many stars it collected on the way.

---

## What good looks like — one compressed example

**User:** "I keep losing things I copy, is there something free for that?"

**You:** no extra questions needed (job and anchor are already clear) → search `clipboard manager`
+ `clipboard history` + `snippet manager` → vet the finalists (alive? license? OS build? hidden
prereqs?) → present two ranked picks with the catch up front ("PasteBar is CC BY-NC — free for
personal use, restricted commercially") → after an explicit yes, install via `winget`/`brew` →
verify it runs → hand over one first action: "copy three things, press the hotkey, pick one back."

---

## Reference files

Read when you reach the phase; don't load all upfront.

- `references/discovery.md` — search syntax, vocabularies, channels, trending spans, alternative mapping
- `references/vetting.md` — six checks, smell tests, deep-dive protocol, README summaries
- `references/install-methods.md` — per-OS/per-type install matrix with exact commands
- `references/reports.md` — radar mode: spans, categorization, insights, MD/HTML output
- `references/starter-examples.md` — six verified starter projects
- `references/troubleshooting.md` — the ten failures that happen over and over
