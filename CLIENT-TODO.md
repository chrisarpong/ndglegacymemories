# Client follow-up

- [ ] Confirm `ndglegacy.com` resolves to the intended Vercel project and HTTPS is active.
- [ ] Verify that `hello@ndglegacy.com` receives mail and is monitored.
- [ ] Confirm the listed pricing is current: Silver GHS 2,500, Gold GHS 5,000, Platinum GHS 10,000, Diaspora from GHS 3,000, partner retainer GHS 1,500/month, rush fees, and deposit terms.
- [ ] Confirm whether the business is ready to restore its Facebook, Instagram and TikTok profile links using the notes below.

## Add back when ready

These links are intentionally absent from `home.html` for now. Each anchor below can be pasted unchanged into both locations:

1. In the contact section, create or use `<div class="social">` after the contact list and paste the anchor there.
2. In the footer, paste the same anchor after the WhatsApp link inside `<div class="foot-contact">`.
3. In the `application/ld+json` script in the `home.html` head, add the URL as an item in a `sameAs` array. If restoring multiple platforms, put all their URLs in that array.

### Facebook

Contact section and footer anchor:

```html
<a href="https://www.facebook.com/ndgplusmemorials" target="_blank" rel="noopener" aria-label="Facebook: NDG+ Legacy Memorials" title="NDG+ Legacy Memorials on Facebook"><svg viewBox="0 0 24 24" width="22" height="22" fill="currentColor" aria-hidden="true"><path d="M14 8h3V4h-3a4 4 0 0 0-4 4v2H7v4h3v8h4v-8h3l1-4h-4V8z"/></svg></a>
```

JSON-LD `sameAs` item:

```json
"https://www.facebook.com/ndgplusmemorials"
```

### Instagram

Contact section and footer anchor:

```html
<a href="https://www.instagram.com/ndgplusmemorials" target="_blank" rel="noopener" aria-label="Instagram: NDG+ Legacy Memorials" title="NDG+ Legacy Memorials on Instagram"><svg viewBox="0 0 24 24" width="22" height="22" fill="currentColor" aria-hidden="true"><rect x="3" y="3" width="18" height="18" rx="5" fill="none" stroke="currentColor" stroke-width="2"/><circle cx="12" cy="12" r="4" fill="none" stroke="currentColor" stroke-width="2"/><circle cx="17.3" cy="6.7" r="1.2"/></svg></a>
```

JSON-LD `sameAs` item:

```json
"https://www.instagram.com/ndgplusmemorials"
```

### TikTok

Contact section and footer anchor:

```html
<a href="https://www.tiktok.com/@ndgplusmemorials" target="_blank" rel="noopener" aria-label="TikTok: NDG+ Legacy Memorials" title="NDG+ Legacy Memorials on TikTok"><svg viewBox="0 0 24 24" width="22" height="22" fill="currentColor" aria-hidden="true"><path d="M16 3c.3 2.4 1.8 4.100 4 4.300v3.300a7.400 7.400 0 0 1-4-1.300V16a6 6 0 1 1-6-6c.3 0 .7 0 1 .1v3.400a2.700 2.700 0 1 0 1.700 2.500V3H16z"/></svg></a>
```

JSON-LD `sameAs` item:

```json
"https://www.tiktok.com/@ndgplusmemorials"
```
