# Performance and deployment fixes

## Step 1: Images

- Recompressed the site JPGs as progressive JPEGs and stripped metadata. Gallery images are no larger than 1200 px on the long side; hero images are no larger than 1600 px. All are about 150 KB or smaller.
- Kept quality 78 for most images. Lowered `backdrop.jpg` to 68, `obituary-poster.jpg` to 66, and `obituary-a1.jpg` to 74 to meet the size target. Compared original and optimized contact sheets; no obvious visual differences appeared at the review scale.
- Optimized `ndg-logo.png` losslessly. Its decoded pixels are identical to the original.
- Added 400 px-high thumbnails for the ten splash marquee images and kept the existing full-size filenames for the site gallery and hero.

## Step 2: Video

- Re-encoded the 12-second reveal as H.264, yuv420p, 1280×720, with no audio and the MP4 index at the beginning. It is 928 KiB, down from 2.1 MiB.
- CRF 26 produced a 2.1 MiB file, so CRF 34 was used to meet the size target. Compared frames from seven points in the video; no visible quality drop was apparent at the reviewed size.
- Homebrew could not install FFmpeg because the Mac has not accepted its Xcode license. Used a temporary FFmpeg binary under `/tmp`; no project dependency or system license setting was changed.

## Step 3: Loading behavior

- Added intrinsic width and height attributes to every image in both pages. CSS continues to control rendered sizing; the hero logo explicitly keeps its natural aspect ratio.
- Added asynchronous decoding to non-critical images, lazy loading to splash thumbnails, and deferred the six non-first hero slides until the window load event.
- Both pages now use the same Google Fonts stylesheet URL.

## Step 4: Vercel

- Added `vercel.json` with clean URLs and the requested 30-day asset cache policy plus one-day stale-while-revalidate.
- In a local clean-URL simulation, the splash page's `home.html` navigation resolved to `/home`, direct `/home` loaded, and the configured asset cache header was present. This was not a hosted Vercel deployment.

## Step 5: Verification

- A script checked local `src`, `data-src`, and other asset paths in both HTML files; all resolve. Browser checks found width and height attributes on every rendered image and no horizontal document overflow at 360, 390, 768, or 1280 px.
- Lighthouse 13.5.0 mobile category scores:

  | Page | Performance | Accessibility | Best Practices | SEO |
  |---|---:|---:|---:|---:|
  | Splash (`/`) | 70 | 100 | 100 | 63 |
  | Home (`/home`) | 64 | 96 | 100 | 100 |

- The splash SEO score reflects its intentional `noindex, follow` directive. Home's remaining accessibility score is 96; its page still has a contrast audit finding. No colors were changed because the client requested the original visual design.

## Referenced page weight

Approximate total of each HTML file plus its unique local image and video files referenced by `src` or `data-src`. The totals include gallery images that load lazily and slides that load after `window.load`; external Google Fonts and response headers are excluded. Original figures use the repository baseline before this performance pass.

| Page | Before | After | Reduction |
|---|---:|---:|---:|
| Splash | 4,016 KiB | 1,305 KiB | 67.5% |
| Home | 4,824 KiB | 2,565 KiB | 46.8% |

No deployment was performed.
