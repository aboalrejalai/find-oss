# Installing what you recommended

An install that matches the person beats an install that impresses them. Optimize for two things:
it works today, and they can undo it tomorrow.

## Match method to project type

| What you're installing | Use | Reasoning |
|---|---|---|
| Desktop GUI app with published releases | Official installer, `winget`, `brew --cask`, `.AppImage` | No terminal involved; the package manager owns updates |
| Anything that runs as a service (automation servers, media libraries, dashboards, DBs) | Docker | State and dependencies stay inside a container; removal is one command |
| Python project, especially AI/ML | `uv` virtualenv or the project's documented flow | System Python stays untouched; `uv` resolves the interpreter itself |
| Node CLI | `npx <pkg>` first, `npm i -g` after they commit | `npx` leaves zero residue if they bounce |
| Rust / Go tool | Prebuilt release binary, else `cargo install` / `go install` | One static file — the cheapest install class there is |
| Plugin for an editor or coding agent | The host's own plugin/marketplace command | Paths and updates handled by the host |
| Browser extension | Store listing when it exists; unpacked load only for terminal-comfortable users | |

Rule of thumb when torn: **Docker for services, native for windows.**

## Verify prerequisites before the first command

A failure at step four of six costs more trust than a minute of checking:

```bash
docker --version && docker info   # installed AND the daemon is up
python3 --version                 # exact — some projects pin 3.12
node --version                    # many modern tools want 20+/22+
git --version
uv --version
```

Windows: `docker --version`, `python --version`, `node --version`, `winget --version`.
Missing prerequisite? Install that first and explain why. And weigh proportion: a 600MB runtime
for a 5MB utility is a bad trade — say so and reconsider.

## Package managers per OS

**Windows** — `winget install -e --id <Publisher.App>` for GUI apps (built into 11 / modern 10);
`scoop install <app>` for CLI tools and machines without admin; `choco install <app>` when only
Chocolatey has it (needs admin).

**macOS** — `brew install <formula>` for CLI, `brew install --cask <app>` for GUI. No Homebrew?
Install it first; on Apple Silicon add `/opt/homebrew/bin` to PATH if the installer asks.

**Linux** — distro package manager when packaged; otherwise `.AppImage` (chmod +x, run — nothing
installed), Flatpak, or the project's script.

## Docker patterns

```bash
# disposable trial
docker run -it --rm -p 8080:8080 <image>

# keep-it install: named volume so data outlives the container
docker volume create appdata
docker run -d --name myapp --restart unless-stopped \
  -p 8080:8080 -v appdata:/data <image>

# multi-service projects
docker compose up -d
```

Translate the port mapping into human words ("open http://localhost:8080"), and always hand over
the undo: `docker stop myapp && docker rm myapp`. People try things willingly when exit doors are
visible. Port taken? Change the host side: `-p 8090:8080`.

## Python patterns

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh      # macOS/Linux; PowerShell script on Windows
uv venv --python 3.12
source .venv/bin/activate                             # Windows: .venv\Scripts\activate
uv pip install -r requirements.txt
```

Never install a project's requirements into system Python. On Debian/Ubuntu the
"externally-managed-environment" wall is pip protecting the OS — the answer is a virtualenv, never
`--break-system-packages`.

## Configuration is where people quit

Usually it's a sample file renamed and one secret filled in:

```bash
cp .env.example .env        # or config.example.toml → config.toml
```

Point at the exact file and line. **Never let them paste a real secret into the chat** — show the
shape (`OPENAI_API_KEY=sk-...`), they type the value. If the tool can run on a local model
(Ollama), offer it: it deletes the cost and privacy question in one move. Set ports, storage paths,
and language now, not after they hit bad defaults.

## Prove it runs, then teach one move

Verify out loud: open the URL, run `<tool> --version`, check `docker ps`, see the window. Describe
what success looks like so they can confirm independently. Then hand over **one concrete first
action** doable in two minutes — the "oh, nice" moment, not a feature tour. Close with the two
one-liners they'll ask for next week: how to update, how to uninstall.

## When you can't touch their machine

Numbered steps, copy-paste command blocks, and per step one line describing what success looks
like. Offer to go step-by-step instead of dumping all six at once — small increments surface the
failure at the step it actually happened. Ask them to paste error output verbatim when something
breaks.
