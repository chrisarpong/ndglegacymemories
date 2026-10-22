# NDG+ Legacy Memorials — Site Audit

**Date:** 22 October 2026 · **Auditor:** Automated code audit · **Scope:** `index.html`, `home.html`, `assets/`

## Summary

The site is a well-structured two-page static website with good visual design, clean semantic HTML, and solid foundations. However, several issues prevent a production launch. The **enquiry form relies entirely on `mailto:`, which will silently fail for any visitor without a configured desktop mail client** — the single most important blocker. SEO metadata is incomplete (no Open Graph, no favicon, no canonical, no JSON-LD). All 15 gallery images in `home.html` are correctly lazy-loaded, but none of the images anywhere on the site carry `width`/`height` attributes, guaranteeing layout shift. There is no `prefers-reduced-motion` media query despite an infinite typewriter animation, an infinite marquee, and a looping hero slideshow. The email address `hello@ndglegacymemorials.com` has a developer comment next to it confirming it is unverified, and an HTML comment `<!-- Confirm your business email address -->` is shipped to production. No critical asset-path mismatches were found — every referenced file exists and every file in `assets/` is referenced by at least one HTML page.

---

## Issues Table

| ID | Severity | File | Problem | Recommended Fix |
|----|----------|------|---------|-----------------|
| **1** | **Blocker** | `home.html:341-344` | The enquiry form submits via `mailto:` link (`location.href = mailto:…`). On mobile devices, many users have no mail app configured (they use Gmail in browser). The form silently does nothing, and the user's typed data is lost. | Replace with a real form backend: Formspree, Netlify Forms, a Vercel serverless function posting to email/WhatsApp, or at minimum a `mailto:` fallback with a copy-to-clipboard option and a WhatsApp deep-link alternative. |
| **2** | **Blocker** | `home.html:309` | HTML comment `<!-- Confirm your business email address -->` is shipped to production, confirming `hello@ndglegacymemorials.com` may not have a real mailbox. If the domain/mailbox doesn't exist, even users who have a mail app will bounce. | Provision the mailbox on the domain, or switch to a verified address. Remove the HTML comment. |
| **3** | **High** | Both | No favicon (`<link rel="icon">`), no apple-touch-icon. Browser tabs show a generic icon; saved-to-homescreen on iOS shows a blank. | Add a 32×32 `.ico` or `.svg` favicon and a 180×180 `apple-touch-icon.png`. |
| **4** | **High** | Both | No Open Graph (`og:title`, `og:description`, `og:image`, `og:url`) or Twitter Card meta tags. Links shared on WhatsApp, Facebook, or Twitter will show no preview image or description — critical for a business that markets via WhatsApp. | Add OG and `twitter:card` tags to both pages, with `og:image` pointing to a 1200×630 sharing image. |
| **5** | **High** | Both | No `<link rel="canonical">` on either page. With two entry points (`index.html` and `home.html`), search engines may index both or the wrong one. | Add `<link rel="canonical" href="https://ndglegacymemorials.com/">` to `home.html`. Consider adding `<meta name="robots" content="noindex">` to `index.html` (splash page). |
| **6** | **High** | Both | No `prefers-reduced-motion` media query. `index.html` has an infinite marquee (`animation: rl … infinite`), and `home.html` has an infinite typewriter loop and a looping hero slideshow. Users with motion sensitivity or vestibular disorders may be harmed. | Wrap all infinite/continuous animations in `@media (prefers-reduced-motion: no-preference) { … }` or disable them with `@media (prefers-reduced-motion: reduce) { animation: none; transition: none; }`. |
| **7** | **High** | Both | **No `width` / `height` attributes on any `<img>` element.** All 17 asset images across both pages lack intrinsic dimensions, causing Cumulative Layout Shift (CLS) as images load. | Add `width` and `height` attributes matching each image's natural dimensions to every `<img>` tag, or use `aspect-ratio` in CSS. |
| **8** | **High** | Both | No `LocalBusiness` JSON-LD structured data. Google cannot show rich results (map pin, hours, phone). | Add a `<script type="application/ld+json">` block to `home.html` with `@type: LocalBusiness`, `name: NDG+ Legacy Memorials`, `address: {Accra, Ghana}`, `telephone: +233200968878`, `url`, `image`, `description`, and `priceRange`. |
| **9** | **Medium** | `home.html:5` | Missing `viewport-fit=cover` in the viewport meta tag (present in `index.html` but not `home.html`). On iPhones with a notch/Dynamic Island, content may be clipped or not inset properly in fullscreen/PWA mode. | Change to `<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">` and use `env(safe-area-inset-*)` padding on the header and footer. |
| **10** | **Medium** | `home.html` | The header uses `position: sticky` with `backdrop-filter: blur(8px)`, but there is no `-webkit-backdrop-filter` fallback. Older Safari versions (< 14) on iOS will show a fully transparent header. | Add `-webkit-backdrop-filter: blur(8px)` alongside `backdrop-filter`. |
| **11** | **Medium** | `home.html:65-66` | `.trust .wrap` uses `grid-template-columns: repeat(4, 1fr)` with no `@media` override below `920px` — it switches to `1fr 1fr` at 920px but at 360–390px the two-column layout may still overflow or cramp text. | Add a breakpoint at ~480px to collapse to `grid-template-columns: 1fr`. |
| **12** | **Medium** | `home.html:81-84` | `.audience` uses `repeat(4, 1fr)`. At 920px it becomes `1fr 1fr`, but at 360px the two-column audience grid is very tight. | Collapse to single column below ~480px. |
| **13** | **Medium** | `home.html:110` | `.steps` uses `repeat(4, 1fr)` → `1fr 1fr` at 920px. Same 360px concern. | Consider single column below ~480px. |
| **14** | **Medium** | `home.html:104` | `.gallery { columns: 4 240px }`. At 360px width minus 48px padding = 312px, this will render as 1 column, which is fine, but the `column-gap: 16px` means images may be 296px wide. Acceptable but tight. | Test at 360px and confirm no horizontal overflow. |
| **15** | **Medium** | `index.html:13` | `html, body { overflow: hidden }` on the splash page. If the viewport is very short (landscape phone, 360×640 rotated), the "Enter site" button and marquee band may be cut off with no way to scroll. | Consider `overflow-y: auto` or ensure the layout adapts to landscape viewports with `min-height` queries. |
| **16** | **Medium** | `home.html` | No `<meta name="robots">` on either page. The splash page (`index.html`) is the Vercel entry point, so Google will index it. A splash page with no meaningful content should not be the indexed page. | Add `<meta name="robots" content="noindex, nofollow">` to `index.html` and ensure `home.html` is the canonical. |
| **17** | **Medium** | `index.html:48` | The marquee band images in `index.html` are duplicated via JS (`tr.appendChild(i.cloneNode())`), but the cloned `<img>` elements lose their `alt` attribute since `cloneNode()` without `true` copies the element but not attributes beyond the default. Actually, `cloneNode(false)` copies attributes but not children — for `<img>` this copies `src` and `alt`. However, these images are `aria-hidden="true"` on the parent, so empty alt is correct. **No action needed — false alarm on review.** | N/A |
| **18** | **Medium** | `index.html:60` | `cloneNode()` is called without argument (defaults to `false`— shallow clone). For `<img>` this is fine (no children), but the cloned images won't have `src` correctly if the browser hasn't resolved them yet. In practice this works, but `cloneNode(true)` is safer. | Change to `i.cloneNode(true)`. |
| **19** | **Medium** | `home.html` | The hero section loads 7 full-size background images (`backdrop.jpg`, `obituary-poster.jpg`, etc.) eagerly (no `loading="lazy"`), plus the video. Combined, these above-the-fold assets are ~1.8 MB of images + 2.1 MB video = ~3.9 MB before the page is interactive. | Serve hero slideshow images as smaller/compressed versions (e.g., 1200px wide, quality 75 WebP). Consider loading only the first 1–2 and lazy-loading the rest via JS when they're about to rotate in. |
| **20** | **Medium** | `index.html` | The marquee loads 10 images eagerly with no `loading="lazy"`. These are thumbnails by height (`height:100%` of a 110–200px band), but the source images are full-size (90–272 KB each, ~1.7 MB total). | Generate smaller thumbnail versions or add `loading="lazy"` and `decoding="async"` to the marquee images. |
| **21** | **Low** | `home.html:166` | The logo image in the hero `<div class="layer logo-reveal">` has `alt=""`. Since it's the NDG+ logo used as a visual element with `aria-hidden` not set, it should arguably have meaningful alt text (or the parent should be `aria-hidden`). | Add `alt="NDG+ Legacy Memorials logo"` or add `aria-hidden="true"` to the parent `<div>`. |
| **22** | **Low** | Both | Google Fonts are loaded via two `<link rel="preconnect">` tags and one `<link href="…/css2…">` — this is correct. However, two different font subsets are loaded across the two pages (index uses `wght@500;600`, home uses `wght@400;500;600;700`). This means navigating from splash to home triggers a new font download. | Unify the font request or preload the full weight range on `index.html` so it's cached before the user reaches `home.html`. |
| **23** | **Low** | `home.html:324` | The submit button text is "Prepare my enquiry", which is informative, but users may not understand it opens a mail app until they read the fine print below. If the mail app fails to open, there is no error message or fallback. | Show an inline fallback: "If your email app didn't open, WhatsApp us at 020 096 8878" or copy the message body to clipboard. |
| **24** | **Low** | Both | Colour contrast: The `--mute` colour (`#6b6f78`) on `--paper` background (`#fffdf9`) has a contrast ratio of approximately **4.7:1**, which passes AA for normal text but fails AAA. The `.sub` text (`#b9b5aa`) on `#000` in `index.html` is approximately **7.5:1** — passes. Gold on dark (`#c8a45d` on `#0b0d12`) is approximately **5.9:1** — passes AA. The `--cream` text `#d4d1c8` on the dark hero `#0b0d12` is approximately **11.6:1** — passes. Overall, contrast is acceptable at AA level. | Consider darkening `--mute` to `#5c5f67` for AAA compliance on body text. |
| **25** | **Low** | `home.html` | `.foot div a { margin-left: 24px }` — footer links use only margin for spacing with no `gap` on a flex parent. On very narrow screens the links may wrap awkwardly. | Use `flex-wrap: wrap; gap: 12px 24px` on the parent `div`. |
| **26** | **Low** | `home.html:346-367` | The typewriter animation is an infinite `while(true)` loop that never stops. On low-end devices this continuously manipulates the DOM. If the user scrolls away from the hero, the animation still runs. | Use `IntersectionObserver` to pause the typewriter when the hero is off-screen. |
| **27** | **Low** | `home.html:69` | `skip` button in `index.html` calls `enter()` → `location.href='home.html'`. The Escape key handler also calls `enter()`. These work correctly. The `Skip intro` button disappears (`.done .skip { opacity:0; pointer-events:none }`) once the video ends, but it remains in the DOM and tab order. | Add `tabindex="-1"` or `display:none` in the `.done` state. |
| **28** | **Low** | Deployment | No `vercel.json` for cache headers. Assets are fingerprint-free (e.g., `backdrop.jpg`, not `backdrop.a1b2c3.jpg`), so Vercel's default caching may serve stale images if they're ever updated. | Add a `vercel.json` with `Cache-Control: public, max-age=31536000, immutable` for `assets/**` and consider filename-hashing, or use shorter `max-age` with `must-revalidate`. Also add `cleanUrls: true` to drop `.html` extensions. |

