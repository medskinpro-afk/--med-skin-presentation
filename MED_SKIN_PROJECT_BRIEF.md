# Med & Skin SRL — Corporate Presentation Website
## Complete Project Brief for Claude Code

---

## 1. PROJECT OVERVIEW

**What it is:** A single-page corporate presentation website (business card / pitch deck format) for Med & Skin SRL. It is NOT an e-commerce site. No forms, no registration, no social media integration.

**Purpose:** Given to doctors, partners, and clients to browse. Think of it as a digital brochure / slide deck in a browser.

**Live URL:** https://medskinpro-afk.github.io/--med-skin-presentation/
**GitHub repo:** https://github.com/medskinpro-afk/--med-skin-presentation
**Main file:** `index.html` (single self-contained file — all CSS, JS, and photos embedded as base64)

---

## 2. COMPANY INFO

| Field | Value |
|---|---|
| Company | Med & Skin SRL |
| Founded | 2017, Brussels, Belgium |
| HQ | Chaussée d'Etterbeek 168, 1040 Etterbeek, Bruxelles |
| Email | info@med-skin.be |
| Phone | +32 471 30 77 52 |
| Website | https://med-skin.be |
| Events | https://med-skin.events |
| Markets | Benelux, United Kingdom, France |
| Brand partners | 9+ |
| Trainings | 500+ |

**Business:** Premium distributor of professional cosmetics and aesthetic medicine products. Combines injectable medicine with cosmeceutical brands. Serves dermatologists, aesthetic doctors, clinics, and beauty institutes.

---

## 3. FILE ARCHITECTURE

```
index.html                ← Single self-contained file (~2.87 MB)
│
├── <head>
│   ├── Meta tags (SEO, OG, canonical)
│   ├── No Google Fonts (system fonts only)
│   └── <style> — all CSS inline
│
├── <body>
│   ├── <header class="site-header">    ← Fixed top, z-index: 300
│   ├── <div class="mouse-spotlight">   ← z-index: 5, pointer-events: none
│   ├── <div class="deck" role="main">  ← Fixed, z-index: 10
│   │   ├── Slide 1 — Company Overview
│   │   ├── Slide 2 — Portfolio & Timeline
│   │   ├── Slide 3 — Education & Seminars
│   │   ├── Slide 4 — International Congresses
│   │   └── Slide 5 — Photo Gallery
│   ├── <nav class="nav-controls">      ← Fixed bottom, z-index: 300
│   ├── <div class="lightbox">          ← z-index: 1000
│   └── <script>                        ← All JS inline
```

**⚠️ CRITICAL z-index rules (buttons not working was caused by z-index conflicts):**
- Header: `z-index: 300`
- Nav controls: `z-index: 300`
- Deck: `z-index: 10`
- Mouse spotlight: `z-index: 5, pointer-events: none`
- Lightbox: `z-index: 1000`
- **NEVER use `backdrop-filter` on header or nav** — it breaks click hit-testing
- **NEVER use `transform: scale()` on `.slide`** — creates stacking context that blocks clicks
- Only `transform: translateY()` for slide animation is safe

---

## 4. COLOR PALETTE

```css
:root {
    /* Backgrounds */
    --bg:          #ffffff;
    --bg-2:        #f4f8fc;
    --surface:     #ffffff;
    --surface-2:   #eef4fa;

    /* Borders */
    --border:      rgba(39,170,225,0.15);
    --border-gold: rgba(39,170,225,0.4);

    /* Brand accent blue */
    --gold:        #27aae1;   /* PRIMARY ACCENT — used everywhere */
    --gold-light:  #5cc3ed;
    --gold-dim:    rgba(39,170,225,0.08);

    /* Text hierarchy */
    --text:        #1a1a1a;   /* Headlines, primary */
    --text-mid:    #58595b;   /* Subtitles, body */
    --text-muted:  #8a8b8c;   /* Details, labels */
    --text-pale:   #c0c1c2;   /* Inactive dots */

    /* Layout */
    --header-h: 64px;
    --nav-h: 62px;
}
```

**Note:** Variable names use `--gold` historically but the actual color is `#27aae1` (Med & Skin brand blue). Don't rename them — too many references throughout the code.

---

## 5. TYPOGRAPHY

**No Google Fonts imported.** Pure system font stack:

```css
--font-display: 'Century Gothic', 'Myriad Pro', Tahoma, 'Trebuchet MS', system-ui, sans-serif;
--font-body:    'Century Gothic', 'Myriad Pro', Tahoma, 'Trebuchet MS', system-ui, sans-serif;
--font-ui:      'Century Gothic', 'Myriad Pro', Tahoma, system-ui, sans-serif;
```

