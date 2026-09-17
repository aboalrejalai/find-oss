# Starter examples

Six projects that show the range of what open source can hand someone today: a desktop utility, a
general AI agent, a self-hosted automation server, a codebase-comprehension plugin, an AI memory
library, and a code-to-video renderer. Use them when the user has no named need and just wants to
see what's possible.

**Re-verify live before quoting anything.** Stars, licenses, and install commands drift; fetch the
repo this session and read its current install section. Never state a number you didn't just check.

---

## PasteBar — clipboard history & snippets
`github.com/PasteBar/PasteBarApp` · Windows, macOS · CC BY-NC (non-commercial, limited business exception)

Unlimited clipboard history plus a library of snippets you re-paste on demand — the direct answer
to "I keep losing things I copy."

```powershell
winget install -e --id PasteBar.PasteBar
```

macOS has no cask: grab the `.dmg` from Releases or pastebar.app. If work use comes up, surface the
non-commercial license once, plainly.

---

## OpenManus — self-run general AI agent
`github.com/FoundationAgents/OpenManus` · cross-platform · MIT · Python 3.12 + an LLM API key

The reference free answer to "Manus but mine": plans and executes multi-step tasks itself. Set
expectations honestly — it's a terminal framework, not a polished app.

```bash
git clone https://github.com/FoundationAgents/OpenManus.git && cd OpenManus
uv venv --python 3.12 && source .venv/bin/activate
uv pip install -r requirements.txt
playwright install                                  # optional: browser tasks
cp config/config.example.toml config/config.toml    # your API key goes here
python main.py
```

---

## n8n — self-hosted workflow automation
`github.com/n8n-io/n8n` · cross-platform · Sustainable Use License (fair-code, not OSI)

A Zapier/Make replacement you host yourself: 400+ integrations, native AI nodes. It's a stateful
service, so Docker is the right home — isolated and removable in one command.

```bash
docker volume create n8n_data
docker run -d --name n8n --restart unless-stopped \
  -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

Then http://localhost:5678. `npx n8n` is fine for a peek but persists poorly.

---

## Understand Anything — question any codebase
`github.com/Egonex-AI/Understand-Anything` · macOS, Linux, Windows · MIT

Compiles a codebase into a knowledge graph your coding agent can answer questions about. It lives
as a plugin inside agents (Claude Code, Cursor, Copilot, Codex, Gemini CLI), not as a standalone app.

Claude Code:
```
/plugin marketplace add Egonex-AI/Understand-Anything
/plugin install understand-anything
```
Shell installers also exist for macOS/Linux (`install.sh`) and Windows (`install.ps1`) on the repo.

---

## Cognee — long-term memory for agents
`github.com/topoteretes/cognee` · cross-platform · Apache-2.0 · Python 3.10+

Self-hosted knowledge-graph memory so LLM applications remember across sessions. Aimed at builders,
not end users.

```bash
uv pip install cognee        # or: pip install cognee
```
Docker: `docker run --env-file ./.env -p 8000:8000 --rm -it cognee/cognee:main`. TypeScript and Rust
clients ship too.

---

## Hyperframes — HTML in, video out
`github.com/heygen-com/hyperframes` · cross-platform · Apache-2.0 · Node 22+ and FFmpeg

Deterministic HTML/CSS compositions rendered to MP4, designed for AI agents to drive — programmatic
video without a timeline editor.

```bash
npx hyperframes init my-video && cd my-video
npx hyperframes preview      # live browser studio
npx hyperframes render       # export MP4
```
Confirm the `heygen-com` org — same-name namespaces exist elsewhere.

---

## Using these well

Don't recite the catalog. Ask what flavor of help would land — desktop utility, self-run agent, or
automation between apps — offer the one or two that fit, then run the normal vetting and install
flow on whichever they pick. The list opens conversations; it never replaces the pipeline.