---

## 1. Asset Integrity

### All asset references

| File | Attribute | Path | Exists? |
|------|-----------|------|---------|
| `index.html` | `src` | `assets/ndg-logo-reveal.mp4` | ✅ |
| `index.html` | `src` | `assets/ndg-logo.png` | ✅ |
| `index.html` | `src` | `assets/obituary-poster.jpg` | ✅ |
| `index.html` | `src` | `assets/brochure-black-gold.jpg` | ✅ |
| `index.html` | `src` | `assets/tshirt.jpg` | ✅ |
| `index.html` | `src` | `assets/rollup-heritage.jpg` | ✅ |
| `index.html` | `src` | `assets/memorial-book.jpg` | ✅ |
| `index.html` | `src` | `assets/invitation.jpg` | ✅ |
| `index.html` | `src` | `assets/keepsake-box.jpg` | ✅ |
| `index.html` | `src` | `assets/thank-you.jpg` | ✅ |
| `index.html` | `src` | `assets/obituary-a1.jpg` | ✅ |
| `index.html` | `src` | `assets/mugs.jpg` | ✅ |
| `home.html` | `src` | `assets/ndg-logo.png` | ✅ |
| `home.html` | `src` | `assets/ndg-logo-reveal.mp4` | ✅ |
| `home.html` | `src` | `assets/obituary-poster.jpg` | ✅ |
| `home.html` | `src` | `assets/brochure-black-gold.jpg` | ✅ |
| `home.html` | `src` | `assets/backdrop.jpg` | ✅ |
| `home.html` | `src` | `assets/memorial-book.jpg` | ✅ |
| `home.html` | `src` | `assets/rollup-heritage.jpg` | ✅ |
| `home.html` | `src` | `assets/tshirt.jpg` | ✅ |
| `home.html` | `src` | `assets/invitation.jpg` | ✅ |
| `home.html` | `src` | `assets/banner.jpg` | ✅ |
| `home.html` | `src` | `assets/obituary-a1.jpg` | ✅ |
| `home.html` | `src` | `assets/keepsake-box.jpg` | ✅ |
| `home.html` | `src` | `assets/mugs.jpg` | ✅ |
| `home.html` | `src` | `assets/programme-cover.jpg` | ✅ |
| `home.html` | `src` | `assets/programme-spread.jpg` | ✅ |
| `home.html` | `src` | `assets/rollup-etsey.jpg` | ✅ |
| `home.html` | `src` | `assets/thank-you.jpg` | ✅ |
| `home.html` | `href` | (same 15 gallery images) | ✅ |

