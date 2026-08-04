# AGENTS.md — working rules for this repository

Canonical instructions for **any** AI coding agent working here (Claude Code,
Google Antigravity, Cursor, Copilot, or a human). Read this before editing.

---

## ⚠️ START HERE — four non-negotiables

1. **Open `PROJECT_MEMORY.md` before you touch anything.** It lists decisions
   already made and — critically — a **"Rejected"** table of things that were
   built and thrown away. Rebuilding one of those is the most common failure
   mode here.
2. **Do not redesign the visual language.** A complete restyle was built and
   rejected by the owner. Work *within* the dark navy + gold system.
3. **Everything is one file** (`index.html`), no build step, **zero external
   runtime requests**. No frameworks, no CDNs, no web fonts.
4. **When you finish, append to the Changelog in `PROJECT_MEMORY.md`** — what
   changed, why, what you left undone. An undocumented change is one the next
   agent will silently undo.

Before saying a task is done, run the verification suite in **§7**. Not one of
its checks is optional.

---

Companion file: **[PROJECT_MEMORY.md](PROJECT_MEMORY.md)** — current state,
decisions already made, and things deliberately rejected. Read that too, and
update it when you finish work.

---

## 1. What this is

A one-page marketing site for **Renospatial Solutions** — a solo consultancy in
Parksville, BC, run by Reno Sun. Services: workflow automation, systems
integration, AI agent adoption, and (optional) GIS/web mapping.

Live domain: `https://renospatial.com` · Repo: `RenoSpatial/renospatial-solutions`
· Branch: `main` · Hosting: Cloudflare Pages (auto-deploys on push to `main`).

## 2. Hard constraints — do not violate

| Rule | Why |
|---|---|
| **Everything lives in `index.html`** — HTML, CSS and JS inline | No build step. The owner edits and deploys directly. Do not split into `/src`, do not add bundlers. |
| **No frameworks, no npm dependencies at runtime** | No React/Tailwind/jQuery. Vanilla JS and hand-written CSS only. |
| **Zero external runtime requests** | No CDN scripts, no Google Fonts, no remote images. The only `https://` strings in the file are schema.org URLs, SVG namespaces, and self-references. Keep it that way — verify with the check in §7. |
| **System font stack only** | Web fonts were tried and rejected (see PROJECT_MEMORY §Rejected). |
| **Never break the accessibility toolbar** | It is a WCAG 2.2.2 obligation, not decoration. |

## 3. File map

```
index.html                 the entire site (~4,100 lines)
logo.svg                   vector master of the logo
logo-{256,512,1024}.png    transparent, for dark backgrounds
logo-light-bg-{512,1024}.png  deeper colours, for white/light backgrounds
robots.txt                 allows AI crawlers (GPTBot, ClaudeBot, PerplexityBot…)
sitemap.xml                single URL; update <lastmod> on significant edits
```

Inside `index.html`, in order: `<head>` + 2 JSON-LD blocks → `<style>` →
`<body>` markup → one `<script>` IIFE.

## 4. Design language — established, not up for reinvention

- **Dark navy + gold.** `--bg #0a1322`, `--panel #122238`, `--gold #f0c55e`,
  `--blue #6fa3e8`, `--ink #edf1f8`.
- Rounded cards (`--radius 14px`), soft shadows, gradient accents on headings.
- System sans-serif throughout.

**A full visual redesign was attempted and explicitly rejected by the owner.**
Do not propose or apply a new palette, typeface pairing, or layout system.
Improve *within* this language. See PROJECT_MEMORY §Rejected before you get ideas.

## 5. Copy voice

- **First person singular.** "I build…", "I will tell you…". Never "we/our/us"
  when the subject is the practice.
  - *Exception:* FAQ **questions** are written in the reader's voice, so
    "Do **we** have to replace **our** systems?" is correct and intentional.
  - *Exception:* legal footer, `<meta>` tags and JSON-LD may use third person.
- **Address the reader as "you."**
- **Plain language first, technical term second.** Non-technical buyers must
  understand the value; technical readers still get the specifics. e.g. *"a new
  open standard called MCP (Model Context Protocol) lets an AI agent plug
  securely into the software your organization already uses."*
- Say **"AI agent"**, never "chatbot".

### Banned words and phrases

These read as machine-written. Do not reintroduce them:

> seamless / seamlessly · cutting-edge · empower · streamline your workflow ·
> leverage · robust · tailored solutions · drive innovation · unlock · elevate ·
> best-in-class · game-changer · holistic · synergy · world-class ·
> state-of-the-art · "it's not just X, it's Y"

