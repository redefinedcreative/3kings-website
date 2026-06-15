# 3 Kings — Design System

The design language for the 3 Kings band site. Use this when building new pages, components, or marketing assets that need to feel like the site. `index.html` is the runnable source of truth; this document explains the *why* and *how* so the same language can be reused outside the site.

## Design intent
Premium live rock + DJ fusion band. The aesthetic is **dark, regal, electric**: matte black backgrounds with high-contrast gold accents, oversized display type set in tight letter-spacing, soft radial glows that suggest stage lighting, and tactile micro-interactions (hover scales, ripple effects, magnetic buttons). It should feel like the lights just went down before the set starts.

## Color tokens
Defined as CSS custom properties on `:root` in `index.html`:

| Token | Hex | Usage |
|---|---|---|
| `--gold` | `#FFB700` | Primary brand accent — buttons, eyebrow pill, headings highlight, glows |
| `--dark-gold` | `#8B6914` | Hover/disabled gold variants, deep accents |
| `--black` | `#000000` | Page background; text on gold buttons |
| `--dark-gray` | `#1A1A1A` | Section variants, tooltip backgrounds, surfaces |
| `--white` | `#FFFFFF` | Primary body text on dark |
| `--off-white` | `#F5F5F5` | Reserved (currently unused) |
| `--gray` | `#8B95A5` | Secondary text, captions, member roles |

**Contrast:** Gold `#FFB700` on black is ~10.5:1 — WCAG AAA. New compositions must preserve this. Never pair gold *text* on white (insufficient contrast); for inverse, use black text on gold backgrounds.

**Glow rgba pattern:** `rgba(255, 183, 0, x)` at alphas 0.08, 0.15, 0.2, 0.3, 0.4 — used in radial gradients (spotlights) and box-shadows (button hover halos).

## Typography
Two families, both loaded via Google Fonts:

- **Display:** `'Bebas Neue', sans-serif` — uppercase via markup or `text-transform: uppercase`. Used for logo, headings (h1–h3), eyebrow, section labels, footer logo, hero tagline.
- **Body:** `'Space Grotesk', sans-serif` — weights `400`, `500`, `600`. Used for paragraphs, buttons, navigation, footer text.

`<head>` already preconnects to `fonts.googleapis.com` and `fonts.gstatic.com`. Don't add new families without a deliberate reason — the Bebas Neue / Space Grotesk pairing *is* the brand.

### Type scale (extracted from production)
| Use | Font | Size | Letter-spacing |
|---|---|---|---|
| Hero tagline | Bebas Neue | `clamp(4rem, 8vw, 8rem)` | `-2px` |
| Section heading XL ("Crown Your Event") | Bebas Neue | `clamp(3rem, 6vw, 6rem)` | `-1px` |
| Section heading | Bebas Neue | `2.5rem` | varies |
| Hero eyebrow (large) | Bebas Neue | `clamp(2rem, 5vw, 4.5rem)` | `-1px` |
| Footer logo | Bebas Neue | `2.5rem` | `4px` |
| Hero subtitle | Space Grotesk | `clamp(1rem, 2vw, 1.25rem)` | — |
| Bio text | Space Grotesk | `1.15rem` | — |
| Body / nav | Space Grotesk | `0.9rem`–`1rem` | `0.5px`–`2px` |
| Section label (small caps) | Bebas Neue | `0.85rem`–`0.95rem` | `3px`–`4px` |

**Rule of thumb:** display headings tight (`-1px` to `-2px`); small-caps labels loose (`+2px` to `+4px`); body relaxed (`0` to `+0.5px`).

## Spacing
Everything is in `rem`. Common rhythms:
- **Section vertical margin:** `10rem` desktop, `8rem` mobile (`margin: 10rem auto;`)
- **Section horizontal padding:** `0 4rem` desktop, `0 2rem` mobile
- **Grid gap:** `2rem`–`3rem`
- **Hero top padding:** `9rem` on mobile to clear the fixed header

When adding new sections, match this rhythm so they slot in seamlessly.

## Border radii
- `6–7px` — small inline elements (tooltips)
- `8px` — buttons, hero image
- `10px` — cards, member images, video containers
- `50px` — pill shapes (eyebrow)
- `50%` — circles (close button, icons)

## Components (reference)
All live in `index.html` as inline CSS classes.

- **`.magnetic-btn`** — primary CTA button. Gold background, black text, ripple-on-hover (`::before` pseudo growing to 300×300). Use for any primary action.
- **`.nav-cta`** — nav-bar CTA variant. Smaller, same gold accent.
- **`.hero-eyebrow`** — pill-shaped eyebrow at top of hero. Light gold background, gold border, uppercase via CSS.
- **`.gold-highlight`** — inline gold text span (used in tagline for "New Monarchs").
- **`.member-image`** — portrait image, `300px` desktop / `250px` mobile, `object-fit: cover`, `object-position: center top` (keeps faces in frame across aspect ratios).
- **`.bio-image`** — about-section image, `object-fit: cover; object-position: center`.
- **`.video-grid`** — 3-column on desktop (`repeat(3, 1fr)`), 1-column on mobile.
- **`.video-container`** — 16:9 iframe wrapper.
- **`.credentials-grid`** — equal columns with dividing borders between credentials; collapses to 1-col on mobile.
- **`.skip-to-main`** — visually hidden until focused (accessibility).

