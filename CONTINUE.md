# CONTINUE.md — Flameback Capital site + app handoff

> Handoff doc for picking this work up on another machine (e.g. a Windows PC).
> Written 2026-06-21. Read this top-to-bottom once before touching anything.

---

## 0. TL;DR / read this first

- This is a **static website + mobile-app prototype** for **Flameback Capital**, an
  Indian investment manager (SEBI-registered PMS + RIA). **No framework, no build step,
  no dependencies.** Every page is a single self-contained `.html` file with an inline
  `<style>` and inline `<script>`. Fonts come from Google Fonts over CDN. It is hosted on
  **GitHub Pages**.
- There are **three working copies on disk** and **two GitHub repos**. This is the single
  most important thing to understand — see §1.
- **You are currently reading the copy inside `v3-active/`, which is the OLD source and is
  NOT what's deployed.** The live, current work is in the sibling `flameback-publish/`
  folder (git repo `flameback-site`). A duplicate of this file lives there too. **Do your
  editing in `flameback-publish/`, not here.**

---

## 1. Project layout — the three copies (CRITICAL)

Everything lives under `…/Downloads/flameback-site 3/`:

```
flameback-site 3/
├── README.md                  ← bundle index (describes v3-active page-by-page)
├── v3-active/                 ← OLD subfoldered SOURCE. NOT a git repo. Stale (Jun 13).
│   ├── flameback-home.html        Marketing pages only — NO app screens here.
│   ├── who-we-are/  tools/  insights/  regulatory/  onboarding/
│   └── CONTINUE.md            ← (this file)
├── flameback-publish/         ← ★ CURRENT. git repo `flameback-site`. Served by Pages.
│   └── (flat: all .html in one dir, incl. 31 app screens + lobby + spec)
├── flameback-publish-v2/      ← git repo `flameback-site-v2`. A full RESTYLE of publish.
│   └── (same flat file set + v2.css)
├── _archive-landing-iterations/   old hero/landing experiments — do not deploy
└── _archive-v2-pages/             superseded pages — reference only
```

### What each is

| Folder | Git repo | Live URL | Role |
|---|---|---|---|
| `v3-active/` | none (not in git) | — | Original **subfoldered** source. Older. Marketing pages only. **Don't deploy from here.** |
| `flameback-publish/` | `sh-shenoy/flameback-site` | https://sh-shenoy.github.io/flameback-site/ | **The live site.** Flat structure. Full marketing site **+ the 31-screen app.** |
| `flameback-publish-v2/` | `sh-shenoy/flameback-site-v2` | https://sh-shenoy.github.io/flameback-site-v2/ | A parallel **redesign** of the exact same pages in a new visual language. |

`flameback-publish/` was created by **flattening** `v3-active/` (folders collapsed, internal
links rewritten to flat filenames like `flameback-tools.html`) and then the app screens were
added. The two have since diverged — **treat `flameback-publish/` as the source of truth.**

`flameback-publish-v2/` has the **same filenames and the same commit history** as
`flameback-publish/` — it is a restyle, not a different site. See §6 for the design
difference. **The two repos are maintained in parallel and can drift; keeping them in sync
is manual.** (Biggest project risk — see Known Issues.)

---

## 2. What the product is

**Flameback Capital** — a quant-driven Indian investment manager. Regulatory identities
that appear throughout the copy:
- **PMS** — Portfolio Management Service, SEBI reg `INP000007358`
- **RIA** — Registered Investment Adviser, SEBI reg `INA200013798`

There are **two surfaces**:

### A. Marketing site (~25 pages)
Desktop-first editorial pages. Key ones (flat names as in `flameback-publish/`):
- `flameback-home.html` — homepage. `index.html` is a copy of it.
- `flameback-who-we-are.html`, `flameback-what-we-do.html`, `flameback-our-work.html`
  (18 strategies + the "MOV" 8-layer stack), `flameback-clients.html`, `flameback-offbook.html`
- Who-we-are deep pages: `flameback-firm.html`, `flameback-team.html`,
  `flameback-philosophy.html`, `flameback-strategy-rd.html`, `flameback-technology.html`
- Insights: `flameback-insights.html`, `-alpha-feed`, `-qlab-notes`, `-opensource`, `-release-notes`
- Tools: `flameback-tools.html`, `-portfolio-doctor`, `-asset-map`, `-insurance-advisor`,
  `-screener`, `-economic-indicators`, `-market-indicators`, `-engagements`, `-international`,
  `-strategy-large-cap-momentum`
