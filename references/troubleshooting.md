# When the install breaks

Almost every failure is one of ten familiar ones. Read the error text first, guess second, and
translate the diagnosis into plain language — "something else is already using that port" lands;
a pasted stack trace doesn't.

## "command not found" right after a clean install

The binary landed but the shell can't see it. Restart the terminal before anything cleverer — it
fixes this surprisingly often. Then:
- macOS/Linux: inspect `echo $PATH`; Apple Silicon Homebrew lives in `/opt/homebrew/bin` — add the
  export to `~/.zshrc` / `~/.bashrc` and `source` it.
- Windows: installers update PATH for *new* terminals only — open a fresh PowerShell; inspect with
  `$env:PATH -split ';'`.
- npm globals: `npm config get prefix`, then confirm `<prefix>/bin` is on PATH.

## Port already in use

`EADDRINUSE` / "port is already allocated": another process owns it. Either move your host-side
mapping (`-p 5679:5678`) or evict the squatter — macOS/Linux: `lsof -i :5678`; Windows:
`netstat -ano | findstr :5678` then `taskkill /PID <pid> /F`.

## Docker daemon unreachable

Installed ≠ running. Start Docker Desktop and wait for it to settle; on Linux
`sudo systemctl start docker`. Socket "permission denied" on Linux means the user isn't in the
`docker` group: `sudo usermod -aG docker $USER`, then log out and back in.

## Python version mismatch

"requires Python >=3.12", wheels failing to build: don't wrestle the system interpreter — let `uv`
supply the right one (`uv venv --python 3.12`). The Debian/Ubuntu externally-managed error is pip
guarding the OS; a virtualenv is the fix, `--break-system-packages` is the trap. Venv "ignored"?
Check the prompt shows `(.venv)` and `which python` points inside it.

## Node too old

Syntax errors from `npx` or postinstall scripts usually mean ancient Node. Current LTS via
`nvm install 22 && nvm use 22`, `fnm`, or the system package manager. Confirm: `node --version`.

## Permission denied

Prefer user-level installs over `sudo` — `sudo npm i -g` in particular seeds root-owned files that
poison later updates. Windows: some installers demand an admin PowerShell; scoop deliberately
doesn't, which is why it exists for non-admin machines. Downloaded binary inert on macOS/Linux:
`chmod +x <file>`.

## macOS "damaged" / unidentified developer, Windows SmartScreen

Gatekeeper and SmartScreen blocking unsigned builds is *normal* for small open-source projects.
macOS: right-click → Open, or Privacy & Security → "Open Anyway". Windows: More Info → Run anyway.
Walk someone through this **only** for a repo you vetted yourself this session.

## Clone failures

`Permission denied (publickey)` = SSH URL without keys configured; switch to the HTTPS URL.
Enormous or glacial clone: `git clone --depth 1 <url>` takes just the latest snapshot.

## Starts, then dies or errors on first action

Configuration, nine times out of ten:
- Secret missing, or saved with stray quotes/trailing spaces — open the actual `.env`/config and look.
- Key valid but the account lacks the model named in the config.
- `.env.example` copied but never renamed to `.env`.
- Read the logs: `docker logs <name>`, or the terminal it runs in.

## Half-finished install

Clean the wreckage before retrying, or you'll debug corrupted state: Docker → `docker rm -f <name>`;
Python → delete `.venv` and rebuild; Node → delete `node_modules` + lockfile; cloned repo → delete
the folder, clone fresh.

## Know when to change roads

Two attempts at the same path failing for *different* reasons = stop grinding, switch strategy:
Docker instead of native, prebuilt binary instead of building from source, or the second-ranked
candidate from your recommendation. Announce the switch and the reason. An hour buried in a
stubborn install costs more than a slightly different tool that runs in five minutes.
