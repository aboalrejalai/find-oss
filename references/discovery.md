# Discovering the right open-source project

Two jobs: find candidates for a **need** (Mode 1/2), and sweep what's **hot right now** (Mode 3
uses `reports.md`, but the queries here feed it). Search live, always.

## Which tool to reach for

1. **`gh` CLI** — best rate limit (30 searches/min, uses the user's auth), cleanest output.
2. **REST search API via `curl`** — works with no token at 10 searches/min.
3. **Web search** — always works, and genuinely better for "what do people recommend as an
   alternative to X" where community judgment beats metadata.

Use more than one. The API gives metadata; the web gives judgment.

## `gh` CLI

```bash
gh search repos "clipboard manager" --stars=">500" --sort=stars --limit=10

gh search repos --topic=ai-agent --language=python --stars=">1000" \
  --updated=">2026-01-01" --sort=stars --limit=20 \
  --json fullName,stargazersCount,description,updatedAt,license
```

Gotchas: quote values containing `>` / `<` (`--stars=">500"`); qualifiers with no flag go after
`--` (`gh search repos "x" -- -topic:linux`). Useful flags: `--topic --language --license --stars
--updated --created --archived=false --limit --json`.

## REST API with curl

```bash
curl -s -H "Accept: application/vnd.github+json" \
  "https://api.github.com/search/repositories?q=clipboard+manager+stars:%3E500&sort=stars&order=desc&per_page=10" \
  | jq -r '.items[] | "\(.full_name)\t\(.stargazers_count)★\t\(.license.spdx_id // "no-license")\tpushed \(.pushed_at[:10])\t\(.description)"'
```

URL-encode `>` as `%3E`, `<` as `%3C`; spaces become `+`. Unauthenticated: 10 req/min, 1000 results
per query — make each query count. If `search/*` is blocked in the environment, fall back to web
search with `site:github.com`, then fetch the specific repo pages.

## Qualifiers that actually matter

| Qualifier | Why |
|---|---|
| `stars:>500` | Popularity floor. Use `>100` for niche needs — good small-niche tools never hit 1k. |
| `pushed:>YYYY-MM-DD` | **The most important filter.** Living projects vs archives. |
| `archived:false` | Excludes explicitly retired repos. |
| `license:mit` / `apache-2.0` | Only when commercial use matters to the user. |
| `topic:` | Higher signal than free text — maintainers tag deliberately. |
| `language:` | Proxy for install method: python → venv, typescript → npm/npx, rust/go → single binary. |
| `in:readme` | Widens a narrow keyword search that returned too little. |

## Search vocabulary — the part people get wrong

The person asking and the person maintaining rarely use the same words for the same thing: one
says "save stuff I copy," the other tags it `clipboard-history`. Run every need through both
dialects.

| The user says | Also search |
|---|---|
| "save things I copy" | clipboard manager, clipboard history, snippet manager |
| "AI agent like Manus" | autonomous agent, general AI agent, computer use agent, agent framework |
| "automate between apps" | workflow automation, zapier alternative, low-code automation, n8n |
| "chat with my documents" | RAG, document QA, knowledge base, local LLM chat |
| "notes app" | personal knowledge management, PKM, second brain, markdown notes |
| "self-hosted Google Photos" | photo management, self-hosted photos, media library |
| "AI memory for my agent" | agent memory, long-term memory, knowledge graph memory |

## Alternative mapping (Mode 2 core)

For "free/open alternative to <paid app>", run all four:

1. `gh search repos "<app> alternative"` and `"open source <app>"` and `"self-hosted <app>"`
2. Web search `best open source <app> alternative 2026` — comparison articles are curated already
3. `awesome-*` lists (below) — self-hosted lists index most paid-SaaS replacements
4. The topic page of the category (`github.com/topics/password-manager`, `…/crm`, `…/helpdesk`)

Then vet normally. Beware fair-code: several famous "open-source alternatives" (n8n, PasteBar,
Cal.com…) are source-available with commercial restrictions — disclose, don't discard.

## Discovery channels beyond raw search

- **`awesome-*` lists** — `gh search repos "awesome self-hosted"`, `"awesome ai agents"`,
  `"awesome windows apps"`. Human-curated; annotations say what each project is actually for.
  `awesome-selfhosted` is the best single index for anything server-side.
- **GitHub topic pages** — `github.com/topics/<topic>`, and topic search:
  `gh search repos --topic=<topic>`. Topics are also a good *trending* channel: recently-pushed
  repos inside a hot topic beat a global star sort for "what's moving in this field".
- **README of the closest project you already found** — good projects link peers and predecessors
  in comparison tables. Free snowball.
- **Web search "best open source X 2026"** — surfaces what people run day-to-day, which stars miss.

## Trending: there is no official API

GitHub exposes **no trending API**. `github.com/trending` must be scraped, or approximated with
search. Both are legitimate; search is more stable.

**Approximate with search (preferred):**

```bash
# hot right now: high-star repos pushed in the last 7 days
gh api "search/repositories?q=stars:>1000+pushed:>$(date -v-7d +%F 2>/dev/null || date -d '7 days ago' +%F)&sort=stars&order=desc&per_page=30" \
  --jq '.items[] | [.full_name, .language, .stargazers_count, .created_at[:10]] | @tsv'

# brand new and rising: created in last 7 days, sorted by stars
gh api "search/repositories?q=created:>$(date -v-7d +%F 2>/dev/null || date -d '7 days ago' +%F)&sort=stars&order=desc&per_page=30" \
  --jq '.items[] | [.full_name, .language, .stargazers_count, .description] | @tsv'
```

**Scrape fallback** (when `gh`/search is unavailable): fetch `https://github.com/trending` (optionally
`/trending/<language>?since=daily|weekly|monthly`) with a normal User-Agent and parse
`article.Box-row` blocks: repo link in `h2 a[href]`, description in `p.col-9`, language in
`[itemprop="programmingLanguage"]`, stars in `a[href$="/stargazers"]`, period delta in the
`.float-sm-right` text ("123 stars today"). Selectors break when GitHub redesigns — if parsing
returns nothing, say so and switch to the search approximation instead of guessing. Cache
aggressively; be polite.

## Reading results fast

For each candidate, six fields before deciding anything: name, stars, license, last push date,
primary language, one-line description. Present compactly (table or tight block) so the user can
scan, not read.
