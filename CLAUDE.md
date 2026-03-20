# Ek Nazar — Claude Code Instructions
## FIRST THING EVERY SESSION
- At session start — read CLAUDE.md, CHANGELOG.md and DECISIONS.md before doing anything.
## CRITICAL RULES
- NEVER save files to /tmp/ — that folder gets wiped on restart
- ALWAYS save files to /Users/shankarganesh/ek-nazar-pipeline/
- After every task, confirm which files were written to disk
- Before starting work, run ls -la to check existing files
- ALWAYS sync index.html → the-brief-cms.html before committing (they must be identical)
- ALWAYS validate JS syntax before committing: `node -e` with `new Function()` on script blocks
- ALWAYS use localDateStr() for dates — NEVER use toISOString().split('T')[0] (UTC bug)
## Project
Ek Nazar — Dainik Bhaskar daily news brief
Repo: github.com/shankariy88-kratos/eknazarproject
## Files
- index.html — Editorial CMS (~144KB, v3.9.9) — the source of truth
- DECISIONS.md — Why decisions were made
- the-brief-cms.html — exact copy of index.html, served from GitHub Pages
- ek-nazar-reader.html — Reader app (vertical card swipe + horizontal slot swipe, v4.0.0)
- ek-nazar-landing.html — Landing page
- CHANGELOG.md — Full version history
- package.json — Node.js config
## Stack
Node.js, Claude API (Sonnet), Metabase API, localStorage bundles
## Architecture Notes
- Reader uses 2D swipe: vertical (cards) + horizontal (slots) — like Reels + Stories
- Reader DOM: .slot-track > 3x .slot-panel > .card-track (vertical) per slot
- CMS and Reader share localStorage key `ek_nazar_bundles`
- Metabase feed uses 3-tier CORS proxy (direct → allorigins → corsproxy.io)
- callClaude() is the shared API helper — all AI calls go through it
- getSummaryPrompt()/getSummaryRules() are the single source for prompt text
- sanitizeSummaryHTML() normalizes all summaries on save
- localDateStr() is the only date formatter — never use UTC methods
- Bundle dates come from date picker (editorial date), savedAt is actual timestamp
## Known Patterns
- index.html and the-brief-cms.html must always be identical
- loadAllBundles() runs migrations on load (mediaUrl→imageUrl, feedBody cleanup for old bundles, duplicate date+slot merge)
- Auto-color headline runs on Extract/tag-change only, NOT on blur (user owns styling after prefill)
- One bundle per date+slot — saveBundle() merges, loadAllBundles() deduplicates
- fixFeedTitle() strips leading ":" from Metabase titles
## Never do
- Save to /tmp/
- Start coding without reading existing files first
- End session without confirming files are saved
- Use toISOString() for date comparisons (UTC timezone bug)
- Store feedBody in localStorage for OLD bundles (bloats toward 5MB limit) — today's bundles keep it