Also avoid **hedge padding**: *genuinely, actually, simply, really, truly,
honestly*. These were deliberately stripped. Only three survive in the whole
document, each carrying real meaning — leave them alone.

## 6. The particle animation engine

One reusable factory drives six canvases. Do not fork it per section.

```js
createField(canvasElement, {
  patterns: ["hole"],   // which pattern(s) to cycle
  morph: true,          // true = converges into the logo (hero only)
  density: 3200,        // lower number = more particles
  maxDots: 340,
  goldRatio: 0.35       // proportion of gold vs blue dots
});
```

| Section | Canvas id | Pattern | Morphs to logo |
|---|---|---|---|
| Hero | `net` | `hole` — black hole, logo pulsing in the core | yes |
| About | `fxAbout` | `roads` — city street grid with gold vehicles | no |
| Use Cases | `fxUse` | `net` — network mesh | no |
| AI Adoption | `fxAi` | `brain` — neural web | no |
| FAQ | `fxFaq` | `quest` — drifting question marks | no |
| Contact | `fxContact` | `galaxy` — Milky Way | no |

Shape-based patterns (`brain`, `quest`, logo) are drawn by a painter function
into a 300×300 offscreen canvas, then sampled to particle targets by
`samplePoints()`. To add a shape, write a `paintX(g)` and follow `paintQuestion`.

**Performance contract:** every field pauses when its section is off-screen or
the tab is hidden. Preserve that. Never let a section animate unseen.

**Known trap:** particle *trails* are drawn from a previous position. If a
particle wraps a screen edge or teleports, guard the trail or it draws a
full-width flash across the section. This bug shipped once; do not repeat it.

## 7. Verification before you claim done

Run these. Do not report success without them.

```bash
# 1. Zero external runtime requests (expect only schema.org / w3.org / self)
grep -o -E 'https?://[^"'"'"' )]+' index.html | sort -u

# 2. Both JSON-LD blocks must parse
node -e 'const h=require("fs").readFileSync("index.html","utf8");
[...h.matchAll(/<script type="application\/ld\+json">([\s\S]*?)<\/script>/g)]
.forEach((b,i)=>{try{JSON.parse(b[1]);console.log("block",i,"OK")}
catch(e){console.log("block",i,"FAIL",e.message);process.exit(1)}})'

# 3. Visible FAQ count MUST equal FAQPage schema count (mismatch is penalised)
echo "visible: $(grep -c 'class="uc faq-card' index.html)"
echo "schema:  $(grep -c '"@type": "Question"' index.html)"

# 4. No banned words crept back in
grep -n -i -E 'seamless|cutting-edge|empower|leverage|chatbot' index.html
```

Then in a browser: no console errors, no horizontal scroll at **320px**, and
the accessibility toolbar still opens with `Ctrl+U`.

## 8. Accessibility contract

- **Pause Animations** in the toolbar must stop *every* canvas. If you add a
  field, confirm it responds.
- Card titles are `<h3 class="uc-title"><button class="uc-head">` — the heading
  is there for the document outline; the button is the control. Keep both.
- On desktop the collapse buttons are `disabled` (the card cannot collapse), so
  keyboard users do not land on dead controls. Do not "fix" this by removing
  `disabled` without also making desktop cards collapsible.
- A native `<button>` already handles Enter and Space. Never add a `keydown`
  handler to one — it double-fires.
- Respect `prefers-reduced-motion` everywhere.

## 9. SEO / GEO contract

- Title ≤ 60 chars, meta description ≤ 160 chars.
- **Any FAQ text change must be made in two places**: the visible `<p class="ans">`
  *and* the matching `acceptedAnswer` in the FAQPage JSON-LD. They must say the
  same thing.
- When adding a service, add it to `hasOfferCatalog` and add relevant terms to
  `knowsAbout`.
- `robots.txt` deliberately allows AI crawlers. Do not tighten it.
- ⚠️ The canonical URL is hardcoded to `https://renospatial.com/`. If the site
  is served from another origin, **fix it** — a wrong canonical suppresses
  indexing.

## 10. Git

Conventional commits, e.g. `feat(ui): …`, `fix(a11y): …`, `docs: …`.

Write the subject on one line (≤ ~60 chars). If you want a body, use a terminal
heredoc — pasting multi-line text into some editor commit boxes has collapsed it
onto the subject line twice in this repo's history.

Always stage new files (`git add -A`). Untracked assets have nearly been left
behind before.

## 11. Handover discipline

Multiple agents work on this repo and none of them share a session. Therefore:

1. **Before editing:** read `PROJECT_MEMORY.md`.
2. **After editing:** append to its Changelog — what changed, why, and anything
   you left unfinished. One or two lines is enough. An undocumented change is a
   change the next agent will undo.
