# NDG+ Legacy Memorials

NDG+ Legacy Memorials is a two-page marketing and enquiry website for a funeral branding and print studio in Accra, Ghana. It introduces the brand, describes print and memorial services, publishes packages and pricing, shows sample work, and helps visitors contact the studio.

## Business and project ownership

The website presents **NDG+ Legacy Memorials** as the funeral and memorial branding division of **NDG+**. The business is the apparent client and intended operator of the public site. The checked-in project files do not identify an individual legal owner, a named client representative, or who should legally hold the domain registration. Confirm those details with the client before purchase. For continuity, register the domain in an account controlled by the client/business and give the developer delegated access for setup; do not make the developer's personal account the only owner.

The site lists Accra, Ghana and **020 096 8878** (`+233 200 968 878`) as the public location and phone/WhatsApp contact.

## What visitors can do

- View a branded splash screen and enter the main business site.
- Browse the studio's services, including core funeral print, banners, apparel, digital tributes, online memorials, and event branding.
- Review Silver, Gold and Platinum packages, specialist programmes, turnaround terms and payment terms.
- View fifteen portfolio examples and open any example at full size.
- Read the order process and FAQs.
- Submit an enquiry form that validates in the browser, prepares a message and opens WhatsApp. The visitor must still send the message in WhatsApp. If opening WhatsApp is blocked, the page offers retry, copy and call options.
- Contact the studio through direct WhatsApp, telephone and email. Social links are temporarily removed and can be restored using [CLIENT-TODO.md](CLIENT-TODO.md).

The displayed packages are Silver **GHS 2,500**, Gold **GHS 5,000**, Platinum **GHS 10,000**, diaspora coordination from **GHS 3,000**, and a partner retainer of **GHS 1,500/month**. The page also states standard 3–5 business day turnaround, 24–48 hour priority service (+20%), same-day/overnight service (+30%), and a 70% production deposit. These are published business claims; the client should confirm they are current before launch.

## Pages and routes

| File | Production route | Purpose |
| --- | --- | --- |
| `index.html` | `/` | Video logo reveal, portfolio marquee, entry link and WhatsApp shortcut. It is marked `noindex` and canonicals to the main page. |
| `home.html` | `/home` on Vercel | Main business site with services, pricing, gallery, FAQs and enquiry form. |

Vercel's `cleanUrls` setting maps `home.html` to `/home`. A basic local static server does not apply that rewrite, so use `/home.html` when previewing locally.

## Technology and runtime

- Plain HTML, embedded CSS and browser JavaScript.
- No framework, package manifest, install step, compile/build step, application server, database or server-side form handler.
- Vercel serves the files and assets as static content. `vercel.json` enables clean URLs and sets cache headers for `/assets/*`.
- Google Fonts supplies Cormorant Garamond and Manrope. System font fallbacks are configured if Google Fonts is unavailable.
- The site has no analytics or payment integration. The enquiry text is assembled client-side; it is not saved or sent by the website.
- The splash intro has a static logo fallback, a skip button, an Escape shortcut, a timeout, and reduced-motion behavior. The main page also supports reduced motion and a mobile navigation menu.

## Project structure

```text
index.html                 Splash page
home.html                  Main business page
assets/                    Logo, reveal video, portfolio images, icons and share image
assets/thumbs/             Small images for the splash-page marquee
.vercelignore              Excludes private project Markdown files from deployments
vercel.json                Clean URL behavior and asset cache headers
README.md                  Project overview, local run and launch instructions
PROJECT-DETAIL.md          Detailed page, content, technical and operations reference
CLIENT-TODO.md             Items that require client confirmation
AUDIT.md                   Historical audit notes from before the hardening work
FIXES.md                   Optimization, implementation and verification record
```

## Run locally

From the project directory, start Python's built-in static server:

```sh
python3 -m http.server 8000
```

Open <http://localhost:8000/> for the splash page and <http://localhost:8000/home.html> for the main page. Stop the server with `Ctrl+C`. Alternatively, open `index.html` directly, though using HTTP is closer to the deployed environment.

There is no build or dependency installation command. Do not expect `/home` to work on Python's basic server; that clean route is provided by Vercel.

## Hosting and deployment

The repository is set up for static hosting on Vercel. The existing workflow uses the GitHub `main` branch as the production branch. Connect the repository to the intended Vercel project, select `main` for production, and leave the build command and output directory empty/default for this plain static site. A successful push alone does not establish that the production deployment is ready; check the Vercel deployment and open the production URL.

### Custom domain setup

The configured domain is **`ndglegacy.com`**. Both pages use it in canonical, social-sharing and structured-data URLs. The public contact email is **`info@ndglegacy.com`**. Social profile links are intentionally absent until the client is ready to add them; [CLIENT-TODO.md](CLIENT-TODO.md) has the exact snippets and insertion points.

1. Confirm `ndglegacy.com` is registered to the client/business and the domain account is under their control.
2. Turn on renewal reminders or auto-renew and protect the registrar account with two-factor authentication.
3. Connect the GitHub repository to the client's Vercel team/project and confirm the production deployment is working on its Vercel URL.
4. In Vercel project **Settings → Domains**, add the approved domain and its `www` version if desired. Follow the DNS records Vercel displays at the registrar; do not copy generic records if Vercel shows project-specific values.
5. Choose one canonical hostname (apex or `www`) and redirect the other to it.
6. Confirm canonical URLs, `og:url`, social image URLs, and the LocalBusiness JSON-LD use `https://ndglegacy.com`.
7. Wait for Vercel to verify DNS and issue HTTPS, then open `/`, `/home`, portfolio images and the WhatsApp links on the live domain.

See Vercel's [custom-domain setup guide](https://vercel.com/docs/domains/set-up-custom-domain) for current DNS verification and certificate steps. DNS settings belong to the registrar/DNS account; the site itself does not include email hosting. Confirm that `info@ndglegacy.com` has a real, monitored mailbox.

## Launch checklist

Before calling the site production-ready, the client should confirm:

- That `ndglegacy.com` and the Vercel project are controlled by the client/business.
- Published package prices, rush fees, deposits, retainer, delivery and turnaround terms.
- That `info@ndglegacy.com` exists and is monitored.
- Which Facebook, Instagram and TikTok profiles to add back; use the snippets in `CLIENT-TODO.md`.
- That the WhatsApp number is the correct business contact and receives enquiries.
- That the final production domain loads securely, the main route works at `/home`, metadata points to that domain, and the enquiry flow opens the expected WhatsApp chat.

The site does not accept online payments, store form submissions, email enquiries automatically or send a WhatsApp message without the visitor's action. If the client needs these capabilities, they require a separate implementation and service setup.

## Related documents

- [PROJECT-DETAIL.md](PROJECT-DETAIL.md) — expanded content, UX, technical architecture, SEO and privacy details.
- [CLIENT-TODO.md](CLIENT-TODO.md) — client-owned launch decisions and confirmations.
- [AUDIT.md](AUDIT.md) — historical audit record; read alongside the newer implementation record.
- [FIXES.md](FIXES.md) — performance changes and prior verification notes.