Century Gothic is the brand font. It renders as Tahoma on systems without it.

---

## 6. SLIDE STRUCTURE

### Slide 1 — Company Overview (`data-slide="1"`)
- Section label + H1 "Med & Skin SRL"
- Address block (right-aligned)
- KPI grid (4 cards): Founded 2017 / Markets 3+ / Brand partners 9+ / Trainings 500+
- Footer: company description + contact links

### Slide 2 — Portfolio & Business Development (`data-slide="2"`)
- Brand portfolio grid (4-col on desktop, 2-col on mobile): 8 brands
- Company timeline (5 entries: 2017, 2018, 2019, 2020, 2021–2025)
- Two-column layout: brands left + timeline right
- JS: `applyResponsiveGrids()` controls column switching at 640px

### Slide 3 — Education, Congresses & Market Activity (`data-slide="3"`)
- Two-column layout: Seminars left + Congress timeline right
- **Seminars (6 cards):** Belgian seminars 2025 (seminar-grid, 2-col)
- **Congress timeline:** 3 entries with `.ct-dot` dots (blue = SBME, grey = Estetika, SSBM)
- Footer with www.med-skin.be and email

### Slide 4 — International Congress Presence (`data-slide="4"`)
- 3 feature cards (`congress-feature-card`) in a grid
- Each has: colored accent strip, large number (01/02/03), badge, tags
- Card 1 (SBME-BVEG): gold accent + "Annual" badge
- Cards 2-3 (Estetika, SSBM): dim accent + "Attended ✓" badge

### Slide 5 — Photo Gallery (`data-slide="5"`)
- 3 sections: SBME 2025, Estetika Brussels, Med & Skin Seminars
- **SBME grid** (`.photo-grid-3`): Special layout — portrait photo spans 2 rows left, 2 landscape right
- **Estetika grid** (`.photo-grid-5`): 5 columns
- **Seminars grid** (`.photo-grid-5`): 5 columns, 10 photos
- All photos = embedded base64 JPEGs (compressed to ~900px width, quality 72)
- Clicking any photo opens **lightbox** with prev/next navigation

---

## 7. BRANDS IN PORTFOLIO

1. **Aesthetic Dermal** — Injectables & fillers
2. **Apriline** — HA dermal fillers
3. **Dr.Pen** — Microneedling devices
4. **HL – Always Active** — Holy Land professional skincare
5. **RRS** — Royal Renew System mesotherapy
6. **Skin Tech** — Chemical peels
7. **WiQo Med** — PRX‑T33® biorevitalisation
8. **DSD de Luxe** — Trichology & hair care

---

## 8. SEMINARS (Slide 3 — Belgian events 2025)

| Date | Title | Speaker / Location |
|---|---|---|
| 24 avr. | Peeling chimique & RRS® Spanish Glow | Dr Philippe Deprez · Dolce La Hulpe · Skin Tech |
| 16 juin | Protocole PRX‑T33 + Cellbooster® Suisselle | Samira Hjiaj · Chée d'Etterbeek 168 · WiQo |
| 26 sep. | WiQo / Cell‑Booster – Belgian Edition | Elke Coppens · Oude Abdij Drongen |
| 15 nov. | Skin Tech RRS Spanish Glow | Dr Houda Laatabi · Rue du Lac 37 · Skin Tech |
| 29 nov. | Formation Peeling Phénol – Lip & Eyelid | Dr Bibiana Romero · Chée d'Etterbeek 168 · Skin Tech |
| 14 oct. & 25 nov. | Holy Land / WiQo – Protocoles de soin | Samira Hjiaj · HL & WiQo |

---

## 9. CONGRESSES

| Name | Location | Badge | Brands |
|---|---|---|---|
| SBME‑BVEG Congress | Bruxelles | Annual (blue) | WiQo Med · RRS · Skin Tech · Holy Land |
| Estetika Brussels | Tour & Taxis, Bruxelles | Attended ✓ | Full portfolio |
| Congrès International Francophone de Mésothérapie | Belgique, Francophone | Attended ✓ | RRS · Royal Renew System |

---

## 10. PHOTOS (embedded as base64)

### Logo
- Med & Skin logo — JPEG ~72KB base64, in `<img class="header-logo">`