**Missing files:** None.  
**Unreferenced files:** None — every file in `assets/` is used.  
**Filename safety:** All filenames are lowercase, use hyphens, and contain no spaces. ✅ Safe for Vercel/Linux.

---

## 2. Performance — Asset Sizes

| # | File | Size | Flag |
|---|------|------|------|
| 1 | `ndg-logo-reveal.mp4` | 2.10 MB | ⚠️ Under 3 MB limit but still large for mobile 3G |
| 2 | `obituary-a1.jpg` | 266 KB | — |
| 3 | `obituary-poster.jpg` | 260 KB | — |
| 4 | `thank-you.jpg` | 255 KB | — |
| 5 | `backdrop.jpg` | 252 KB | — |
| 6 | `keepsake-box.jpg` | 187 KB | — |
| 7 | `rollup-etsey.jpg` | 180 KB | — |
| 8 | `rollup-heritage.jpg` | 178 KB | — |
| 9 | `invitation.jpg` | 173 KB | — |
| 10 | `banner.jpg` | 171 KB | — |
| 11 | `ndg-logo.png` | 133 KB | — |
| 12 | `memorial-book.jpg` | 119 KB | — |
| 13 | `brochure-black-gold.jpg` | 106 KB | — |
| 14 | `programme-cover.jpg` | 93 KB | — |
| 15 | `mugs.jpg` | 90 KB | — |
| 16 | `tshirt.jpg` | 89 KB | — |
| 17 | `programme-spread.jpg` | 88 KB | — |