- Regulatory (SEBI disclosure drafts): `flameback-pms-disclosure.html`,
  `flameback-advisory-disclosure.html`

### B. The investing app (31 mobile-first screens)
Self-contained interactive prototype, built screen-by-screen from
**`Screen-by-Screen Functional Specifications.pdf`** (see §8 for where the source PDFs are).
Files are `flameback-app-*.html` plus `flameback-lobby.html`. The whole spec is also rendered
in the design system as **`flameback-spec.html`** (has a TOC + per-screen "View screen" links —
the best single starting point to understand the app).

**Flow** (spec uses letter codes A–K):
```
A  Splash / entry        flameback-app-splash.html   ← app entry point
   login / register      flameback-app-login.html, flameback-app-register.html
B  Risk profiling        flameback-app-profile-intro.html (S-B00, the universal CTA target),
                         -quick-profiler, -finances (S-B01, DOB+18 check, home-status),
                         -income-streams, -income-security, -income-outlook,
                         -family-wealth, -experience, -restrictions
   KYC                   flameback-app-kyc.html (PAN-driven: enter PAN → name/DOB/address
                         auto-fill, all editable; "Begin" gated on completion)
C  Lobby / dashboard     flameback-lobby.html (also has a post-investment "funded" state per S-C01)
D  Invest                flameback-app-catalogue.html (filters: risk level + style),
                         -marketplace (S-D03 "build your own", risk+style filters),
                         -product, -amount
H  AI portfolio          flameback-app-portfolio-path, -portfolio-preference,
                         -portfolio-review, -portfolio-actions, -path
I  Activation            flameback-app-advisory-agreement, -advisor-config,
                         -broker-linking, -fee-payment, -deployment (lands in the funded lobby)
J/K + misc               flameback-app-drop-reaction, -comingsoon, -schedule-call*
```
\* `flameback-app-schedule-call.html` still exists on disk even though the funnel decision
was to remove scheduling (see Known Issues).

**Funnel directive (locked by the user):** the app is **investing-first**. "Schedule a
call/conversation" CTAs were removed across the site. The **universal primary CTA is
"Start investing" → `flameback-app-profile-intro.html` (S-B00)**. The path picker offers
*Start investing* (primary) + *Explore our strategies* (→ catalogue). Legitimate prose uses
of "schedule" (fee schedule, publication schedule) were left intact.

The marketing **`flameback-onboard-confirmed.html`** historically bridged into the app
("Enter your dashboard" → lobby) but is now effectively **orphaned** (kept, harmless).

---

## 3. Tech stack & conventions

- **Pure static HTML.** No npm, no bundler, no JS framework, no server-side code. Open any
  file in a browser and it works.
- Each page embeds its **own** `<style>` and `<script>`. There is **no shared CSS file** in
  v1 — the design tokens are copy-pasted into each page's `:root` block. (v2 adds one small
  shared `v2.css`, see §6.)
- **Fonts** load from Google Fonts CDN via `<link>` in each `<head>`. So the pages need
  network access to look right.
- **Dark/light theme** via a `.light` class on `<html>` that overrides the `:root` CSS
  custom properties.
- **Grain overlay** via a `body::after` SVG-noise layer (v1 only; v2 turns it off).
- **`design-system.md`** (in each publish folder) is the **source of truth for tokens,
  fonts, components, and voice.** Read it before building any new page. (147 lines; it has
  the exact `:root` block to copy verbatim.)
- **Voice rules** (from design-system.md / README): third-person plain English;
  sentence-case headlines with a terminal full stop; italic-orange accent words. **Banned
  words:** "core", "folder", "audited", "highest standards", eagle metaphors.
- **`reload.js`** (both publish folders): a dev-only auto-reload script — polls the current
  page with HEAD requests every 6s and reloads all open tabs when a new deploy is detected
  (uses BroadcastChannel). **Remove `reload.js` + its `<script src="reload.js">` tags before
  go-live.** Note it needs to be served over HTTP — it won't behave on `file://`.

---

## 4. Setup on a new machine (Windows)

Nothing to install for the *site itself* — it's static. You need tools to edit and deploy.

