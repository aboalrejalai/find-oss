# find-oss — find, vet, and install open-source tools

[![skills.sh](https://skills.sh/b/aboalrejalai/find-oss)](https://skills.sh/aboalrejalai/find-oss)

find-oss is an agent skill that takes you from *"I need something that does X"* to a **vetted,
installed, working open-source tool** on your machine. It searches GitHub live, filters out dead
and sketchy repositories, maps paid software to free self-hosted alternatives, reports what is
trending in open source, and stays with you through install, configuration, and first real use.

Works with Claude Code, opencode, Codex CLI, Cursor, GitHub Copilot, Gemini CLI, and any agent
that reads a `SKILL.md`. Bilingual in practice: it mirrors whatever language you write in.

## Install

```bash
npx skills add https://github.com/aboalrejalai/find-oss --skill find-oss
```

Add `-g` for a global install (available in every project); without it, the skill is scoped to
the current project.

## Updates

find-oss is versionless: the skills CLI records a content hash of what you installed, so updating
is one command — it re-fetches from the repo and replaces your copy only when the content changed:

```bash
npx skills update find-oss        # project scope
npx skills update find-oss -g     # global scope
```

If the recorded source ever goes stale (for example after a repo rename), re-adding fixes it:

```bash
npx skills add https://github.com/aboalrejalai/find-oss --skill find-oss
```

## What find-oss does

| Mode | Use it when |
|---|---|
| **Need → tool** | You describe a job ("manage my clipboard", "automate between apps") and want a free tool that does it, installed and running |
| **Paid → open-source alternative** | You want a free, self-hosted replacement for Notion, Zapier, Manus, ChatGPT, Perplexity, or any paid SaaS |
| **Trending radar** | You want a daily/weekly/monthly report of hot open-source repositories, categorized, with strategic insights |
| **Vet a repository URL** | You paste a GitHub link and want to know what it is, whether it is alive, safe, and worth installing |
| **Deep-dive inspection** | You need source-level answers from a repo — version-specific behavior, implementation details — inspected safely |
| **Starter tour** | You have no specific need yet and want to see what open source can do for you today |

## How it works

### How find-oss searches GitHub

Always live, never from memory — stars, licenses, and maintenance drift. Each need is searched in
two or three vocabularies (the words you use and the words maintainers use), filtered for health
(`stars:>`, recent `pushed:>`, `topic:`, `license:`), and cross-checked against awesome-lists,
GitHub topic pages, and trending spans. Five to eight candidates in, two or three ranked picks out.

### How find-oss vets a repository

Six fast checks on every finalist: is it alive, do the stars make sense, what is the license
*really*, is there a build for your OS, how hard is the install, and what hidden prerequisites
does it need (API keys, VRAM, a database). Plus security smell tests, a real README read for the
top pick, and an optional deep-dive using read-only cached clones and isolated worktrees.

### How installation works

A per-OS, per-project-type decision matrix — installer or package manager for GUI apps, Docker for
services, `uv` for Python, `npx` for Node trials, single binaries for Rust/Go. Prerequisites are
checked before the first command, configuration is done with you (secrets never pass through the
chat), the running tool is verified out loud, and you get one concrete first action plus the
update and uninstall one-liners.

### Trending radar reports

Daily, weekly, and monthly inventories pulled from GitHub search proxies (there is no official
trending API), deduplicated, categorized two levels deep with no catch-all bucket, and finished
with 3–5 strategic insights. Output is Markdown in chat, with an optional self-contained HTML
report.

## Why not just search GitHub yourself?

Because the gap is not access, it is judgment. People search in end-user language while
maintainers tag in developer language; a 20k-star repo can be two years dead; "open source" often
means fair-code with commercial limits; and most recommendations abandon you at step three of a
six-step install. find-oss encodes the checks, ranks the picks with reasons, tells you the catch
before install instead of after, and stays until the tool actually works for you.

## Example session

> **You:** "I keep losing things I copy, is there something free for that?"
>
> **find-oss:** searches `clipboard manager` + `clipboard history` + `snippet manager` → vets the
> finalists → presents two ranked picks with the catch up front ("PasteBar is CC BY-NC — free
> personally, restricted commercially") → after your yes, installs via `winget`/`brew` → verifies
> it runs → first action: "copy three things, press the hotkey, pick one back."

## FAQ

### Is find-oss free?
Yes. The skill is MIT-licensed and it only recommends free, open-source projects.

### Does it need API keys or accounts?
No. GitHub search works unauthenticated (a `gh` login only raises rate limits). Some recommended
tools need their own keys — find-oss surfaces that before install, never after.

### Can it find self-hosted alternatives to paid SaaS?
Yes. The paid → OSS mode maps commercial tools to open-source equivalents and discloses fair-code
and non-commercial licenses plainly, especially when business use is mentioned.

### Which agents support it?
Any agent that reads `SKILL.md`: Claude Code, opencode, Codex CLI, Cursor, GitHub Copilot,
Gemini CLI, Windsurf, and more.

### How do I update it?
`npx skills update find-oss` (add `-g` for global installs). Updates are content-hash based, so
nothing changes on your machine unless the skill actually changed.

### Is it safe?
The skill reads, recommends, and asks before it touches anything: installs run only after you
approve the exact commands. skills.sh security audits (Socket, Snyk, Gen Agent Trust Hub) are
generated automatically after the first install and are public on the skill page.

## Structure

```
SKILL.md                       six modes + the pipeline + honesty rules
references/discovery.md        search syntax, vocabularies, channels, trending spans, alternative mapping
references/vetting.md          six checks, smell tests, deep-dive protocol, README summaries
references/install-methods.md  per-OS / per-project-type install matrix
references/reports.md          radar mode: spans, categorization, insights, MD/HTML output
references/starter-examples.md six verified starter projects
references/troubleshooting.md  the ten install failures that happen over and over
```

## License

MIT
