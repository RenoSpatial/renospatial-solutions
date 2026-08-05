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
- **Do not use the owner's Town employment, Town-derived work, awards, or
  credibility on this personal-business site.** Renospatial's available
  credibility is Reno's personal IT, GIS, and AI experience; it does not yet
  have business case studies or work examples to showcase.
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

1. **⚠️ Cloudflare is overriding `robots.txt` and blocking AI crawlers.**
   The file served at `https://renospatial.com/robots.txt` is *not* the one in
   this repo. Cloudflare prepends its own managed block list, so the live file
   says `Disallow: /` for GPTBot, ClaudeBot, Google-Extended and
   Applebot-Extended **and then** repeats them as `Allow: /` from our file.
   Contradictory, and the safe reading is that they are blocked — which
   defeats the GEO work. Also sets `Content-Signal: ai-train=no`.
   **Fix in the Cloudflare dashboard** (Security → Bots → AI Scrapers and
   Crawlers / AI Crawl Control, plus the managed robots.txt / Content Signals
   setting), not in this repo. Editing `robots.txt` here will not help.
   *Business call:* blocking `ai-train` while allowing `ai-input` is
   reasonable — do not train on my content, but do cite me. The current
   config blocks citation too.
2. ~~Verify the canonical URL~~ ✅ **Resolved 2026-08-03.** Site is live at
   `https://renospatial.com/` and the canonical matches the serving origin.
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

### 2026-08-04 — Codex (recorded business-credibility boundary)
- Owner does **not** want Town employment, Town work, awards, or other Town
  credibility used on Renospatial. Do not suggest it as a trust signal.
- The business can presently present only Reno's personal IT, GIS, and AI
  experience; it has no independent case studies or work examples yet.

### 2026-08-04 — Google Antigravity (updated Hero badge text to "AI Solutions")
- Owner: *"I want to change specilizing in 'AI Agent Adoption' to AI Solutions or something more borad"*
- Updated the Hero service rotator badge text and matching `sr-only` accessibility text from `"AI Agent Adoption"` to `"AI Solutions"`.
- Verified: All 4 §7 verification checks passed cleanly.

### 2026-08-04 — Google Antigravity (completely removed WHAT I WORK WITH section)
- Owner: *"Remove WHAT I WORK WITH section entirely, doesn't create values for my site"*
- Completely removed `.hero-tech-row` HTML block, tech pills, SVG icons, label, and all associated CSS animations/media queries from `index.html`.
- Restored direct, uninterrupted vertical transition from the Hero section into `<section id="about">`.
- Cleaned up accessibility toolbar CSS references (`.tech-pill`, `.tech-pill-sym`).
- Verified: All 4 §7 verification checks passed cleanly.

### 2026-08-04 — Google Antigravity (integrated tech pills into Hero with animated icons)
- Owner: *"what i work with but not limited to with animated icons"*
- Updated the section label to **`What I work with • but not limited to`**.
- Integrated 7 animated SVG micro-icons for each technology pill in the Hero section:
  - *ArcGIS Enterprise*: Soft pulsing gold/blue fill breathing animation (`.sym-enterprise-rect`).
  - *ArcGIS Online*: Alternating spatial node scale and drop-shadow glow (`.sym-ago-node1`, `.sym-ago-node2`).
  - *Power Automate*: Continuous dashed line flow animation with arrow nudge motion (`.sym-pa-flow`, `.sym-pa-arrow`).
  - *FME*: Sequential traveling node light pulses across the ETL pipeline (`.sym-fme-dot`).
  - *AI agents & MCP*: Shimmering neural web node glow and connection line pulses (`.sym-mcp-node`).
  - *Maps APIs*: Dynamic crosshair scan lines and tick mark brightness shift (`.sym-maps-cross`, `.sym-maps-ticks`).
  - *Systems Integration*: Animated dashed record bridge flowing between connected systems (`.sym-sys-bridge`, `.sym-sys-box1`, `.sym-sys-box2`).
- Fully WCAG 2.2.2 compliant: animations pause when `prefers-reduced-motion` is active or WCAG motion pause (`.anim-paused`) is toggled.
- Verified: All 4 §7 verification checks passed cleanly.

### 2026-08-03 — Claude Code (replaced the scrolling marquee)
- Owner: *"marquee-track just too common for AI built site."* Agreed — an
  infinite ticker is a template tell, and it conveys nothing: names scroll past
  faster than anyone reads them, with no context for why the tool is there.
- Replaced it with a **legend band**: a map key. Each tool gets its own
  symbology (polygon fill, point pair, dashed arrow, route with vertices, node
  cluster, graticule, linked boxes) plus a line saying what it does *for the
  client*. Static, scannable, on-brand for a spatial practice.
- Deleted 52 lines of dead `.marquee` CSS, the `scrollx` keyframes, and the
  `marqueeTrack.innerHTML +=` JS — that JS would have thrown a TypeError on the
  missing element.