1. **Install:**
   - [Git for Windows](https://git-scm.com/download/win)
   - [GitHub CLI](https://cli.github.com/) (`gh`) — used for Pages config and auth
   - [VS Code](https://code.visualstudio.com/) (recommended; the **Live Server** extension
     is handy for previewing)
   - Python 3 (optional, gives you a one-line static server) — or use Live Server instead
   - A modern browser (Chrome/Edge)

2. **Authenticate GitHub** as the account that owns the repos:
   ```
   gh auth login
   ```
   The repos are under the **`sh-shenoy`** GitHub account. You need access to that account
   (or have the owner add you as a collaborator). `gh` was previously authed as `sh-shenoy`.

3. **Clone the repo(s):**
   ```
   git clone https://github.com/sh-shenoy/flameback-site.git        # ★ the live site
   git clone https://github.com/sh-shenoy/flameback-site-v2.git     # the v2 restyle (if needed)
   ```
   You do **not** need `v3-active/` on the new machine — it's the stale pre-flatten source
   and is not in git. Everything current is in `flameback-site`.

4. **Preview locally.** Don't just double-click the file — serve it over HTTP so `reload.js`
   and any fetches work:
   ```
   cd flameback-site
   python -m http.server 8000          # then open http://localhost:8000/
   ```
   (or right-click a file in VS Code → *Open with Live Server*). App entry point:
   `http://localhost:8000/flameback-app-splash.html`. Marketing entry: `index.html`.

5. **Edit → deploy:**
   ```
   git add -A
   git commit -m "…"
   git push
   ```
   GitHub Pages rebuilds automatically (~1 min). Branch is `main`, Pages source path is `/`
   (root). Check Pages status with:
   ```
   gh api repos/sh-shenoy/flameback-site/pages
   ```

That's the whole loop. No build, no CI, no env vars.

---

## 5. Current state — what's built and working

**Marketing site:** all v3 top-level + who-we-are deep pages are built and live.
**App:** all **31 screens** are built and live in `flameback-publish/`, wired into the
investing-first flow described in §2. `flameback-spec.html` renders the full functional spec.

Recent committed work (top of `flameback-publish` git log, most recent first):
- Onboarding copy made direct & friendly (dropped editorial commentary)
- Finances screen: added home-status question; **KYC reverted to manual entry** (type as
  per PAN, no auto-fetch)
- Added the **post-investment funded lobby** dashboard (S-C01); `deployment` now lands there
- S-B09: removed goal-linkage question; objective reworded to "How do you want to grow your
  wealth with us?"
- KYC made **PAN-driven** (enter PAN → name/DOB/address auto-fill, editable; Begin gated)
- Marketplace & Catalogue: replaced risk **slider** with risk-level + style **filters**
- Finances: age replaced with **date-of-birth** picker (18+ check)

> Note the KYC history is slightly back-and-forth (PAN-autofill was added, then "reverted to
> manual entry"). Read `flameback-app-kyc.html` to see the actual current behaviour rather
> than trusting the commit subjects.

Both repos' working trees are **clean** and **up to date with origin** as of this writing.

---

## 6. The two design systems (v1 vs v2)

Same pages, two visual languages. `publish-v2` files load `v2.css` **after** their inline
`<style>` so the override wins.

| | **v1** (`flameback-publish` / `flameback-site`) | **v2** (`flameback-publish-v2` / `flameback-site-v2`) |
|---|---|---|
| Background | `#1A1714` (warm near-black) | `#0B0C0E` (cool near-black) |
| Accent | `#FF6D0A` (signature orange) | `#B59669` (champagne / muted gold) |
| Display font | Cormorant Garamond | Instrument Serif |
| Body font | Source Serif 4 | Geist |
| Mono | JetBrains Mono | Geist Mono |
| Texture | grain overlay on | **grain off**, shadowless, "calm and quiet" |
| Shapes | — | pill buttons, 8px cards, 4px inputs |
| Shared CSS | none (tokens inline per page) | adds small `v2.css` override (~22 lines) |

**Open decision:** maintaining two parallel sites with identical filenames is duplicated
effort and a drift hazard. Someone should decide whether v2 supersedes v1 (and retire v1) or
whether they serve different purposes. Until then, **any content change has to be made in
both repos.**

---

## 7. Next steps (priority order)

From `../README.md` "Still pending" + the user's directives. Roughly highest-leverage first:

1. **Decide v1 vs v2 direction** (see §6). This blocks everything else — you don't want to
   build new work twice. Pick one as canonical, or formalize why both exist.
2. **Decide the marketing "Onboard" button target.** It currently points to the older
   `flameback-onboard-welcome.html` intake flow, **not** the app splash. The user hasn't
   decided whether to repoint it to the app. Resolve and propagate.
3. **Port Insights & Tools to the v3 design system.** They were carried over from v2-era
   styling and still need a v3 pass. ("Resources" was renamed to "Tools".)
4. **Insurance Advisor legal review** (`flameback-insurance-advisor.html`) — flagged as the
   highest regulatory-risk surface.
5. **Onboarding chapters 6–9 + RM Console** (Pipeline / Client Record / Live Session /
   Intake) — only chapters 1–5 of the intake flow exist.
6. **Mobile hamburger menu** — propagate a consistent mobile nav across all pages.
7. **Top-nav update** — some older pages still show the v2-era nav; propagate the current nav.
8. **Replace placeholder art with real photography** — 4 leader-portrait SVGs (team page),
   the office sketch (firm page), 5 client-identifier SVGs (clients page).
9. **Swap in audited/real numbers** — all performance, AUM, and strategy figures across the
   site and app are **illustrative placeholders**.
10. **Legal review of both SEBI disclosure documents** (`flameback-pms-disclosure.html`,
    `flameback-advisory-disclosure.html`) — working drafts for "Vishwas".
11. **Before go-live:** delete `reload.js` and its `<script>` tags from every page.

---

## 8. Key files to read first (in order)

1. **`flameback-publish/design-system.md`** — tokens, fonts, components, voice. The rulebook.
2. **`flameback-publish/flameback-spec.html`** — the full 31-screen app spec, rendered, with
   per-screen links. Best map of the app.
3. **`flameback-publish/flameback-app-splash.html`** — app entry; trace the flow from here.
4. **`flameback-publish/flameback-home.html`** + **`index.html`** — marketing entry.
5. **`../README.md`** — bundle index; describes the (older) `v3-active` page set and the
   pending list.
6. Any one app screen, e.g. **`flameback-app-finances.html`**, to see the standard page
   skeleton (inline style + script, the `:root` token block, theme toggle, nav).

### Source documents (NOT in git — kept locally)
The app was specced from PDFs that live in the **grandparent** download folder
(`…/Downloads/`, one level above `flameback-site 3/`):
- **`Screen-by-Screen Functional Specifications.pdf`** ← primary app spec
- `FLOW SCREEN BY SCREEN.pdf`, `RIA APP FLow.pdf`, `RIA APP FLow (2).pdf`
- `Flameback · Prototype (Print).pdf`
- (also various art PDFs: `fish_shark_art…`, `medallion_print…`)

**Copy these to the new machine separately** — they won't come down with `git clone`.

---

## 9. Known issues / gotchas

- **`v3-active/` is stale and not deployed.** Editing here does nothing for the live site.
  It's the pre-flatten source from Jun 13. Use `flameback-publish/`.
- **Two diverging repos** with identical filenames (`flameback-site` and `-v2`). No
  automation keeps them in sync. Easy to update one and forget the other. (See §6.)
- **`flameback-app-schedule-call.html` still exists** even though the funnel decision was to
  remove scheduling. Memory notes a deletion that isn't reflected on disk — verify and clean
  up if scheduling is truly gone.
- **`flameback-onboard-confirmed.html` is orphaned** (no longer in the main flow; left in
  place, harmless).
- **All numbers are placeholders** — performance, AUM, fees, strategy stats. Do not treat as
  real.
- **`reload.js` is dev-only** and polls every 6s; remove before launch.
- **Fonts & some scripts need HTTP** — preview via a local server, not `file://`.
- **GitHub access is tied to the `sh-shenoy` account.** Without auth to it you can read the
  public Pages site but can't push. Sort `gh auth login` first thing.
- The disclosure docs and Insurance Advisor are **unreviewed legal/regulatory drafts.**

---

## 10. Quick command reference

```bash
# clone the live site
git clone https://github.com/sh-shenoy/flameback-site.git
cd flameback-site

# preview locally over HTTP (pick one)
python -m http.server 8000          # http://localhost:8000/
# …or VS Code → right-click file → Open with Live Server

# deploy a change
git add -A && git commit -m "msg" && git push      # Pages rebuilds in ~1 min

# check Pages status / URL
gh api repos/sh-shenoy/flameback-site/pages
```

Live URLs:
- v1: https://sh-shenoy.github.io/flameback-site/
- v2: https://sh-shenoy.github.io/flameback-site-v2/
