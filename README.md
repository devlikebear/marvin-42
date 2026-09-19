# marvin-42

Workshop promo hub for **devlikebear**: a workshop that ships small tools.
It links to MOOD, insights, tars, linetta, the blog, and GitHub.

It's a static site with no build step and no third-party scripts, fonts, or trackers. The only JavaScript is the pageview script from the self-hosted, cookieless Umami at `analytics.marvin-42.com`; the CSP in `_headers` allows that origin and nothing else.

## Files

| File         | Purpose                                              |
| ------------ | ---------------------------------------------------- |
| `index.html` | Landing page (English, `lang="en"`)                  |
| `styles.css` | Self-contained styles, system font stack, light/dark |
| `_headers`   | Cloudflare Pages security headers (CSP, nosniff, …)  |

## Deploy (Cloudflare Pages)

**Connect this repo in Cloudflare Pages → production branch `main`, framework preset `None`, build command empty, output directory `/` (root).**

The site is served from the repo root, so `README.md` is also publicly reachable at `/README.md` (it's marked `noindex` in `_headers`). It contains nothing sensitive. Never commit secrets to this repo.

## Domains

The apex **marvin-42.com** (same as **www.marvin-42.com**) serves this landing page.

- Add `marvin-42.com` as a custom domain on the Pages project.
- For `www`, pick one of these:
  - Add `www.marvin-42.com` as a second custom domain, **or**
  - Create a Cloudflare **Redirect Rule** (Rules → Redirect Rules → Single Redirect) on the zone:
    - When hostname equals `www.marvin-42.com`
    - Dynamic redirect to `concat("https://marvin-42.com", http.request.uri.path)`, status `301`, preserve query string.

There's no `_redirects` file on purpose. Pages `_redirects` only matches paths, not hostnames, so it can't redirect between www and the apex.

The product subdomains (`mood.`, `insights.`, `tars.`) are separate deployments and aren't part of this repo. MOOD (saju, tarot, zodiac; formerly "saju") is served at `mood.marvin-42.com`; `saju.marvin-42.com` may still resolve in parallel.

## Local preview

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

(`_headers` only takes effect on Cloudflare Pages.)
