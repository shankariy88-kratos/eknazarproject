# Ek Nazar — Decision Log

## Why no scraping
bhaskar.com blocks bots with Cloudflare. Data comes from Metabase API instead. Article text for AI summarization uses a CORS proxy (allorigins.win) as fallback, but primary flow is Metabase feed body.

## Why /tmp/ is banned
Files in /tmp/ get wiped on Mac restart. Always use /Users/shankarganesh/ek-nazar-pipeline/

## Validation approach
Before every commit: validate JS syntax using `node -e` with `new Function()` on all `<script>` blocks. Cannot use `node --check` on .html files.

## Why index.html and the-brief-cms.html must be identical
index.html is served on localhost:8080 for development. the-brief-cms.html is the same file served from GitHub Pages (shankariy88-kratos.github.io). They must always be synced with `cp index.html the-brief-cms.html` before committing.

## Why localStorage, not a database
This is a demo/POC project. localStorage keeps it zero-infrastructure — no backend, no database, no hosting costs. The trade-off is 5MB storage limit and per-domain isolation (localhost bundles ≠ GitHub Pages bundles).

## Why feedBody is NOT stored in localStorage bundles
Each Metabase article body is 10-50KB of raw HTML. Storing it per story in localStorage would hit the 5MB limit after a few bundles. feedBody is kept in memory only (live `stories` array) for summarization, then stripped on save.

## Why dates use localDateStr(), never toISOString()
`toISOString()` returns UTC date. In IST (UTC+5:30), after 6:30 PM, UTC date is tomorrow. This caused bundles to disappear from the reader after evening. All date comparisons now use `localDateStr()` which returns `YYYY-MM-DD` in local timezone.

## Why auto-color runs on Extract/tag-change only, not on blur
The headline auto-color (text before ":" gets tag color) used to run on every `onblur`. This destroyed manual color edits from the toolbar. Now it only runs on Extract prefill and tag dropdown change — after that, the user owns the styling.

## Why 3-tier CORS proxy for Metabase
Metabase server (`metabase.dbdigital.in`) doesn't have CORS headers for external origins. Direct fetch works on localhost (same network). From GitHub Pages (`shankariy88-kratos.github.io`), the browser blocks the request with:
```
Access-Control-Allow-Origin header missing → CORS error
```

**Original ask to server team (not needed anymore):**
```
Access-Control-Allow-Origin: https://shankariy88-kratos.github.io
Access-Control-Allow-Methods: POST, OPTIONS
Access-Control-Allow-Headers: x-api-key, Content-Type
```

**Instead, we use 3-tier proxy fallback in `fetchMetabaseWithCORS()`:**
- **Tier 1: Direct fetch** — `fetch(metabaseUrl, {headers: {'x-api-key': key}})`. Works on localhost. Fails on GitHub Pages (CORS).
- **Tier 2: allorigins.win** — `fetch('https://api.allorigins.win/post?url=' + encodeURIComponent(metabaseUrl))`. Free public CORS proxy. Relays the POST request. This is what makes GitHub Pages work.
- **Tier 3: corsproxy.io** — `fetch('https://corsproxy.io/?' + encodeURIComponent(metabaseUrl))`. Second fallback if allorigins is down.

**Verified on 20 March 2026:** Opened `the-brief-cms.html` on GitHub Pages in Chrome, clicked Article Feed → Refresh. Tier 1 failed (CORS), Tier 2 (allorigins.win) succeeded. Articles loaded. No server-side changes needed.

**Trade-offs:**
- allorigins.win and corsproxy.io are free public services — they can go down, rate-limit, or shut down
- For production, either add CORS headers on Metabase or deploy a dedicated proxy
- For demo/POC this is sufficient and avoids any dependency on the server team

## Why allorigins.win is also used for article HTML fetching
The same CORS problem applies when fetching Bhaskar article HTML for Extract and AI Summarize. `fetchArticleHtml()` uses allorigins.win (`/get` and `/raw` endpoints) to proxy the article page. If Cloudflare blocks it, the code falls back to Claude's `web_search` tool (Tier 2 for summarize, Tier 2 for extract).

## Why sanitizeSummaryHTML() exists
Manual typing and clipboard paste produce inconsistent HTML — foreign fonts, random spans, `<br>` instead of `<div>`, no bullet markers. `sanitizeSummaryHTML()` normalizes everything on save to clean `<div class="sum-bullet">• text</div>` with only bold/italic preserved. This ensures the reader renders all summaries identically.

## Why the date picker auto-refreshes
If the CMS tab is kept open past midnight, the date picker still shows yesterday. Bundles saved with the stale date end up under the wrong day. `refreshDateIfStale()` runs on tab switch to editor, and `saveBundle()` warns if the date is in the past.

## Why loadAllBundles() runs migrations
Old bundles may have: (1) `mediaUrl`/`mediaType` instead of `imageUrl`/`videoUrl`, (2) wrong `b.date` from stale date picker, (3) leftover `feedBody` bloat. The migration in `loadAllBundles()` fixes all three on every load and persists the fix.

## Why callClaude() is a shared helper
The Anthropic API call pattern (headers, error handling, text extraction) was duplicated 9 times across summarize, extract, regen functions. `callClaude(messages, opts)` centralizes this. All AI calls go through it.