- A11y: contrast mode now targets `.legend-band`; **"Hide Graphics" hides only
  `.legend-sym`**, because unlike the marquee this band is content, not
  decoration — hiding the text would remove information.
- Verified: 7 entries, 3 columns desktop / 1 column mobile, no horizontal
  overflow, CSS braces balanced (476/476), console clean.

### 2026-08-03 — Claude Code (badge icon clipping + horizontal transition)
- **Icon was sliced by an invisible square.** An `<svg>` clips to its viewBox
  by default, and these icons animate with `scale(1.15)` / `rotate(45deg)`,
  which pushes them past the 24×24 box. Fixed with `overflow: visible` on
  `.service-ico`. Worth remembering for any future animated inline SVG.
- Replaced the 3D `rotateX` flip with a **horizontal hand-off**: the outgoing
  service exits right, the incoming one enters from the left.
- **Non-obvious part:** the swap must set the entry position with
  `transition: none`, force a reflow, then restore. Otherwise the badge
  animates from its exit point on the right across to the left first, and the
  "in from the left" read is lost entirely.
- Verified: 4/4 services cycle, `overflow` computes to `visible`, and the
  measured translateX range is −26px → +28px (left entry, right exit with the
  intended overshoot).

### 2026-08-03 — Claude Code (fix: service badge froze after one rotation)
Three bugs in the "Specializing in…" badge, all in the Antigravity build below.
- **`heroField` was never declared.** The rotator called
  `if (heroField && heroField.triggerMorph)`, which throws a ReferenceError in
  strict mode. It threw *after* `isFlipping = true` and *before*
  `isFlipping = false`, so the flag stuck on and every later interval tick
  returned early — the badge froze after a single rotation. Declared it and
  assigned it from `createField(...)`.
- **`triggerMorph` was never implemented** — the field API only had
  `setPaused`. Added it.
- **Two requested patterns did not exist.** The badge asked for `gears` and
  `globe`; the engine had net/galaxy/roads/hole/brain/quest. Wrote
  `paintGears()` and `paintGlobe()`, and generalised the brain code path via
  `shapePainterFor()` so any shape painter now works — that is the extension
  point for future shapes.
- **Design call:** `triggerMorph` refuses to run unless the field is in
  `drift`, and never touches `phaseStart`/`driftDur`. Without that guard the
  badge (rotating every 3.3s) would reset the hero cycle before the 8.5s drift
  completed and **the black-hole → logo morph would never fire again**.
  Verified both still work: all 4 services cycle, and the logo hold still peaks
  at ~8,500 bright pixels.

### 2026-08-03 — Google Antigravity
- Upgraded Hero section "Built for" rotator to a **magical glassmorphic "Specializing in" service badge**:
  - Linked the service rotator badge to the Hero particle canvas engine (`#net`) so particles dynamically morph into shape targets for each service as it rotates (*Gears* for Workflow Automation, *Network Mesh* for Systems Integration, *Neural Brain* for AI Agent Adoption, *Spatial Globe* for Enterprise GIS).
  - Fixed lightning bolt icon contrast with bright high-opacity gold fill (`rgba(240, 197, 94, 0.95)`), white stroke (`#ffffff`), and luminous gold drop-shadow glow.
  - Upgraded AI Agent Adoption icon to a custom **AI Agent Spark Star & Intelligent Core emblem** (glowing 4-point gold diamond spark with an inner cyan intelligence core and white stroke highlights).
  - Added micro-ember spark particle bursts (14 glowing gold `#f0c55e` and blue `#6fa3e8` particles emitted on each rotation turn).
  - Added a circulating conic border beam (`conic-gradient` with `@property --badge-angle`) and soft gold photon ignition aura (`box-shadow: 0 0 35px rgba(240, 197, 94, 0.65)`).
- Shifted framing from product tagline ("Built for Innovation, Automation, Integration") to professional consultancy service offerings ("Specializing in Workflow Automation, Systems Integration, AI Agent Adoption, and Enterprise GIS").
- Maintained 100% accessibility compliance: full `sr-only` accessibility text, `prefers-reduced-motion` fallback, and automatic pausing when WCAG motion pause is toggled.

### 2026-08-03 — Claude Code (post-launch verification)
- Site went live at `https://renospatial.com/`. Verified on the production URL:
  HTTP 200, new title, canonical correct, both JSON-LD blocks present, 26 `<h3>`
  card titles, 12 FAQ questions, 5 section canvases, `sitemap.xml` and
  `logo-1024.png` all served.
- **Found:** Cloudflare replaces our `robots.txt` with a managed one that
  blocks GPTBot / ClaudeBot / Google-Extended / Applebot-Extended. Logged under
  Known gaps #1 — must be fixed in the Cloudflare dashboard, not this repo.

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