### Gallery (19 photos total)
- **SBME BVEG 2025**: 3 photos (1 portrait + 2 landscape)
- **Estetika Brussels**: 5 photos (1 JPG + 4 professional PNG shots)
- **Med & Skin Seminars**: 10 photos

All compressed with Pillow: max 900px wide, quality 72. Original sources were in `/Photos MedSkin/` folder structure.

---

## 11. NAVIGATION SYSTEM

### Bottom nav bar
```html
<nav class="nav-controls">
  [‹] [dots] [›] [1/5] | [Overview] [Portfolio] [Education] [Congrès] [Galerie]
</nav>
```
- Arrows: `changeSlide(±1)`
- Dots: click → `goToSlide(n)`, auto-generated by JS
- Counter: `1 / 5` (aria-live)
- Labels: hidden on tablet/mobile (≤960px)

### Keyboard navigation
- `ArrowRight` / `Space` → next slide
- `ArrowLeft` → previous slide
- In lightbox: `Escape` → close, `ArrowLeft/Right` → nav photos

### Touch/swipe
- Swipe left = next, swipe right = previous (threshold: 50px)

---

## 12. SLIDE ANIMATION SYSTEM

```javascript
function animateSlide(slide, skipAnim) {
    if (window.innerWidth <= 640 || skipAnim) {
        // Mobile: instant, no animation
        slide.querySelectorAll('.reveal').forEach(el => {
            el.style.opacity = '1';
            el.style.transform = 'none';
            el.style.transition = 'none';
        });
        return;
    }
    // Desktop: staggered fade-in (each .reveal element delays by i * 0.09s)
    slide.querySelectorAll('.reveal').forEach((el, i) => {
        el.style.opacity = '0';
        el.style.transform = 'translateY(16px)';
        // then animate in
    });
}
```

`.reveal` class = any section that animates in when slide becomes active.
First slide is initialized with `skipAnim = true` (instant, no animation).

---

## 13. RESPONSIVE BREAKPOINTS

| Breakpoint | Behavior |
|---|---|
| `> 960px` | Full desktop: 4-col KPI, nav labels visible |
| `≤ 960px` | Tablet: 2-col KPI, nav labels hidden, logo 28px |
| `≤ 640px` | Mobile: body scrolls, slides stack, all grids 1-2 col |
| `≤ 380px` | Small phone: header 46px, smaller text |

### Mobile mode changes
- `html, body { overflow: auto; height: auto; }` — body scrolls
- `.deck { position: relative; height: auto; }` — not fixed
- `.slide { display: none; }` / `.slide.active { display: flex; }` — only active slide shown
- `.reveal { opacity: 1; transform: none; }` — no animations

### Grid responsive (via JS `applyResponsiveGrids()`)
```javascript
const mob = window.innerWidth <= 640;
// Portfolio outer: 1fr vs 1.3fr 1fr
// Brand cards: 2-col vs 4-col
// Seminar grid: 1fr vs 1fr 1fr
// Congress grid: 1fr vs repeat(3, 1fr)
```

---

## 14. LIGHTBOX

```html
<div class="lightbox" id="lightbox" onclick="closeLightbox(event)">
    <button class="lightbox-close" onclick="closeLightboxDirect()">✕</button>
    <button class="lightbox-prev" onclick="lightboxNav(-1)">‹</button>
    <img id="lightbox-img" src="" alt=""/>
    <button class="lightbox-next" onclick="lightboxNav(1)">›</button>
    <div class="lightbox-caption" id="lightbox-caption"></div>
</div>
```
- Opens on `.gallery-item` click
- `lbImages` = all `.gallery-item img` on the page
- Caption: "3 / 18"
- Keyboard: Escape closes, arrows navigate
- Background click closes

---

## 15. KEY CSS CLASSES REFERENCE

