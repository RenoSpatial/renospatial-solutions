# PROJECT_MEMORY.md — state, decisions, and what not to redo

Shared memory for every agent working on this repo. Rules live in
**[AGENTS.md](AGENTS.md)**; *history and judgement* live here.

**Update the Changelog at the bottom whenever you finish a piece of work.**
None of us share a session — this file is the only continuity we have.

---

## Owner

Reno Sun — Renospatial Solutions, Parksville, British Columbia.
Contact on site: `rsun@renospatial.com` · LinkedIn: `linkedin.com/company/renospatial`

Background stated on the site: a decade of GIS and IT management. Runs
Enterprise GIS, Microsoft Power Platform, FME, Python and AI tooling. Works
vendor-neutrally and prefers activating software the client already pays for.

## Current state

Single-page site, ~4,100 lines in `index.html`, ~2,300 words of copy.

**Sections:** Hero → About (3 pillars + "How I work" principles) → Use Cases
(8 cards) → AI Adoption (6 cards) → FAQ (12 questions) → Contact.

**Working:** 6 particle animations (one per section), accessibility toolbar with
12 options and persistence, mobile collapse for Use Cases / AI / FAQ, scroll-spy
nav, JSON-LD (ProfessionalService with 13 services and 70 topics, plus FAQPage
with 12 questions), `robots.txt`, `sitemap.xml`.

## Decisions worth remembering

**Content**
- Copy is **first person singular**. The original GoDaddy site was third-person
  press-release voice; that was deliberately replaced.
- Say **"AI agent"**, never "chatbot" — the owner asked for this specifically.
- MCP is presented as *one piece* of AI adoption, alongside **skills** and
  **orchestration**, not as the whole story.
- Mapping/GIS is framed as **optional** and **not Esri-only** — many clients
  never need a map, and open-source or non-Esri tools are offered where they fit.
- Cost is explained in two kinds: non-AI automation can be **fixed price**;
  AI processing is **pay-as-you-go, like a utility bill**. The owner considers
  this an important expectation to set.
- The Innovation / Automation / Integration trio is brand language from the
  original site. Keep the three names.

**Technical**
- One `createField()` factory drives all six canvases (see AGENTS.md §6).
- Section canvases idle until scrolled into view — deliberate, for performance.
- FAQ panels are slightly translucent so the question-mark animation reads
  behind them on mobile, where the accordion is full width.

## Rejected — do not reintroduce

| Thing | What happened |
|---|---|
| **Full "hydrographic chart" redesign** | Cream paper palette, Fraunces + IBM Plex Mono, hanging marginalia, hard rules, flat cards. Built, reviewed, and rejected: *"I don't like this at all. I like what I had before this change."* Fully reverted. **Do not redesign the visual language.** |
| **Google Fonts / any web font** | Came in with the redesign, went out with it. Broke the zero-external-request property. System stack only. |
| **Headlight trails on the traffic animation** | Trails drew across a car's position jump when it wrapped the screen edge or turned, flashing a full-width gold streak. Owner reported it as hard on the eyes. Removed. |
| **Fast/whipping black hole** | An "improved" version spun much faster with long comet tails. Owner preferred the slower, stately Interstellar feel: *"the previous version was better."* Reverted and kept slow. |
| **First-person rewrite of the About pillar cards (first attempt)** | Was reverted at the time, then later rewritten again by Antigravity in a form the owner kept. Current text is fine — leave it. |

## Known gaps / next steps

Ranked by likely value:

1. **Verify the canonical URL** before/at launch. Hardcoded to
   `https://renospatial.com/`. If Cloudflare serves from a `*.pages.dev`
   origin, this is actively harmful. *(Flagged repeatedly, still unresolved.)*
2. **Credibility signals are thin.** No named founder in the copy, no `Person`
   schema, no case studies, no testimonials, no phone number, only one `sameAs`.
   The **MISA BC award** the owner holds is not mentioned anywhere — that is the
   single strongest trust signal available and it is missing.
3. **Case studies.** Two or three short ones ("council was rekeying permits by
   hand, now it is automatic, ~6 hours/week back") would do more for both
   conversion and AI citation than more service copy.
4. Submit to Google Search Console; set up a Google Business Profile (for a
   local service business this often outperforms the site for "near me" queries).
5. `og:image` is a square 1024×1024 logo. A 1200×630 landscape card would render
   better in social previews.

## Honest assessment of SEO/GEO position

On-page work is solid: structured data, ~2,300 words, Q&A format, AI crawlers
allowed. What is missing is **authority** (no backlinks, new page) and **proof**
(no case studies or credentials). Realistic expectation: findable on long-tail
and local queries — *"Power Automate consultant Vancouver Island"* — not on head
terms like *"workflow automation consulting"*. None of it counts until the site
is deployed at the correct canonical URL and submitted for indexing.

---

## Changelog

Newest first. Add an entry when you finish work.

### 2026-08-03 — Claude Code
- Stripped hedge padding from all copy: `genuinely` 5→0, `simply` 3→0,
  `really` 2→0, `actually` 8→1, `honest` 4→2. The three survivors each carry
  real meaning ("Sometimes replacement is the honest answer"; "…can Power
  Automate *actually* automate?"; "I will give you an honest answer").
  Applied to both visible copy and the matching JSON-LD answers.
- **Restored 26 missing `<h3>` headings.** Card titles had become bare
  `<span>`s inside buttons, removing 26 entries from the document outline —
  bad for screen-reader navigation and for the SEO work. Now
  `<h3 class="uc-title"><button class="uc-head">`, the WAI-ARIA accordion
  pattern. Added a `.uc-title` reset so the inner span keeps the styling.
- **Fixed 14 dead buttons on desktop.** `.uc-head` is a real `<button>`, but
  the toggle early-returned above 768px, so keyboard users tabbed onto controls
  that did nothing. Now `disabled` on desktop, with CSS so it still looks like
  plain text.
- Removed a redundant `keydown` handler on those buttons (native buttons
  already fire click on Enter/Space — it was double-toggling) and the redundant
  `role="button"` / `tabindex="0"`.
- Debounced the collapse resize handler (150ms).
- Added `AGENTS.md` and this file so Claude Code and Antigravity share rules
  and state.

### Earlier — Google Antigravity
- Mobile collapse for Use Cases / AI / FAQ cards (`.uc-head` + `.uc-body`,
  `.is-open`), expand/collapse-all control, scroll-spy nav.
- Richer hover animation on cards; upgraded SVG icons (atom core with spark,
  meshing gears with sync line).
- Rewrote the three About pillar paragraphs, removing the original GoDaddy
  press-release wording.
- CSS-based accessibility overrides for display, motion and text.

### Earlier — Claude Code
- Built the page: sections, particle engine, accessibility toolbar, logo
  (SVG + PNG exports), structured data, `robots.txt`, `sitemap.xml`.
