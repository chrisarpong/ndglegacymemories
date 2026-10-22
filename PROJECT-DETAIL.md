# Project detail: NDG+ Legacy Memorials

## 1. Project overview

NDG+ Legacy Memorials is a static marketing and enquiry website for a funeral branding and print studio based in Accra, Ghana. It presents design and print services for funeral and memorial events, explains packages and turnaround terms, shows sample work, and gives families several ways to contact the studio.

The experience is intentionally split into two pages:

1. A cinematic splash page introduces the identity and offers a clear route into the site.
2. A detailed business page explains the studio, audience, services, prices, examples, process, FAQs and contact options.

The site is built with plain HTML, CSS and browser JavaScript. It has no application server, framework, build pipeline, package manifest or site-specific runtime dependencies. Vercel serves the files as static content.

## 2. Business and audience

The site describes NDG+ Legacy Memorials as the funeral branding division of NDG+. Its value proposition is a coordinated design-to-print service delivered with care, quality and speed. The public contact location is Accra, Ghana; the phone and WhatsApp number is **020 096 8878** (`+233 200 968 878`).

The intended audiences listed on the site are:

- Bereaved families planning a funeral or remembrance.
- Churches, funeral homes and event planners seeking a recurring print partner.
- Families abroad coordinating local design, production and delivery.
- Organisations and associations arranging collective tributes.
- Individuals ordering birthday or anniversary keepsakes.

## 3. Page structure and content

### Splash page — `index.html` / `/`

The splash page uses a full-screen black stage, a logo-reveal video, a moving strip of portfolio thumbnails, the heading “Honouring lives. Preserving legacies.” and the **Explore our work** link. A WhatsApp help link remains visible below the main link. The splash is marked `noindex, follow`; its canonical URL points to `/home` so search indexing is focused on the substantive business page.

The intro can finish when the video ends or after a timeout. If video playback fails or autoplay is blocked, JavaScript hides the video and reveals the static logo image. Visitors can skip the intro with the Skip intro button or the Escape key. When the reduced-motion preference is enabled, the logo fallback is used and the entry content is shown without the normal animation sequence. A short-landscape media query reduces the band and button spacing so the main link remains reachable.

The portfolio band contains ten small images from `assets/thumbs/`. JavaScript clones the image elements to create a continuous marquee. The band is decorative to assistive technology.

### Main site — `home.html` / `/home`

The home page follows this content order:

1. Sticky navigation and hero with a rotating set of work images.
2. Four trust statements for turnaround, design review, production and diaspora coordination.
3. About the studio and its service principles.
4. The families, organisations and partners the studio serves.
5. Six service groups: core print, banners and displays, apparel and merchandise, digital tributes, online memorials, and event branding.
6. Silver, Gold and Platinum packages, followed by turnaround, approval and payment terms.
7. Diaspora coordination, partner retainers, remembrance tributes and referral information.
8. A portfolio gallery of fifteen examples. Selecting an example opens its full image in a new tab.
9. A four-step explanation of the order process.
10. Frequently asked questions.
11. Contact details, social profiles and the enquiry form.
12. Footer navigation, contact shortcuts and copyright line.

The pricing currently displayed includes Silver at GHS 2,500, Gold at GHS 5,000, Platinum at GHS 10,000, diaspora support from GHS 3,000, and a partner retainer of GHS 1,500 per month. The site also states standard and rush turnaround rates and a 70% production deposit. These are published business claims and remain subject to client confirmation.

## 4. Enquiry and contact behavior

The form asks for name, phone, email, service and project details. Name, phone, service and details are required; email is optional. Required fields receive inline validation messages and an invalid state, with no browser alert dialog.

On valid submission, the page formats the answers into a readable message and opens WhatsApp at `https://wa.me/233200968878` with the message URL-encoded. If the browser blocks the new tab, the page navigates to the WhatsApp URL directly. The page then replaces the form with an inline confirmation panel. The panel lets the visitor open WhatsApp again, copy the prepared message (using the Clipboard API and a textarea/`execCommand` fallback), or call the studio.

This is a client-side handoff, not an online form backend. The website does not persist submissions, deliver email, or send WhatsApp messages automatically. The visitor must complete the send action in WhatsApp. The email in the contact section remains a regular `mailto:` link and does not participate in the form flow.

Other contact links include direct WhatsApp, telephone, email and Facebook, Instagram and TikTok profiles. The business should confirm the mailbox and social profiles before relying on them publicly.

## 5. Design and responsive behavior

The site preserves the client's dark, black-and-gold visual identity. The main page pairs a near-black/navy base with warm gold accents, cream section backgrounds and a warm off-white reading surface. Headings use Cormorant Garamond; body and interface text use Manrope. The splash page uses the same font request. If Google Fonts cannot load, the CSS falls back to Georgia and system sans-serif fonts.