| Class | Purpose |
|---|---|
| `.site-header` | Fixed top bar with logo + contacts |
| `.deck` | Fixed container for all slides |
| `.slide` | Individual slide (position: absolute, inset: 0) |
| `.slide.active` | Currently visible slide |
| `.content` | Inner scrollable content wrapper (max-width: 1180px) |
| `.gradient-mesh` | Background blob container |
| `.blob` | Animated background blur circle |
| `.reveal` | Animates in when slide activates |
| `.section-label` | Small uppercase label with line (e.g. "Company Overview") |
| `.slide-title` | Main heading per slide |
| `.slide-subtitle` | Paragraph below title |
| `.section-h3` | Small uppercase section header with bottom border |
| `.kpi-grid` / `.kpi-card` | 4-column stats grid |
| `.brand-card` | Portfolio brand tile |
| `.timeline` / `.timeline-item` | Company history timeline (Slide 2) |
| `.seminar-grid` / `.seminar-card` | Seminar listing (Slide 3) |
| `.congress-timeline` / `.ct-item` | Congress timeline (Slide 3 right column) |
| `.congress-feature-card` / `.cfc-*` | Feature cards (Slide 4) |
| `.photo-grid-3` | SBME gallery grid (portrait + 2 landscape) |
| `.photo-grid-5` | 5-column photo grid |
| `.gallery-item` | Photo container with hover zoom |
| `.nav-controls` | Bottom navigation bar |
| `.nav-btn` | Arrow navigation buttons |
| `.slide-dots` / `.dot` | Dot indicators |
| `.nav-labels` / `.nav-label` | Text slide labels |
| `.slide-footer` | Footer row inside each slide |
| `.footer-right` | Right-aligned footer links |
| `.lightbox` | Full-screen photo viewer |

---

## 16. KNOWN ISSUES & FIXES APPLIED

### Buttons not clickable — FIXED
**Root cause:** `backdrop-filter: blur()` on nav/header creates compositing layer issues that break hit-testing in some browsers. Also `transform: scale()` on slides creates stacking contexts.

**Fix applied:**
- Removed ALL `backdrop-filter` from nav and header
- Changed slide animation from `translateY(8px) scale(0.995)` to `translateY(10px)` only
- Set explicit z-indexes: deck=10, spotlight=5, header/nav=300

### Email obfuscation — FIXED
Original file had Cloudflare `__cf_email__` obfuscation. All replaced with direct `mailto:info@med-skin.be` links.

### Timeline overflow (Slide 2) — FIXED
Timeline items were too tall. Reduced margins and font sizes for compact fit.

### Congress timeline dots misaligned — FIXED
Dots now use `position: absolute` with `left: -1.35rem` relative to `.ct-item`.

---

## 17. DEPLOYMENT

**Platform:** GitHub Pages (free, static)
**Repo:** `medskinpro-afk/--med-skin-presentation`
**Branch:** `main`, root `/`
**File:** `index.html` (just this one file — everything is self-contained)

To update: replace `index.html` in the repository → GitHub Pages auto-deploys in 1-2 minutes.

---

## 18. WHAT NOT TO DO

- ❌ Do NOT add `backdrop-filter` to `.nav-controls` or `.site-header`
- ❌ Do NOT use `transform: scale()` on `.slide` or `.slide.active`
- ❌ Do NOT add Google Fonts (use Century Gothic system stack)
- ❌ Do NOT add e-commerce, forms, or social media
- ❌ Do NOT use Cloudflare email obfuscation patterns
- ❌ Do NOT change the z-index hierarchy (deck=10, nav/header=300)
- ❌ Do NOT add external dependencies — must remain a single self-contained HTML file

---

## 19. SUGGESTED FUTURE IMPROVEMENTS

- Add a language switcher (FR / EN / NL)
- Add a "Contact us" CTA button
- Add animation on KPI numbers (count-up effect)
- Add a 6th slide for "Why partner with us" / value proposition
- Add med-skin.events calendar integration
- Better mobile gallery (horizontal scroll on mobile instead of 2-col grid)
- Add print/PDF export button

---

## 20. CONTACT INFO IN FILE

```
Company:  Med & Skin SRL
Email:    info@med-skin.be (mailto: link)
Phone:    +32 471 30 77 52
Address:  Chaussée d'Etterbeek 168, 1040 Etterbeek, Bruxelles
Website:  https://med-skin.be
Events:   https://med-skin.events
Estetika: https://www.estetika.be/en/
```

---

---

## 21. VERSION TRACKING RULE

**Every time the site is updated, bump the version in TWO places:**

1. `<span class="header-version">vX.X</span>` — visible in the header (search for `header-version`)
2. The version history comment at the very top of `index.html` — add a new line with version, date and what changed

**Current version history:**
| Version | Date | Changes |
|---|---|---|
| v1.0 | 2026-04-07 | Initial release: 5 slides, KPI count-up, mobile gallery scroll |
| v1.1 | 2026-04-07 | Header polish, hero tagline, CTA button, contact section, brand card hover |
| v1.2 | 2026-04-07 | PDF/print layout fixed (A4, per-slide page breaks, gallery grids) |
| v1.3 | 2026-04-07 | Version tag added to header |

---

*Document generated from conversation history with Claude (claude.ai). Current version: v1.3 — index.html ~2.87MB, 5 slides, 19 embedded photos.*
