# marvin-42

The apex **marvin-42.com** no longer hosts a page. It permanently redirects (301) every path to **https://devlikebear.com/projects**, which now lists MOOD, tars, linetta, and the rest.

The redirect lives in `_redirects`. `index.html` is only a fallback with a meta refresh and a link, in case the redirect is ever skipped. There's no JavaScript and no analytics. The CSP in `_headers` allows nothing to load.

## Files

| File         | Purpose                                                  |
| ------------ | -------------------------------------------------------- |
| `_redirects` | `/*` → `https://devlikebear.com/projects` (301)          |
| `index.html` | Fallback page that points to /projects (`noindex`)       |
| `_headers`   | Cloudflare Pages security headers (CSP, nosniff, …)      |

## Deploy (Cloudflare Pages)

**Connect this repo in Cloudflare Pages → production branch `main`, framework preset `None`, build command empty, output directory `/` (root).**

The site is served from the repo root. `/README.md` is caught by the same `/*` redirect, and it's also marked `noindex` in `_headers` as a fallback. Never commit secrets to this repo.

## Domains

The apex **marvin-42.com** (same as **www.marvin-42.com**) is served by this project, which redirects to devlikebear.com/projects.

- Add `marvin-42.com` as a custom domain on the Pages project.
- For `www`, pick one of these:
  - Add `www.marvin-42.com` as a second custom domain, **or**
  - Create a Cloudflare **Redirect Rule** (Rules → Redirect Rules → Single Redirect) on the zone:
    - When hostname equals `www.marvin-42.com`
    - Dynamic redirect to `concat("https://marvin-42.com", http.request.uri.path)`, status `301`, preserve query string.

Pages `_redirects` only matches paths, not hostnames. If `www` is a second custom domain on this project, it gets the same /projects redirect. A zone Redirect Rule to the apex also works: the apex then redirects again.

The product subdomains (`mood.`, `tars.`, `linetta.`) are separate deployments and aren't part of this repo. MOOD (saju, tarot, zodiac; formerly "saju") is served at `mood.marvin-42.com`; `saju.marvin-42.com` may still resolve in parallel.

## Local preview

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

(`_headers` only takes effect on Cloudflare Pages.)