### Decorative pseudo-elements (ambient gold glow)
Sections (`.hero`, `.bio`, `.members`, `.credentials-grid`, `.booking`, `footer`) use `::before` / `::after` for soft radial gold glows — ambient stage-lighting feel. They're stylistic only; never load-bearing for layout. Pattern to reuse:

```css
section::before {
  content: '';
  position: absolute;
  /* offset partially off-section for bleed */
  width: 400–800px;
  height: same;
  background: radial-gradient(circle at center,
    rgba(255, 183, 0, 0.08–0.15) 0%,
    transparent 60%);
  pointer-events: none;
  z-index: 0;
}
```

**Containment rule:** `html` has `overflow-x: hidden` so these glows never cause horizontal scroll on mobile. **Do not remove that line** — without it, narrow viewports get an unwanted horizontal scrollbar.

## Motion
- **Hover:** `transform: scale(1.05)` + gold glow shadow, `transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1)`
- **Page load:** `fadeIn 1.2s ease-out 0.3s backwards`
- **Ambient:** `spotlightPulse 4s ease-in-out infinite` on `.hero::before`
- **Card tilt:** `--angle` custom property updated via JS (mouse-tracked)
- **Reduced motion:** `@media (prefers-reduced-motion: reduce)` already disables animations — keep this honored when adding motion.

## Responsive
- **Breakpoint:** `1200px` (single tier; below = mobile, above = desktop)
- Most grids collapse `repeat(N, 1fr)` → `1fr` below 1200px
- Don't introduce a tablet tier unless absolutely needed — the design is intentionally two-mode.

## Accessibility commitments (preserve these)
- **WCAG AAA color contrast** (~10.5:1 gold-on-black)
- **Semantic HTML** (`<header>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`)
- `aria-label` / `aria-labelledby` on interactive + structural elements
- Skip-to-main link (`.skip-to-main`)
- **Visible focus rings** (gold, 2px outline + 2px offset)
- Keyboard navigation works
- `prefers-reduced-motion` honored
- Alt text on every `<img>`

When adding new components, audit against this list before merging.

## Asset standards
### Images
- **Format:** WebP for photos (smaller than JPG/PNG at equivalent quality). Keep one `.jpg` fallback for `og:image` — some social platforms don't render WebP previews.
- **Location:** `/media/` in the repo root.
- **Naming:** prefer `hyphens-or-underscores.webp` over filenames with spaces (URL-encoded as `%20`, harder to read in logs). Existing `DJ Chaney.webp` / `Joe Conner.webp` / `Ryan Patrick.webp` use spaces — leave them as-is, but use hyphens for *new* files.
- **Sizing (current convention):**
  - Hero portrait: `~800×1200`, loaded with `fetchpriority="high"`, `loading="eager"`
  - About image: `~600×800`, `loading="lazy"`
  - Member portraits: at least `400×300`, `loading="lazy"`
  - OG image: `1200×630` (target — generate a proper export when you can)
- **Compression:** compress before committing. Don't push 20 MB+ source JPGs into git.
- **Raw originals** live outside the repo in `../Raw Media/` and `../Band Members/`.

### Videos
Embed via YouTube `<iframe>` (lazy-loaded). No locally-hosted video.

### Fonts
Only Bebas Neue + Space Grotesk via Google Fonts. Don't add new families.

### SEO meta
When content changes meaningfully, update the OG / Twitter / JSON-LD blocks in `<head>` to match. Keep all URLs on `3kmusic.com`. No `3kingsband.com` anywhere.

## Voice & tone
- **Lean into:** kingdom, throne, royal, monarchs, crown, electrify, fusion, kings, stage, electric.
- **Avoid:** generic event-vendor language ("for all your event needs"), discount/cheap framing, anything that reads as corporate-stiff.
- **Sentence energy:** short → long → short. Confident statements; no hedging.
- **Example (current bio):** "3 Kings is not just a band—it's a royal decree for unforgettable nights."

## Extending the system
When you build a new asset (a second site page, an email signature, a social graphic, a print one-sheet, a partner landing page):

1. **Pull colors from the table above** — don't introduce new hues. If a new accent is truly needed, derive it from existing tokens.
2. **Bebas Neue + Space Grotesk only.** No third family.
3. **Mirror the section rhythm** (`10rem auto` / `0 4rem` desktop; `8rem auto` / `0 2rem` mobile).
4. **Keep the gold-glow + dark-base pattern.** Ambient warmth on black is the brand.
5. **Preserve WCAG AAA contrast** — run a checker before shipping.
6. **Document the new component here** — a one-paragraph addition is fine, so the system stays current.

## Changelog for this document
- **2026-05-27** — Initial extraction from production `index.html`. Tokens, scale, components, motion, asset standards, and voice notes captured as the canonical brand reference.
