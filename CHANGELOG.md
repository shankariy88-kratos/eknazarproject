# Ek Nazar — Change Log

## v3.9.8 — 20 March 2026
**Added:** Card Preview button (📱 Preview) in History bundles — same preview modal as New Bundle
**Tested:** Preview opens for history bundles, navigation works, slot/tag/image renders correctly
**Status:** Pushed to GitHub ✅

## v3.9.7 — 20 March 2026
**Fixed:** Bold text in manual/pasted summaries now preserved in reader frontend
**Changed:** Bullet button works like Word — paste plain text, click "+ Bullet", all lines get • prefix
**Tested:** AI summary, manual typing, clipboard paste — all produce identical bullet HTML in reader
**Status:** Pushed to GitHub ✅

## v3.9.6 — 20 March 2026
**Added:** sanitizeSummaryHTML() — normalizes all summaries on save to clean bullet structure
**Fixed:** Pasted summaries no longer carry foreign fonts, colors, or spacing into reader
**Tested:** Paste from Word, Chrome, plain text — all render consistently
**Status:** Pushed to GitHub ✅

## v3.9.5 — 20 March 2026
**Fixed:** Auto-migration for bundles saved with wrong dates (stale date picker bug)
**Fixed:** Strips leftover feedBody from old bundles to free localStorage space
**Tested:** Old bundles auto-corrected on page load, console logs corrections
**Status:** Pushed to GitHub ✅

## v3.9.4 — 20 March 2026
**Fixed:** Date/time mismatch in Add to Bundle modal and History — was mixing b.date with b.savedAt
**Changed:** Modal now uses savedAt as single source of truth for display
**Changed:** History shows full "Saved 20 Mar 1:47 am" instead of just time
**Tested:** Bundles with mismatched dates now show correct timestamps
**Status:** Pushed to GitHub ✅

## v3.9.3 — 20 March 2026
**Changed:** Today/Yesterday labels now include actual date — "Today (Thursday, 20 March)"
**Tested:** History view and Add to Bundle modal both show date alongside label
**Status:** Pushed to GitHub ✅

## v3.9.2 — 20 March 2026
**Fixed:** Manual headline color edits were being overwritten by auto-color on every blur
**Changed:** Removed onblur auto-color — now only runs on Extract prefill and tag change
**Tested:** Color toolbar works after auto-color, manual selections preserved
**Status:** Pushed to GitHub ✅

## v3.9.1 — 20 March 2026
**Fixed:** Colon ":" now included in colored headline span (was left as default black)
**Fixed:** Stale date picker — auto-updates when switching to editor tab, warns on save if date is past
**Tested:** Overnight scenario — date picker refreshes correctly after midnight
**Status:** Pushed to GitHub ✅

## v3.9.0 — 20 March 2026
**Fixed:** CRITICAL — Reader getToday() used UTC date, bundles invisible after 6:30 PM IST
**Fixed:** CRITICAL — _modalCallAI silently dropping instruction parameter
**Fixed:** feedBody (10-50KB HTML per story) no longer stored in localStorage bundles
**Fixed:** Date pickers used UTC valueAsDate — now use local date
**Fixed:** Landing page date comparison also fixed (UTC → local)
**Added:** Clipboard .catch() error handling
**Tested:** Reader shows bundles at all hours, modal regen works, localStorage stays lean
**Status:** Pushed to GitHub ✅

## v3.8.9 — 20 March 2026
**Changed:** Code optimization — removed 292 lines of bloat across all files
**Changed:** Created callClaude() shared API helper (eliminated 9 duplicate fetch blocks)
**Changed:** Consolidated summary prompt into getSummaryPrompt()/getSummaryRules()
**Changed:** Merged extractFromUrl + modalExtractFromUrl into extractFromUrlGeneric()
**Changed:** Merged populateExtracted + populateModalExtracted into populateExtractedGeneric()
**Fixed:** Reader — removed redundant SLOT_ICONS_SM alias, cleaned empty updateProgress
**Fixed:** Landing page — fixed broken hintText element reference
**Tested:** JS syntax validated, all features working, index.html 149KB → 136KB
**Status:** Pushed to GitHub ✅

## v3.8.8 — 20 March 2026
**Added:** Auto-color Indian place tags with राज्य-शहर color (#FF554B)
**Added:** ~70 major Indian cities/states in Hindi and English
**Tested:** Tags like इंदौर, भोपाल, Indore get correct color
**Status:** Pushed to GitHub ✅

## v3.8.7 — 20 March 2026
**Fixed:** Custom tag input in modal — "नया Tag जोड़ें" now actually saves the tag
**Tested:** Add custom tag from modal, tag persists in dropdown
**Status:** Pushed to GitHub ✅

## v3.8.6 — 20 March 2026
**Fixed:** Add to New Bundle from Article Feed — story now actually adds to live bundle
**Tested:** Single article add and bulk add both work
**Status:** Pushed to GitHub ✅

## v3.8.5 — 19 March 2026
**Fixed:** Today/Yesterday labels in Add to Bundle modal — use local timezone instead of UTC
**Tested:** Labels correct at all times of day
**Status:** Pushed to GitHub ✅

## v3.8.4 — 19 March 2026
**Added:** Publish date shown in Article Feed list view
**Fixed:** Refresh button in Article Feed now respects date/category/search filters
**Tested:** Filter by date, refresh, results match filter
**Status:** Pushed to GitHub ✅

## v3.8.3 — 19 March 2026
**Changed:** Removed emoticons from New Bundle, History, Article Feed tab headers
**Tested:** Visual check in browser, regression on Extract, Summarise, Bundle Save
**Status:** Pushed to GitHub ✅

## v3.8.2 — 19 March 2026
**Fixed:** Skeleton loading image height to match Figma (270px)
**Status:** Pushed to GitHub ✅

## v3.8.1 — 19 March 2026
**Changed:** Simplified History date filter — single date picker, default today
**Status:** Pushed to GitHub ✅

## v3.8 — 19 March 2026
**Added:** Date filter to History tab
**Status:** Pushed to GitHub ✅

## v3.7 — 19 March 2026
**Added:** Add to existing bundle from Article Feed (modal with bundle list)
**Status:** Pushed to GitHub ✅

## v3.6.1 — 19 March 2026
**Fixed:** Modal manual summary — show summary section on "खुद लिखें" mode
**Status:** Pushed to GitHub ✅

## v3.6 — 19 March 2026
**Added:** Date picker in Article Feed — filter articles by published date
**Status:** Pushed to GitHub ✅

## v3.5 — 19 March 2026
**Added:** Modal parity — feed body, dynamic button label, body reference in edit modal
**Status:** Pushed to GitHub ✅

## v3.4 — 19 March 2026
**Added:** Auto-fill deeplink from Metabase ARTICLE_DEEPLINK column
**Status:** Pushed to GitHub ✅

## v3.3 — 19 March 2026
**Added:** Article Feed sort toggle — Latest first (default) or Oldest first
**Status:** Pushed to GitHub ✅

## v3.2 — 19 March 2026
**Changed:** Dynamic Summarize button label — shows source (Article Body vs URL)
**Status:** Pushed to GitHub ✅

## v3.1 — 19 March 2026
**Fixed:** Metabase CORS — 3-tier fetch with proxy fallbacks (direct, allorigins, corsproxy.io)
**Status:** Pushed to GitHub ✅

## v2.17.4 and earlier — pre-19 March 2026
**Summary:** Initial CMS build — editor, bundle preview, history, reader, landing page, tag colors, Extract from URL, AI Summarize, headline color toolbar, completion card, responsive layout.
**Status:** Pushed to GitHub ✅
