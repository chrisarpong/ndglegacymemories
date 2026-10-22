# NDG+ Legacy Memorials

A static, two-page website for NDG+ Legacy Memorials, a funeral branding and print studio in Accra, Ghana. The site presents the studio, services, packages, portfolio and contact options, with a WhatsApp-first enquiry flow.

## Pages

- **Splash page (`/`)** — branded logo reveal, sample-work marquee and the “Explore our work” link to the main site. The video has a logo-image fallback, a Skip intro button and an Escape-key shortcut.
- **Main site (`/home`)** — long-form business site with service information, prices, sample work, process, FAQs and contact form.

See [PROJECT-DETAIL.md](PROJECT-DETAIL.md) for the page structure, interactions, assets, technical design, operational notes and outstanding client decisions.

## Technology

- Plain HTML, CSS and browser JavaScript; no framework, build step, package manifest or project runtime dependencies.
- Static assets are served from `assets/`.
- Google Fonts supplies Cormorant Garamond and Manrope; local system fonts are configured as fallbacks.
- `vercel.json` enables clean URLs and applies a cache policy to `/assets/*`.

## Preview locally

Open `index.html` in a browser, or start a simple static HTTP server from the project directory:

```sh
python3 -m http.server 8000
```

Then visit <http://localhost:8000/>. The splash links to `home.html`; with Vercel's `cleanUrls` enabled, the main page is also available at `/home`.

## Deploying

The repository is configured for Vercel static hosting. Connect the GitHub repository to Vercel and use `main` as the production branch. Push production changes to `main`; pushes to other branches can create preview deployments. No build command or output directory is needed for this static site.

The canonical and social metadata currently use `https://ndglegacymemorials.com` as the intended domain. Confirm domain ownership and configure it in Vercel before treating that URL as the live production address. See [CLIENT-TODO.md](CLIENT-TODO.md).

## Project files

```text
index.html          Splash page
home.html           Main business site
assets/             Logo, video, portfolio and optimized images
assets/thumbs/      Small images used by the splash marquee
vercel.json         Clean URLs and static asset cache headers
AUDIT.md            Original issue list and audit findings
FIXES.md            Implemented fixes and performance notes
CLIENT-TODO.md      Items requiring client confirmation
PROJECT-DETAIL.md   Detailed project documentation
```

## Contact flow

The main page form validates required fields in the browser and prepares an enquiry message for `wa.me`. It does not submit data to a site server or send a message automatically. Visitors can retry WhatsApp, copy the prepared message, or call. The visible email address is a `mailto:` link; confirm that mailbox is provisioned and monitored.
