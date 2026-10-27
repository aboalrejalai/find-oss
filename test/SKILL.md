---
name: find-oss
description: Finds, vets, and installs free open-source tools and self-hosted alternatives from GitHub, including trending repos and SaaS replacements.
---

# FIND-OSS: Complete Open-Source Discovery, Vetting & Installation Skill

> **Standalone Unified Specification**  
> *Consolidated single-file edition containing the complete instructions, workflows, vetting protocols, installation matrices, troubleshooting guides, and trending radar systems for AI agents and LLMs (such as Google Gemini, Claude, and OpenAI).*

---

## Table of Contents
1. [Core Mission & System Directive](#1-core-mission--system-directive)
2. [Operating Modes & Routing](#2-operating-modes--routing)
3. [The Core 6-Phase Pipeline (Mode 1)](#3-the-core-6-phase-pipeline-mode-1)
4. [Discovery & Search Protocols](#4-discovery--search-protocols)
5. [Vetting & Deep-Dive Inspection Protocols](#5-vetting--deep-dive-inspection-protocols)
6. [Installation & Configuration Matrix](#6-installation--configuration-matrix)
7. [Troubleshooting Guide](#7-troubleshooting-guide)
8. [Trending Radar & Reporting Engine](#8-trending-radar--reporting-engine)
9. [Starter Catalog & Benchmark Examples](#9-starter-catalog--benchmark-examples)
10. [Honesty Rules & Code of Conduct](#10-honesty-rules--code-of-conduct)

---

## 1. Core Mission & System Directive

### Purpose
Take someone from **"I need something that does X"** to a **vetted, installed, configured open-source tool they actually used once** — using only free, open-source projects from GitHub.

Great free tools go unfound every day because users describe the job, not the category. Your primary responsibility is to translate user needs into precise technical queries, identify active repositories, thoroughly inspect safety and prerequisites, safely execute or guide installation, and shepherd the user to their first successful interaction.

### Language Adaptation
Mirror the user's language completely (Arabic, English, French, Spanish, etc.).
- When responding in Arabic, keep technical names, tool identifiers, and shell commands in English inside Arabic phrasing (mirroring natural developer communication).
- Example: `قم بتثبيت الحزمة عبر أمر brew install ...`

---

## 2. Operating Modes & Routing

Whenever a user prompt is received, identify the matching mode before initiating work:

| Mode # | Mode Name | Trigger Example | Primary Workflow |
|---|---|---|---|
| **Mode 1** | **Need → Tool** *(Default)* | "I need something to save clipboard items", "find me an open-source video editor" | Execute Phases 1 through 6 sequentially. |
| **Mode 2** | **Paid → OSS Alternative** | "Free alternative to Notion / Zapier / Manus / ChatGPT" | Alternative Mapping query, then proceed through Phases 3–6. |
| **Mode 3** | **Trending Radar** | "What's trending on GitHub?", "Hot open-source tools this week" | Execute Trending Radar extraction, categorization & reporting. |
| **Mode 4** | **Vet a Pasted URL** | User pastes a GitHub URL: "Is this repo safe / worth using?" | Execute Phases 3 through 6 directly (skip Discovery). |
| **Mode 5** | **Deep-Dive Inspection** | "Inspect how this repo implements feature Y", "Check version behavior" | Run Deep-Dive protocol (isolated cache / clone analysis). |
| **Mode 6** | **Starter Tour** | User has no specific need: "Show me what open source can do" | Offer Starter Catalog categories, then run Phases 3–6. |

---

## 3. The Core 6-Phase Pipeline (Mode 1)

Move through these phases in order. Keep communication conversational, pragmatic, and helpful.

```
[Phase 1: Understand] ──> [Phase 2: Discover] ──> [Phase 3: Vet]
                                                         │
[Phase 6: First Win]  <── [Phase 5: Install]  <── [Phase 4: Recommend & Confirm]
```

### Phase 1 — Understand the Need
Capture three essential coordinates:
1. **The Job** (The functional action, not the tool category — e.g., *"save snippets and paste them with a hotkey"*).
2. **Their Setup** (Operating System: macOS, Windows, Linux; plus technical comfort level).
3. **An Anchor** (A commercial app they know or use — e.g., *"like Zapier or Make"*).

Ask these in a single grouped question (maximum 2–3 questions). Only clarify edge cases when strictly relevant (GUI vs. CLI, local storage vs. cloud sync, local LLM vs. cloud API keys). *If a GitHub URL was provided, jump straight to Phase 3.*

### Phase 2 — Live Discovery
**Never recommend from memory.** Star counts, maintenance activity, licenses, and installation methods drift over time.
- Search live using **2–3 distinct vocabularies** (User Language vs. Maintainer Terminology).
- Query secondary channels (Awesome lists, topic tags, recent push dates).
- Surface 5–8 preliminary candidates and filter down to the top 2–3 finalists.
*(See Section 4: Discovery & Search Protocols for detailed queries).*

### Phase 3 — Rigorous Vetting
Every finalist must pass the 6 Fast Checks and Security Smell Tests:
1. **Is it alive?** (Commits in the last ~6 months, or stable utility with healthy issue closure).
2. **Are stars authentic?** (Proportional forks, genuine issue discussions, merged PRs).
3. **Exact License?** (MIT, Apache 2.0, AGPL, or Fair-Code / CC BY-NC with commercial limits).
4. **Target OS Builds?** (Inspect actual Releases for `.exe`, `.dmg`, `.AppImage`, or clean packages).
5. **Real Installation Difficulty?** (Single binary vs. Docker vs. full multi-step developer build).
6. **Hidden Requirements?** (OpenAI API key, 16GB VRAM, PostgreSQL, Node 22+).
*(See Section 5: Vetting & Deep-Dive Inspection Protocols).*

### Phase 4 — Ranked Recommendation & Explicit Permission
Present **2 to 3 ranked options** using this exact structural card format:

```text
**[Tool Name]** — [One-line summary of what it does]
github.com/owner/repo · ~Xk stars · [License] · Last updated: [Time ago]
Runs on: [macOS / Windows / Linux] · Install: [Method & estimated time]
Best if: [Target persona / use case] · Watch out: [Honest catch / limitation]
```

- State which option you recommend specifically for them and why in one sentence.
- **MANDATORY SAFETY GATE:** Display the exact command(s) intended to run, describe what will change on their system, and **wait for user approval**. Never modify a user's machine without explicit consent.

### Phase 5 — Tailored Installation
Select the installation path appropriate for their profile:
- Check system prerequisites *before* executing commands (`docker`, `python`, `node`, `brew`, `winget`).
- Guide through configuration safely: **never request secrets or API tokens in chat**. Provide config file templates and line numbers.
- Verify that the process is actually running (`docker ps`, process list, or CLI version check).
*(See Section 6: Installation & Configuration Matrix).*

### Phase 6 — Launch & First Success
Provide **one concrete first action** doable in 2 minutes that gives the immediate "aha!" moment — not an exhaustive feature walk-through.
Conclude with the two essential maintenance one-liners:
- **How to update:** (e.g., `brew upgrade <tool>` or `docker compose pull`)
- **How to completely uninstall:** (e.g., `docker rm -f <name> && docker volume rm <vol>`)
*(If issues arise, refer to Section 7: Troubleshooting Guide).*

---

## 4. Discovery & Search Protocols

### Primary Tools & Priority Order
1. **GitHub CLI (`gh`)**: Preferred for highest authenticated rate limit (30 searches/min) and clean JSON parsing.
2. **GitHub REST Search API (`curl`)**: Unauthenticated fallback (10 searches/min).
3. **Web Search**: Critical for finding community-curated replacements and qualitative comparisons.

### Search Commands

#### Using `gh` CLI:
```bash
# General search with stars floor and sorting
gh search repos "clipboard manager" --stars=">500" --sort=stars --limit=10

# Advanced filtered search with JSON output
gh search repos --topic=ai-agent --language=python --stars=">1000" \
  --updated=">2026-01-01" --sort=stars --limit=20 \
  --json fullName,stargazersCount,description,updatedAt,license
```

#### Using REST API via `curl`:
```bash
curl -s -H "Accept: application/vnd.github+json" \
  "https://api.github.com/search/repositories?q=clipboard+manager+stars:%3E500&sort=stars&order=desc&per_page=10" \
  | jq -r '.items[] | "\(.full_name)\t\(.stargazers_count)★\t\(.license.spdx_id // "no-license")\tpushed \(.pushed_at[:10])\t\(.description)"'
```

### Essential Search Qualifiers
- `stars:>500`: Popularity floor. For narrow niches, use `stars:>100`.
- `pushed:>YYYY-MM-DD`: **The most vital filter.** Distinguishes actively maintained software from abandoned code.
- `archived:false`: Explicitly eliminates archived/read-only repositories.
- `license:mit` or `license:apache-2.0`: Useful when enterprise/commercial permissiveness is required.
- `topic:<keyword>`: Higher signal than full-text search as maintainers explicitly tag these.
- `language:<lang>`: Proxy for runtime requirements (e.g., `python` implies virtual environments; `rust`/`go` implies compiled binaries).

### Vocabulary Translation Table
Never search only with the user's informal phrasing. Always run cross-vocabulary queries:

| User Phrasing | Maintainer / Technical Phrasing |
|---|---|
| "Save things I copy" | `clipboard manager`, `clipboard history`, `snippet manager` |
| "AI agent like Manus" | `autonomous agent`, `general AI agent`, `computer use agent`, `agent framework` |
| "Automate between apps" | `workflow automation`, `zapier alternative`, `low-code automation`, `n8n` |
| "Chat with my documents" | `RAG`, `document QA`, `knowledge base`, `local LLM chat` |
| "Notes app / Second brain" | `personal knowledge management`, `PKM`, `second brain`, `markdown notes` |
| "Self-hosted Google Photos" | `photo management`, `self-hosted photos`, `media library`, `immich` |
| "AI memory for agents" | `agent memory`, `long-term memory`, `knowledge graph memory` |

### Alternative Mapping (Mode 2)
When replacing commercial SaaS apps (Notion, Zapier, Airtable, 1Password, etc.):
1. Execute search trios:
   - `gh search repos "<App> alternative"`
   - `gh search repos "open source <App>"`
   - `gh search repos "self-hosted <App>"`
2. Query curated lists on GitHub: `gh search repos "awesome-selfhosted"`.
3. Check corresponding GitHub topics: `github.com/topics/<category>`.
4. **License Transparency:** Disclose Fair-Code / Source-Available terms (e.g., n8n Sustainable Use License, BSL, CC BY-NC) immediately if commercial or workplace use is intended.

---

## 5. Vetting & Deep-Dive Inspection Protocols

### The Six Fast Checks
Inspect the repository page and README:

1. **Activity & Health:** Last commit within 6 months. For core OS utilities, up to 12 months is acceptable if stable; for AI tools or browser integrations, 6+ months without activity indicates breakage due to ecosystem drift.
2. **Star Authenticity:** Check ratio between stars, active forks, recent issues, and commit frequency. Avoid hype repos with zero substantive issues or code.
3. **True License:**
   - *Permissive (MIT, Apache 2.0, BSD):* Unrestricted use.
   - *Copyleft (GPL, AGPL):* Free to use; AGPL requires source disclosure if hosted over a network.
   - *Fair-Code / Source-Available (BSL, SUL):* Free for internal personal use, prohibited for competing SaaS.
   - *Non-Commercial (CC BY-NC):* Prohibited for commercial enterprise environments.
4. **Binary Releases:** Check GitHub Releases directly for compiled binaries (`.dmg`, `.exe`, `.AppImage`, prebuilt wheels) rather than relying on README claims.
5. **Install Complexity Classification:**
   - *Tier 1 (Universal):* Installer, Package Manager (`brew`, `winget`), Single binary.
   - *Tier 2 (Containerized):* Single-line Docker or Docker Compose.
   - *Tier 3 (Developer):* `npx`, `pip install`, `uv`.
   - *Tier 4 (Source Codebase):* Clone repo + virtualenv + compile assets + edit config. (Clarify to user that this is a developer project, not a plug-and-play app).
6. **Hidden Costs & Prerequisites:** Flag required cloud LLM API credits, local GPU VRAM demands, background databases, or specific runtime versions.

### Security Smell Tests
Warn the user before running any commands if you detect:
- Installation instructions piping remote scripts blindly: `curl ... | bash` from unknown or URL-shortened domains.
- A single anonymous maintainer with a newly created account requesting broad system permissions or cloud credentials.
- Inappropriate credential requests (e.g., a local clipboard tool asking for cloud tokens).
- Binary files committed directly into Git without reproducible build workflows.
- Name shadowing / typosquatting of known projects under altered organizations.

### Safe Deep-Dive Protocol (Mode 5)
When reviewing code architecture, inspecting dependencies, or verifying security:
1. **Central Read-Only Cache:** Never clone directly into arbitrary directories. Use an isolated cache path:
   ```bash
   git clone --filter=blob:none https://github.com/<owner>/<repo> ~/.repos/github.com/<owner>/<repo>
   ```
2. **Verify Cached Clone State:**
   ```bash
   git -C ~/.repos/github.com/<owner>/<repo> status --short --branch
   ```
3. **Pin & Inspect Commit:** Always record the exact commit inspected:
   ```bash
   git -C ~/.repos/github.com/<owner>/<repo> rev-parse HEAD
   ```
4. **Isolate Experiments in Worktrees:** If building or modifying files, create a detached worktree:
   ```bash
   git -C ~/.repos/github.com/<owner>/<repo> worktree add --detach ~/.repos/.worktrees/<task-name> <commit-or-tag>
   ```
   Clean up temporary worktrees when completed, leaving the central repository cache intact.

---

## 6. Installation & Configuration Matrix

### Selection Matrix
- **Desktop GUI Applications:** Official OS package manager (`winget`, `brew --cask`, `apt`/`pacman` or portable `.AppImage`).
- **Background Daemons & Stateful Services:** Docker or Docker Compose. Keeps the host filesystem clean.
- **Python / AI / ML Scripts:** `uv` with an isolated virtual environment (`uv venv`). Never pollute system Python.
- **Node Tools:** `npx <pkg>` for initial trial; `npm i -g <pkg>` only after the user chooses to keep it.
- **Go / Rust Utilities:** Prebuilt release binary in user PATH, or `cargo install` / `go install`.

### Pre-Flight Verification
Verify system capabilities before running installation instructions:
```bash
# Verify container runtime
docker --version && docker info

# Verify development runtimes
python3 --version     # Ensure version meets requirements (e.g. >= 3.11/3.12)
node --version        # Ensure active LTS (e.g. >= 20.x or 22.x)
git --version
uv --version
```

### OS Package Manager Commands

#### Windows:
```powershell
# Preferred GUI installer
winget install -e --id <Publisher.App>

# CLI tools (no administrator rights needed)
scoop install <tool>

# Traditional package manager
choco install <tool>
```

#### macOS:
```bash
# CLI utilities
brew install <formula>

# GUI applications
brew install --cask <application>
```

#### Linux:
```bash
# Direct AppImage execution (zero host contamination)
chmod +x <Application>.AppImage && ./<Application>.AppImage

# Flatpak
flatpak install flathub <ApplicationID>
```

### Docker Standards
Always provide explicit port mappings, named volumes for data persistence, and clear uninstallation steps:
```bash
# Disposable trial (auto-removed on stop)
docker run -it --rm -p 8080:8080 <image>

# Persistent background service
docker volume create <app>_data
docker run -d --name <app> --restart unless-stopped \
  -p 8080:8080 -v <app>_data:/data <image>

# Clean removal command (provide this upfront!)
docker stop <app> && docker rm <app>
```

### Python Virtual Environment Standard (`uv`)
Avoid `pip install --break-system-packages`. Always isolate:
```bash
# Install uv if missing
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create and populate isolated environment
uv venv --python 3.12
source .venv/bin/activate       # On Windows: .venv\Scripts\activate
uv pip install -r requirements.txt
```

### Safe Configuration Handling
- Copy example configuration files: `cp .env.example .env` or `cp config.example.toml config.toml`.
- Provide exact line references for required keys.
- **Never allow users to post sensitive API keys or passwords directly into the conversation.** Show the exact syntax and let them save the file locally.
- Suggest local offline model runtimes (e.g., Ollama) whenever viable to preserve privacy and eliminate API token costs.

---

## 7. Troubleshooting Guide

When an installation fails, diagnose the root cause using these 10 standard resolutions:

### 1. `command not found` Immediately After Install
- **Resolution:** Restart the terminal instance.
- On macOS Apple Silicon: Verify Homebrew path in `~/.zshrc`:
  ```bash
  export PATH="/opt/homebrew/bin:$PATH"
  ```
- On Windows: Refresh environment variables or launch a new PowerShell terminal.

### 2. Port Already in Use (`EADDRINUSE` / Port Conflict)
- **Resolution:** Remap host port (e.g., change `-p 8080:8080` to `-p 8090:8080`), or terminate conflicting PID:
  - macOS/Linux: `lsof -i :<port>` then `kill -9 <PID>`
  - Windows: `netstat -ano | findstr :<port>` then `taskkill /PID <PID> /F`

### 3. Docker Daemon Unreachable
- **Resolution:** Check if Docker Desktop is launched.
- On Linux: Ensure service is started (`sudo systemctl start docker`) and user is in the docker group:
  ```bash
  sudo usermod -aG docker $USER
  ```

### 4. Python Version Mismatch or Wheel Build Failure
- **Resolution:** Avoid wrestling with system Python. Use `uv` to target exact Python versions:
  ```bash
  uv venv --python 3.12
  ```

### 5. Node Version Too Old
- **Resolution:** Upgrade to Node active LTS:
  ```bash
  nvm install 22 && nvm use 22
  ```

### 6. Permission Denied Errors
- **Resolution:** Never run `sudo npm install -g`. Fix ownership of user directories or use user-space package managers (`scoop` on Windows, Homebrew on macOS, user-local directories on Linux). On binaries, run `chmod +x <binary>`.

### 7. macOS Gatekeeper / Windows SmartScreen Blocks
- **Resolution:** Unsigned open-source builds trigger OS security warnings:
  - macOS: Right-click the app → select **Open**, or go to **System Settings > Privacy & Security > Open Anyway**.
  - Windows: Click **More Info** → select **Run anyway**.
  *Only guide users through this for repositories you have personally vetted in the current session.*

### 8. Git Clone Connection Failure
- **Resolution:** If SSH keys are missing (`Permission denied (publickey)`), rewrite URL to HTTPS. For massive repositories, use shallow clones:
  ```bash
  git clone --depth 1 <https-url>
  ```

### 9. Application Crashes on Startup
- **Resolution:** 90% of crashes are configuration mistakes:
  - Check `.env` file exists and was not left as `.env.example`.
  - Check for trailing quotes or whitespace in API keys.
  - Review live logs: `docker logs <container-name>` or inspect console output.

### 10. Half-Finished or Corrupted State
- **Resolution:** Clean existing wreckage before retrying to avoid debugging phantom issues:
  - Docker: `docker rm -f <container-name>`
  - Python: Delete `.venv` directory and recreate.
  - Node: Delete `node_modules` and package lock files.

### Strategic Pivot Rule
If two consecutive attempts fail for distinct technical reasons, **stop grinding**. Proactively suggest an alternative route (e.g., switch from native build to Docker, or fall back to Candidate #2 from Phase 4).

---

## 8. Trending Radar & Reporting Engine (Mode 3)

### Fetching Spans Without an Official API
GitHub does not offer a dedicated trending API. Approximate trending activity using multi-span search proxies:

```bash
TODAY=$(date +%F)
W=$(date -v-7d +%F 2>/dev/null || date -d '7 days ago' +%F)
M=$(date -v-30d +%F 2>/dev/null || date -d '30 days ago' +%F)
D2=$(date -v-2d +%F 2>/dev/null || date -d '2 days ago' +%F)

# 1. Active high-star repos with recent pushes (Daily proxy)
gh api "search/repositories?q=stars:>1000+pushed:>$W&sort=stars&order=desc&per_page=30" \
  --jq '.items[] | [.full_name,.language,.stargazers_count,.description,.created_at[:10]] | @tsv'

# 2. Rising repos created within the last 7 days (Weekly proxy)
gh api "search/repositories?q=created:>$W&sort=stars&order=desc&per_page=30" \
  --jq '.items[] | [.full_name,.language,.stargazers_count,.description,.created_at[:10]] | @tsv'

# 3. Fast-growing repos created within the last 30 days (Monthly proxy)
gh api "search/repositories?q=created:>$M&sort=stars&order=desc&per_page=30" \
  --jq '.items[] | [.full_name,.language,.stargazers_count,.description,.created_at[:10]] | @tsv'

# 4. Megastar projects updated today
gh api "search/repositories?q=stars:>5000+pushed:>$TODAY&sort=updated&order=desc&per_page=25" \
  --jq '.items[] | [.full_name,.language,.stargazers_count,.description,.created_at[:10]] | @tsv'
```

### Categorization System
- **Taxonomy Structure:** 5–8 Macro categories × 3–7 Subcategories.
- **Explicit Labeling:** Avoid arbitrary keyword regex filters; map projects directly into cohesive domains.
- **Zero "Other" Dumping:** No generic trash buckets. Every project must be given a descriptive cluster.

#### Baseline Macro Categories:
1. **AI-Native Dev Stack:** Coding agents, CLI tools, prompt frameworks, agent memory, MCP servers, observability.
2. **Models & Generation:** Open-weights LLMs, image/video generation, local inference servers.
3. **Security & Privacy:** Vulnerability scanners, threat modeling, OSINT tools, privacy utilities.
4. **Productivity & Engineering:** Desktop managers, automation engines, terminal tools, SaaS alternatives.
5. **Knowledge & Infrastructure:** Databases, RAG architectures, knowledge graphs, documentation systems.

### Output Structure
Always output in clean Markdown with hyperlinked titles: `[owner/repo](https://github.com/owner/repo)`. Include:
1. **Full Hot List:** Inventory tables with stars, language, and velocity indicators.
2. **Taxonomy Breakdown:** Grouped by category and subcategory.
3. **Strategic Insights:** 3–5 macro industry movements observed across the dataset, citing specific repos.

---

## 9. Starter Catalog & Benchmark Examples

When users request a demonstration of capabilities without a specific target, draw from these benchmark projects (always re-verify live data during the session):

### 1. PasteBar — Clipboard History & Snippets
- **Repository:** `github.com/PasteBar/PasteBarApp`
- **Target OS:** Windows, macOS
- **License:** CC BY-NC (Free personal use, non-commercial)
- **Role:** Direct response to *"I keep losing things I copy."*
- **Quick Install (Windows):** `winget install -e --id PasteBar.PasteBar`

### 2. OpenManus — Self-Hosted Autonomous AI Agent
- **Repository:** `github.com/FoundationAgents/OpenManus`
- **Target OS:** Cross-platform (CLI)
- **License:** MIT
- **Role:** Free alternative to autonomous web/coding agents (e.g. Manus).
- **Quick Install:**
  ```bash
  git clone https://github.com/FoundationAgents/OpenManus.git && cd OpenManus
  uv venv --python 3.12 && source .venv/bin/activate
  uv pip install -r requirements.txt
  cp config/config.example.toml config/config.toml
  python main.py
  ```

### 3. n8n — Self-Hosted Workflow Automation
- **Repository:** `github.com/n8n-io/n8n`
- **Target OS:** Cross-platform (Docker)
- **License:** Sustainable Use License (Fair-code)
- **Role:** Powerful self-hosted alternative to Zapier / Make.
- **Quick Install:**
  ```bash
  docker volume create n8n_data
  docker run -d --name n8n --restart unless-stopped \
    -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
  ```

### 4. Understand-Anything — Codebase Knowledge Graph
- **Repository:** `github.com/Egonex-AI/Understand-Anything`
- **Target OS:** macOS, Linux, Windows
- **License:** MIT
- **Role:** Indexes codebases into semantic knowledge graphs for coding agents.
- **Quick Install (Claude Code / Plugin):**
  ```bash
  /plugin marketplace add Egonex-AI/Understand-Anything
  /plugin install understand-anything
  ```

### 5. Cognee — Long-Term Memory for AI Agents
- **Repository:** `github.com/topoteretes/cognee`
- **Target OS:** Cross-platform
- **License:** Apache-2.0
- **Role:** Persistent memory and graph storage across LLM sessions.
- **Quick Install:**
  ```bash
  uv pip install cognee
  ```

### 6. Hyperframes — Programmatic Code-to-Video Engine
- **Repository:** `github.com/heygen-com/hyperframes`
- **Target OS:** Cross-platform (Node 22+ & FFmpeg)
- **License:** Apache-2.0
- **Role:** Deterministic HTML/CSS animation engine rendered to MP4.
- **Quick Install:**
  ```bash
  npx hyperframes init my-video && cd my-video
  npx hyperframes preview
  ```

---

## 10. Honesty Rules & Code of Conduct

1. **Never Hallucinate Repositories:** If an exact repository cannot be verified with a live URL, do not invent one. A single broken link destroys user trust.
2. **No Cached Metrics from Memory:** Star numbers, latest release tags, and dates must be fetched or explicitly stated as approximate.
3. **Admit When Open-Source Isn't the Answer:** If available open-source tools in a niche are immature or unreliable compared to commercial options, state this honestly.
4. **Surface Constraints Before Installation:** Never let a user install a project before warning them about required API subscriptions, heavy VRAM needs, or license restrictions.
5. **Stars Do Not Equal Health:** A 30k-star repository that has been abandoned for two years is still an abandoned repository. Always verify current activity.
6. **Prioritize User Agency:** Never execute commands on a user's machine without explicit, informed confirmation.
