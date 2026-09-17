# Trending radar — categorized open-source reports

The "what is the open-source world shipping right now" mode. Produces a full inventory, a
two-level categorization, and 3–5 strategic insights — in chat as Markdown, optionally as a
self-contained HTML file.

## Step 1 — Fetch the spans

GitHub has no trending API (see `discovery.md`). Pull three spans plus two extras, dedup later:

```bash
TODAY=$(date +%F); W=$(date -v-7d +%F 2>/dev/null || date -d '7 days ago' +%F); M=$(date -v-30d +%F 2>/dev/null || date -d '30 days ago' +%F); D2=$(date -v-2d +%F 2>/dev/null || date -d '2 days ago' +%F)

# daily proxy — active high-star repos pushed this week
gh api "search/repositories?q=stars:>1000+pushed:>$W&sort=stars&order=desc&per_page=30" --jq '.items[] | [.full_name,.language,.stargazers_count,.description,.created_at[:10]] | @tsv'
# weekly — new repos created this week by stars
gh api "search/repositories?q=created:>$W&sort=stars&order=desc&per_page=30" --jq '.items[] | [.full_name,.language,.stargazers_count,.description,.created_at[:10]] | @tsv'
# monthly — new repos created this month by stars
gh api "search/repositories?q=created:>$M&sort=stars&order=desc&per_page=30" --jq '.items[] | [.full_name,.language,.stargazers_count,.description,.created_at[:10]] | @tsv'
# today's hottest — megastar repos pushed today
gh api "search/repositories?q=stars:>5000+pushed:>$TODAY&sort=updated&order=desc&per_page=25" --jq '.items[] | [.full_name,.language,.stargazers_count,.description,.created_at[:10]] | @tsv'
# brand new — created in last 2 days
gh api "search/repositories?q=created:>$D2&sort=stars&order=desc&per_page=20" --jq '.items[] | [.full_name,.language,.stargazers_count,.description,.created_at[:10]] | @tsv'
```

If a scrape of `github.com/trending?since=daily|weekly|monthly` is available and working, merge it
in as a sixth source (it captures momentum the search proxies miss).

## Step 2 — Full inventory & dedup

List **every** project found; omit none. Mark projects appearing in multiple spans
("Daily + Monthly") — repetition across spans is itself a signal worth showing.

## Step 3 — Categorize: two levels, explicit mapping, no catch-all

| Unique projects | Structure |
|---|---|
| < 30 | single level, 5–9 buckets |
| 30–200 | **two levels: 5–8 macros × 3–7 subs** (the usual case) |
| > 200 | three levels, or cluster automatically |

**Use explicit `full_name → (macro, sub)` mapping, not keyword heuristics.** Keyword rules
(`if 'skill' in desc…`) cap out around 75% hit rate, dump the rest into "other", and rot every time
a new genre appears. At ≤200 projects, labeling once by hand is cheaper than maintaining rules
forever, and every boundary decision stays auditable. Unknown stragglers may fall back to a
heuristic, but the mainstream must be explicit.

Starting macros (adjust to what the data actually surfaces):

- **AI-native dev stack** — skills, AI coding CLIs, agent harnesses, LLM platforms, AI workspaces, evals/observability, MCP/SDK
- **Models & creative generation** — frontier/open models, multimodal generation, model hardware
- **Security & defense** — defensive/threat modeling, supply-chain scanning, OSINT, dual-use
- **Engineering & productivity** — desktop/mobile tools, terminal, web, data integration, SaaS replacements
- **Research & learning** — educational resources, awesome-lists, RAG/knowledge systems
- **Platforms & classic infra** — frontend frameworks, systems/kernel, general dev tooling

**Sub-buckets emerge from the data.** Lay all projects out first, then cut along natural clusters
(19 skill projects → 5 subs; 5 skill projects → 1–2 subs). Never pre-fix counts; empty buckets and
forced splits are the tell.

**No "other" catch-all.** If the misc bucket exceeds ~8 projects, force them into named buckets
even if narrow (`suspicious / game exploits`, `no description / unknown`, `game ports`). Naming the
weird makes it visible instead of hidden.

**One record, one home.** Cross-domain projects (a security tool shipped as a skill) go to the
*most specific* macro; imply the secondary nature in the sub label. Never double-count.

Verify coverage before writing: mapped count must equal inventory count; any unmapped name means
the categorization is unfinished.

## Step 4 — Hyperlink everything

Every project carries its full URL: `[owner/repo](https://github.com/owner/repo)`. No exceptions —
a report the reader can't click is a wall of text.

## Step 5 — Strategic insights

3–5 high-level trends across the inventory (what's accelerating, what's new this span, what died),
plus a one-sentence "vibe summary" of the period. Insights must cite projects from the inventory —
no floating opinions.

## Step 6 — Output

**Markdown (default), in chat:**

```
# Open-Source Radar — <YYYY-MM-DD>
## Part 1: Full hot list
### Daily — table (repo · language · stars · created/pushed)
### Weekly — table
### Monthly — table
## Part 2: Categorization
(one table per macro, subs as sections)
## Part 3: Strategic insights
(3–5 insights + vibe summary)
```

**HTML (optional, when asked):** a single self-contained file — clean cards per project, sticky
mini-TOC on the right for reports with ≥6 macro sections (scroll-spy, collapsible subs; avoid a
full-width inline TOC, it eats the first screen). Save next to the working directory and open it
for the user. If they keep notes in a vault or folder, save a copy there only when they say where.

**Language:** mirror the user. Arabic report = Arabic labels and insights, repo names and links
untouched.