No single image exceeds 300 KB. The video is under 3 MB. However:

- **`index.html` total eager payload:** ~1.7 MB (10 marquee images) + 2.1 MB (video) + 133 KB (logo) = **~3.9 MB**
- **`home.html` above-the-fold eager payload:** 7 hero slideshow images (~1.34 MB) + video (2.1 MB) + logo (133 KB) = **~3.6 MB**

### Lazy loading

- **`home.html`:** 15 gallery images correctly have `loading="lazy"`. The 7 hero slideshow images + logo + video are loaded eagerly (above the fold — correct for the first image, but images 2–7 rotate in later and could be deferred).
- **`index.html`:** 0 images have `loading="lazy"`. The 10 marquee images are all eagerly loaded.

### Width/Height attributes

**No `<img>` element in either file has `width` or `height` attributes.** This will cause layout shift (CLS) on every image load.

---

## 3. Functionality

### Enquiry form (`home.html` lines 316–326)

The form has no `action` or `method` attribute. On submit, JavaScript calls `e.preventDefault()`, builds a `mailto:` URL with subject and body from the form fields, and sets `location.href` to it. **This opens the user's default mail application** (Apple Mail, Outlook, etc.) with a pre-filled email.

**Critical problem:** This does **not** work if:
1. The visitor has no mail app configured (common on mobile — many users use Gmail/Outlook in the browser only).
2. The visitor is on an incognito/guest browser profile.
3. The `mailto:` body exceeds ~2000 characters (URL length limits vary by browser/client).
4. The visitor is on a kiosk or shared device.