The main layout is centered in a maximum-width content wrapper. Its grids are multi-column on wide screens, reduce at the tablet breakpoint, and collapse to one column for the trust strip, audience list and process steps below about 480 pixels. Navigation changes to a toggleable mobile menu. Safe-area padding is applied to the sticky header and footer. Focus-visible outlines, semantic headings, form labels and meaningful portfolio alt text support keyboard and assistive-technology use.

Both pages include reduced-motion handling. The splash stops animated transitions and marquee movement; the main page suppresses CSS transitions and animation, while its JavaScript uses static presentation for the text and slideshow. These settings respect the visitor's operating-system preference.

## 6. Technical architecture

### Runtime and files

- `index.html` and `home.html` are the complete application pages.
- CSS is embedded in each HTML file; page interactions are small inline JavaScript blocks.
- `assets/` contains image, icon, logo and video files. No server-side code is present.
- No bundling, transpilation, framework runtime or package installation is required.
- Google Fonts is the only external font delivery service referenced by the pages. If unavailable, local font stacks are used.

### Image and video loading

The portfolio JPGs were resized and recompressed as progressive JPEGs with metadata stripped. Hero/backdrop images were limited to a 1600-pixel long side and gallery images to 1200 pixels. Ten roughly 400-pixel-tall derivatives are used by the splash marquee. The logo PNG was optimized losslessly. Image elements include intrinsic width and height so the browser can reserve their layout space; CSS controls their displayed size while preserving aspect ratio.

Gallery images load lazily and decode asynchronously. The splash thumbnails are lazy and asynchronously decoded. The main page loads its first hero slide initially and defers the other slides until the page load event. The logo-reveal video was converted to H.264/yuv420p, capped at 1280×720, stripped of audio and prepared for progressive playback. The recorded optimized file is about 928 KiB.

The historical performance notes in `FIXES.md` record approximate referenced local payload totals of 1,305 KiB for the splash and 2,565 KiB for the main page after optimization. They also record mobile Lighthouse scores from that review; scores can change with browser, network, Vercel response and Lighthouse versions.

### SEO and sharing

The main page has a title, description, canonical URL, Open Graph and Twitter large-image metadata, and LocalBusiness JSON-LD built from information already presented on the page. It uses `https://ndglegacymemorials.com/home` as the canonical. The splash is noindex and points its canonical to the main page. Both pages link the generated favicon, large favicon, Apple touch icon and 1200×630 sharing image.

The domain is a placeholder until the client purchases/configures the domain. Before launch, update the canonical, metadata, structured-data URL and client documentation together. `CLIENT-TODO.md` lists the known occurrences and client approvals.

## 7. Vercel hosting

`vercel.json` sets `cleanUrls` to `true`, allowing `home.html` to be served at `/home`, and configures `/assets/*` with `Cache-Control: public, max-age=2592000, stale-while-revalidate=86400`. The splash link points to `home.html`; Vercel's clean URL behavior serves the destination at `/home`.

The project is a static deployment: there is no build command or output directory to configure. GitHub's `main` branch is the production branch. The repository should be connected to the intended Vercel project, and successful production deployment status should be checked in Vercel after changes are pushed. Branches other than `main` may produce preview deployments, which do not replace the production deployment until promoted or merged to `main`.

## 8. Privacy, reliability and operational boundaries

The form data is processed in the visitor's browser to construct a WhatsApp link. This site does not include a database, account system, payment processing, analytics package or server-side submission handler. Once the visitor opens WhatsApp, message delivery and data handling are controlled by WhatsApp and the visitor's device/account.

The splash video is not required to navigate: the image fallback, Skip intro, Escape shortcut and final timeout provide alternate paths. The main navigation and form are usable without a build-time JavaScript framework, but the WhatsApp form, mobile menu and animated presentation depend on browser JavaScript.

## 9. Remaining client decisions

See [CLIENT-TODO.md](CLIENT-TODO.md) for the tracked checklist. Key items are:

- Confirm the production domain and configure it in Vercel; replace the placeholder URL consistently.
- Provision and monitor `hello@ndglegacymemorials.com`.
- Confirm all package prices, fees, deposits and retainer details.
- Confirm ownership and activity of the linked Facebook, Instagram and TikTok profiles.

## 10. Related documentation

- [README.md](README.md) — quick start, project layout and deployment summary.
- [AUDIT.md](AUDIT.md) — original audit issues and findings.
- [FIXES.md](FIXES.md) — implementation and performance-change record.
- [CLIENT-TODO.md](CLIENT-TODO.md) — follow-up actions requiring client input.
