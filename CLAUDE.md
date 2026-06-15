# 3 Kings Band Website — Project Context

This file auto-loads when Claude Code (or any Claude tool) opens this repo. It captures live deployment facts, workflow rules, brand basics, and recent decisions. For the full design system see [DESIGN.md](./DESIGN.md).

## Project
- **Client:** 3 Kings — live rock + DJ fusion band, Las Vegas
- **Agency:** Redefined Creative (Keegan Lanier)
- **Status:** Production — site is live and being maintained

## Live deployment
- **Live URL:** https://3kmusic.com
- **Repository:** `redefinedcreative/3kings-website` (branch `main`)
- **Hosting chain:** GitHub Pages → Cloudflare → 3kmusic.com
- **Custom domain:** set via the `CNAME` file in repo root (= `3kmusic.com`) — **do not delete or change this file**
- **Auto-deploy:** every push to `main` deploys via GitHub Pages in ~30 s – 2 min
- **Cache:** Cloudflare caches aggressively; always hard-refresh (`Cmd+Shift+R`) after deploy

**Dead domain warning:** `3kingsband.com` does NOT resolve (NXDOMAIN). Old staging docs in `../files/` (sibling Google Drive folder) still reference it — that domain is not live. The real domain is `3kmusic.com`.

## Workflow — the rules that keep deploys clean
1. **Edit only in this local clone.** Never upload or edit files on github.com — that caused a divergence on 2026-05-27 and blocked pushes.
2. **Commit + Push via GitHub Desktop.** The CLI `git` on this machine has no GitHub credentials cached; GitHub Desktop holds its own auth.
3. **Hard-refresh after deploy** (`Cmd+Shift+R`). Cloudflare caches the HTML even when GitHub Pages has already updated.
4. **If GitHub Desktop shows phantom changes** or `.git` errors: that's Google Drive File Stream evicting files. Pause Drive sync during commit/push.

## Critical caveats
- **Booking email** is `threekings130@gmail.com`. On the live page it renders as a scrambled `/cdn-cgi/l/email-protection` link — that's **Cloudflare's Email Address Obfuscation**, not a bug. The `mailto:` works normally for real visitors.
- **YouTube embed IDs** (`MqGzPfq79TE`, `8I6YymNl9zA`, `RSSMSufvRiE`) are owned by their uploaders. If any creator disables embedding, the video shows Error 153 — no fix on our end.
- **No automated tests, no staging environment.** A push goes straight to live. Eyeball-check before pushing.
- **Clone lives in Google Drive** (operator's choice — risk noted). File Stream evicts files mid-operation, which can confuse git tooling. If commands report "No such file," retry; don't conclude it's missing.

## Brand quick-reference
- **Primary gold:** `#FFB700` (WCAG AAA on black, ~10.5:1)
- **Fonts:** Bebas Neue (display/headings, uppercase by default), Space Grotesk (body — weights 400/500/600)
- **Tone:** Premium, regal (kingdom, throne, monarchs, crown), party-energy. Confident, no hedging.
- **Eyebrow:** "The premiere Las Vegas party band"
- **Tagline:** "The New Monarchs of the Party"
- **Booking inbox:** `threekings130@gmail.com`

Full design tokens, components, motion, and asset standards: see [DESIGN.md](./DESIGN.md).

## Decision log
- **2026-05-27** — Removed the "Site under construction" banner (HTML, JS, and CSS). All `construction-banner` markup deleted.
- **2026-05-27** — Enabled the booking button: removed `disabled` class, dropped the ✕ icon and "under construction" tooltip, set `href="mailto:threekings130@gmail.com"`.
- **2026-05-27** — Replaced all stale `3kingsband.com` references with `3kmusic.com`: `canonical`, `og:url`, `og:image`, `twitter:url`, `twitter:image`, and JSON-LD `url`.
- **2026-05-27** — Updated JSON-LD `email` from `booking@3kings.com` → `threekings130@gmail.com`.
- **2026-05-27** — Pointed `og:image` / `twitter:image` to `https://3kmusic.com/media/3kings-hero.jpg` (existing JPG). Previous targets at `/images/og-image.jpg` were 404s.
- **2026-05-27** — Added `.gitignore` (`.DS_Store`); `.DS_Store` untracked from the repo.
- **2026-05-27** — Fixed mobile horizontal scroll by adding `overflow-x: hidden` to the `html` rule. (Only `body` had it; decorative radial glows were bleeding past viewport on narrow screens.)
- **2026-05-27** — `.member-image` set to `object-position: center top` so member portraits crop from the top (heads in frame on tighter aspect ratios).

## Known open items
- **Stale staging docs** in `../files/` (`CONFIG.json`, `DEPLOYMENT_GUIDE.md`, `BUILD_PROMPT.md`) still reference `3kingsband.com` and `booking@3kings.com`. Update or delete before they mislead a future reader.
- **`../files/build.sh` is broken** — case-sensitive grep that fails on the lowercase eyebrow text; also looks for a `media/` folder in the wrong location. It will always report "BUILD FAILED" even on a healthy site.
- **Raw uncompressed images** live only in `../Raw Media/` and `../Band Members/` on Google Drive. Consider an external backup (private repo, external drive) if you'll ever need to re-export at different sizes.
- **OG image** is currently `media/3kings-hero.jpg` (an existing file), but its actual dimensions are unverified vs. the declared 1200×630. A purpose-built 1200×630 export would render better in social previews.
- **No favicon currently declared** — worth adding (drop a `favicon.ico` or `favicon.png` and link it in `<head>`).