There is **no fallback, no error handling, and no confirmation** that the email was sent.

### Anchor links

| Anchor | Target ID exists? |
|--------|-------------------|
| `#top` | ✅ `<main id="top">` |
| `#about` | ✅ `<section id="about">` |
| `#services` | ✅ `<section id="services">` |
| `#pricing` | ✅ `<section id="pricing">` |
| `#work` | ✅ `<section id="work">` |
| `#process` | ✅ `<section id="process">` |
| `#faq` | ✅ `<section id="faq">` |
| `#contact` | ✅ `<section id="contact">` |

All anchor links resolve correctly. ✅

### External links

| Link | Well-formed? | Notes |
|------|-------------|-------|
| `https://wa.me/233200968878` | ✅ | Correct Ghana country code (233) + number without leading 0. |
| `tel:+233200968878` | ✅ | Correct E.164 format. |
| `mailto:hello@ndglegacymemorials.com` | ✅ format | But the mailbox may not exist (see issue #2). |
| `https://www.facebook.com/ndgplusmemorials` | ✅ | Verify page exists. |
| `https://www.instagram.com/ndgplusmemorials` | ✅ | Verify page exists. |
| `https://www.tiktok.com/@ndgplusmemorials` | ✅ | Verify page exists. |

### Splash → Home flow (`index.html`)

- **Skip button:** Calls `location.href='home.html'` — works. ✅
- **Escape key:** `document.addEventListener('keydown', e => { if(e.key==='Escape') enter() })` — works. ✅
- **Autoplay failure fallback:** If `v.play()` promise rejects, `useFallback()` is called, which hides the video and shows a CSS fade-in of the logo image, then triggers `finish()` after 3.8s. ✅ Well-implemented.
- **Stalled video fallback:** A 4-second timeout checks if the video is still paused and falls back. ✅
- **Hard timeout:** A 15-second `setTimeout(finish, 15000)` guarantees the "Enter site" button appears. ✅
- **Mobile video:** The video is `muted` and `playsinline` — meets autoplay policy requirements on iOS/Android. ✅
- **Focus management:** After finish, `document.getElementById('go').focus({preventScroll:true})` is called after 1.3s — sends keyboard focus to the "Enter site" button. ✅

---

## 4. SEO and Sharing

| Check | `index.html` | `home.html` |
|-------|-------------|-------------|
| `<title>` | ✅ "NDG+ Legacy Memorials \| Honouring life beautifully" | ✅ "NDG+ Legacy Memorials \| Funeral Branding & Print Studio, Ghana" |
| `<meta name="description">` | ✅ Present | ✅ Present |
| `lang="en"` | ✅ | ✅ |
| `<link rel="canonical">` | ❌ Missing | ❌ Missing |
| Open Graph tags | ❌ Missing | ❌ Missing |
| Twitter Card tags | ❌ Missing | ❌ Missing |
| Favicon | ❌ Missing | ❌ Missing |
| Apple-touch-icon | ❌ Missing | ❌ Missing |
| `robots` meta | ❌ Missing (should be `noindex`) | ❌ Missing |
| JSON-LD | ❌ Missing | ❌ Missing |

### Heading structure

- **`index.html`:** Single `<h1>` — ✅
- **`home.html`:** Single `<h1>`, followed by 9 `<h2>` sections, each with appropriate `<h3>` children — ✅ Well-structured.

### Alt text

- All meaningful images (gallery) have descriptive alt text. ✅
- Decorative images (marquee in `index.html`, hero slideshow backgrounds in `home.html`) have `alt=""`. ✅ Correct.
- The hero logo-reveal `<img>` in `home.html` has `alt=""` — should arguably be `alt="NDG+ Legacy Memorials logo"` since it's not inside an `aria-hidden` container.

### Suggested JSON-LD values

```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "NDG+ Legacy Memorials",
  "description": "Premium funeral branding and print studio in Accra, Ghana",
  "url": "https://ndglegacymemorials.com/",
  "telephone": "+233200968878",
  "email": "hello@ndglegacymemorials.com",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Accra",
    "addressCountry": "GH"
  },
  "priceRange": "GHS 2,500 – GHS 10,000",
  "image": "https://ndglegacymemorials.com/assets/ndg-logo.png",
  "sameAs": [
    "https://www.facebook.com/ndgplusmemorials",
    "https://www.instagram.com/ndgplusmemorials",
    "https://www.tiktok.com/@ndgplusmemorials"
  ]
}
```

---

## 5. Accessibility and Mobile

### Colour contrast (estimated)

| Pair | Foreground | Background | Ratio | Verdict |
|------|-----------|------------|-------|---------|
| Body text on paper | `#2a2d34` | `#fffdf9` | ~14:1 | ✅ AAA |
| Muted text on paper | `#6b6f78` | `#fffdf9` | ~4.7:1 | ✅ AA, ❌ AAA |
| Gold on ink | `#c8a45d` | `#0b0d12` | ~5.9:1 | ✅ AA |
| Cream body on ink (hero) | `#d4d1c8` | `#0b0d12` | ~11.6:1 | ✅ AAA |
| Sub text on black (splash) | `#b9b5aa` | `#000000` | ~7.5:1 | ✅ AAA |
| Nav text on dark header | `#d9d6cd` | `rgba(11,13,18,.95)` | ~11:1 | ✅ AAA |

### Keyboard and focus

- `:focus-visible` outline is styled with `2px solid var(--gold)` on both pages. ✅
- The mobile hamburger button has `aria-label="Open menu"` and toggles `aria-expanded`. ✅
- Form inputs have visible `<label>` elements wrapping them. ✅
- The "Skip intro" button has visible text. ✅
- The "Enter site" link receives focus programmatically after the intro. ✅

### `prefers-reduced-motion`

**Not implemented anywhere.** The following animations run unconditionally:
1. `index.html`: Infinite marquee (`animation: rl … infinite`), fade-in.
2. `home.html`: Infinite typewriter `while(true)` loop, hero background slideshow loop, cursor blink animation.

### Layout at key widths

| Width | Potential issue |
|-------|----------------|
| **360px** | `.trust .wrap` (2-col), `.audience` (2-col), `.steps` (2-col) may cramp. `.row` in the form correctly collapses to 1-col at ≤920px. Footer links with `margin-left: 24px` may wrap poorly. |
| **390px** | Same as 360px, slightly more room. |
| **768px** | Below the 920px breakpoint — single-column grids activate. Should be fine. |
| **1280px** | Above all breakpoints — full 3/4-column layouts. Should be fine. Max content width is 1160px, centered. |

### Safe-area handling

- **`index.html`:** The skip button uses `env(safe-area-inset-top)` — ✅. `viewport-fit=cover` is set — ✅.
- **`home.html`:** No `viewport-fit=cover` in the meta tag, and no `env(safe-area-inset-*)` usage. The sticky header will be behind the iPhone notch/Dynamic Island.

### Horizontal overflow

- `index.html`: `overflow: hidden` on `html, body` — no horizontal scroll possible. ✅
- `home.html`: No overflow protection on `html` or `body`. The `.trust`, `.audience`, and `.steps` grids could cause horizontal overflow on very narrow screens if content is wide. The `.gallery` uses CSS columns which auto-collapse. No explicit `overflow-x: hidden` safeguard.

---

## 6. Content Placeholders

| Item | Location | Status |
|------|----------|--------|
| `hello@ndglegacymemorials.com` | `home.html:308, 344` | ⚠️ May be a placeholder — accompanied by HTML comment `<!-- Confirm your business email address -->` on line 309. Needs a real mailbox. |
| HTML comment: `<!-- Confirm your business email address -->` | `home.html:309` | ⚠️ Developer note shipped to production. Remove. |
| Pricing (GHS 2,500 / 5,000 / 10,000) | `home.html:235-240` | Looks intentional, but confirm these are final prices. |
| Social media links (Facebook, Instagram, TikTok) | `home.html:314, 332` | Verify these profiles exist and are active. |
| No `#` placeholder links found | — | ✅ All links point to real anchors or external URLs. |
| No lorem ipsum or dummy text found | — | ✅ |

---

## 7. Deployment Readiness for Vercel

### Structure

The folder is a **valid static site**:
- `index.html` is at the root — Vercel will serve it at `/`. ✅
- `home.html` will be served at `/home.html` (or `/home` with `cleanUrls`). ✅
- `assets/` is a flat directory of images and one video. ✅

### Recommended `vercel.json`

A `vercel.json` is **recommended but not required**:

```json
{
  "cleanUrls": true,
  "headers": [
    {
      "source": "/assets/(.*)",
      "headers": [
        { "key": "Cache-Control", "value": "public, max-age=2592000, stale-while-revalidate=86400" }
      ]
    }
  ]
}
```

- `cleanUrls: true` allows `/home` instead of `/home.html`.
- 30-day cache for assets (not immutable, since filenames aren't hashed).
- Consider adding `trailingSlash: false` for consistency.

---

## Proposed Fix Order

| Priority | Issues | Action |
|----------|--------|--------|
| **1 — Do first** | #1 | Replace `mailto:` form with a real submission backend (Formspree, serverless function, or WhatsApp API fallback). This is the only way visitors can enquire. |
| **2 — Do first** | #2 | Provision the `hello@ndglegacymemorials.com` mailbox and remove the HTML comment. |
| **3 — Before launch** | #3, #4, #5, #8, #16 | Add favicon, OG/Twitter tags, canonical URL, JSON-LD, and `noindex` on the splash page. |
| **4 — Before launch** | #6 | Add `prefers-reduced-motion` media queries. |
| **5 — Before launch** | #7 | Add `width`/`height` attributes to all images. |
| **6 — Before launch** | #9, #10 | Fix `viewport-fit` and `-webkit-backdrop-filter` in `home.html`. |
| **7 — Polish** | #11, #12, #13, #15 | Add responsive breakpoints for narrow screens (360–480px). |
| **8 — Polish** | #19, #20, #22 | Optimize image sizes, unify font loading, defer non-critical hero images. |
| **9 — Polish** | #23, #25, #26, #27, #28 | Form fallback UX, footer flex spacing, typewriter perf, skip button cleanup, `vercel.json`. |
| **10 — Optional** | #21, #24 | Alt text refinement, AAA contrast for muted text. |
