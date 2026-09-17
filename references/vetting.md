# Vetting a repo before you recommend it

Avoid two specific failures: recommending something **dead**, and recommending something
**unsafe**. Both waste far more of the user's time than two minutes of checking costs you.

## The fast pass — six checks

WebFetch the repo page and README; that's usually enough for all six.

**1. Is it alive?**
A commit within the last ~6 months is healthy. 6–12 months can be fine for a small, finished,
stable utility (a clipboard tool has nothing left to break) but is suspicious for anything touching
AI APIs, browsers, or OS internals — those rot from the outside. Past 12 months with unanswered
issues stacking up usually means the install fails on a modern system.

**2. Do the stars make sense?**
Not "enough" — *real*. Thousands of stars with a handful of forks, no issues, three commits =
inflated. Healthy projects have discussion: issues opened **and** closed, PRs merged, releases tagged.

**3. What's the license, really?**
Read the field, don't assume:
- **Permissive (MIT, Apache-2.0, BSD)** — do anything, including commercial.
- **Copyleft (GPL, AGPL)** — free to use; matters if redistributing or building a product on top.
  AGPL specifically affects SaaS use.
- **Fair-code / source-available (n8n SUL, BSL, Elastic)** — free to self-host for own use,
  restricted for reselling. Fine for individuals; mention if they said "for my company."
- **Non-commercial (CC BY-NC — PasteBar)** — personal use free, business use needs permission.

State the category in plain words once. Don't lecture.

**4. Is there a build for their OS?**
Check the Releases page, not README claims. "Cross-platform" sometimes means "compiles on Linux if
you're patient." Look for real `.exe`/`.msi`, `.dmg`, `.AppImage`/`.deb` assets or a published package.

**5. How hard is it really to install?**
Classify honestly from the README's install section:
- Installer / package manager / single binary → anyone
- Docker one-liner → anyone who can install Docker
- `npx` / `pip install` → comfortable-with-terminal
- Clone + venv + edit config + supply API key → **that's a codebase, not a product. Say so.**

**6. Does it need anything expensive or unobvious?**
LLM API key. GPU VRAM. Separate database. Node 22+. Python 3.12 exactly. A paid account somewhere.
Surface every one in the recommendation, before install.

## README summary (for Mode 4 and top picks)

Fetch the README (`gh api repos/<owner>/<repo>/readme --jq .content | base64 -d`, or WebFetch the
repo page) and summarize in the user's language: what it actually does, what it needs, what the
install really is, and the first thing a user sees when it runs. Three to six lines. The README
often reveals the project is an early prototype — that's the point of reading it.

## Security smell tests

Flag — don't silently refuse — when you see:

- Install instructions **piping a script from a non-official domain** into a shell. `curl | bash`
  from the project's own raw URL or documented site is standard; from a random shortener, it isn't.
- **A single, very new, anonymous maintainer** on a project asking for broad credentials.
- **Credential requests it has no reason to need** — a clipboard tool wanting cloud tokens.
- **No issues, no discussions, no forks, but heavy promotion.** Real usage leaves traces.
- **Obfuscated/minified code in the repo**, or binaries committed without a build pipeline.
- **A name shadowing a well-known project** in a different org — typosquatted namespaces are common.
  Always confirm you're at the canonical repo.

Tell the user what you noticed and let them decide. They may have context you don't.

## Choosing between finalists

Prefer the one that is: easier to install **for this specific person** (a slightly worse tool they
can run beats a better one they abandon at step 3); better documented; backed by more than one
person; reversible (Docker and portable apps uninstall cleanly). Say which you'd pick and why —
a ranked recommendation with a reason beats a neutral table.

---

## Deep-dive: open the repo safely (Mode 5, and vetting anything you'll trust deeply)

When implementation details, version-specific behavior, or history matter — or when a repo will
run with real credentials — reading the README isn't enough. Inspect the source, safely:

**Cache, never scatter.** Clone into a central read-only cache, not into the current project:

```bash
git clone --filter=blob:none <url> ~/.repos/<host>/<owner>/<repo>   # strip trailing .git
# --depth=1 only when history and older refs are genuinely unnecessary
```

**Before reusing a cached clone, verify its identity and state:**

```bash
git -C <cache-path> status --short --branch
git -C <cache-path> remote -v
```

A dirty clone: inspect read-only or use an isolated worktree. **Never** reset, clean, pull,
check out, or overwrite it without explicit authorization. Update a clean clone with
`git -C <cache-path> fetch --prune --tags` when fresh refs are needed.

**Pin what you inspected.** Fetch and inspect the named branch, tag, commit, release, or dependency
version; resolve exact refs from local evidence; record the actual commit:

```bash
git -C <cache-path> rev-parse HEAD
```

**Experiments get a worktree, never the cache:**

```bash
git -C <cache-path> worktree add --detach ~/.repos/.worktrees/<name> <ref>
```

Builds, edits, generated files, risky checkouts happen there. Remove only worktrees you created
for this task when done; cached clones are retained (delete only on requested cleanup, after
checking state, size, age).

**Inspect with focused reads.** Targeted search and file/history reads; delegate independent
exploration when useful (fast config for routine scans, stronger config for complex logic).
Request concise findings with paths, refs, and supporting evidence.

**Report honestly:** remote URL, cache path, inspected commit, relevant files/symbols — and
distinguish direct source observations from inference.
