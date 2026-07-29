# Jesus Is My CEO

Landing page for **Jesus Is My CEO** — a lifegroup for Christian business owners and
entrepreneurs, connected through Church by the Glades. The page serves two purposes at
once: it explains the discipleship side (Jesus holds authority over our businesses; we
operate as stewards) and it showcases the group's function as a trusted referral network
of Christian-owned businesses.

## Viewing it

Open `index.html` in any browser. That's it — double-click the file or drag it into a
window. There is no build step, no install, and no server required.

The entire page is one self-contained file: HTML, CSS, and JavaScript inline, system
fonts only, zero external requests. It works offline.

## Filling in the real content

Everything still pending is wrapped in `[SQUARE BRACKETS]`. To list every placeholder
left in the file:

```sh
grep -o '\[[^]]*\]' index.html | sort -u
```

What's waiting on real copy:

| Placeholder | Where | What it needs |
| --- | --- | --- |
| `[DAY/TIME]`, `[LOCATION]` | Hero logistics strip, contact section | Meeting day, time, and place |
| `[PHOTO — GROUP AT THE TABLE]` | Mission section | Replace the `div.ph` with an `<img>` |
| `[HEADSHOT — KELLY]`, `[HEADSHOT — ALEX]` | Leaders | Professional headshots |
| `[KELLY BIO]`, `[ALEX BIO]` | Leaders | Bio copy (replace the `span.ph-text`) |
| `[TITLE — BUSINESS]` | Leaders | Each leader's role and company |
| Directory cards | Business Directory | All 8 entries are **sample data** — swap in real member businesses and remove the `[Sample entries]` badge |
| `[15 MIN]` etc. | How It Works | Real segment timings, or delete the `span.chip` elements |
| `[COST/DUES POLICY]` | FAQ | Dues answer |
| `[ANY ADDITIONAL CRITERIA]`, `[ANY DIRECTORY GUIDELINES]` | FAQ | Optional extra detail |
| `[EMAIL@DOMAIN.COM]`, `[(555) 000-0000]` | Contact | Update both the visible text **and** the `mailto:` / `tel:` `href` |
| `[NEXT MEETING DATE]` | Contact | Next gathering date |
| `[OG IMAGE URL]` | `<head>` | 1200×630 social preview image |

Two things have alternates already written into the code as HTML comments, ready to
swap:

- **Hero tagline** — currently *"Lead your business the way God intended."* Two other
  options sit in a comment directly above it.
- **Anchor scripture** — currently Psalm 24:1. 1 Corinthians 4:2 and Luke 16:10–12 are
  in a comment beside it.

## Design notes

Colors are CSS custom properties at the top of the `<style>` block, so the palette can
be retuned in one place:

```
--navy   #0B1F3A    --gold   #B8964A    --cream  #F7F4EE
```

The layout is mobile-first and fluid — type scales with `clamp()`, so there are only two
breakpoints doing real work (roughly 620px and 900–1000px). Scroll fade-ins and the
sticky-nav shadow are progressive enhancement: with JavaScript disabled the page still
renders completely, and animations are suppressed automatically for anyone with
"reduce motion" enabled.

**There is no signup form anywhere, by design.** Members are recruited through Church by
the Glades and added by the leaders, so every "join" path points to email, phone, or the
church.

## Branches

Two branches, and only two:

| Branch | Role |
| --- | --- |
| `main` | Production. What Vercel deploys to the live domain. Never commit here directly. |
| `dev` | Where the work happens. Every change lands here first and gets a Vercel preview URL. |

Normal workflow:

```sh
git checkout dev
# ...make changes...
git add -A && git commit -m "Update leader bios"
git push origin dev          # Vercel builds a preview URL for this commit
```

When the preview looks right, promote it to production:

```sh
git checkout main
git merge --ff-only dev
git push origin main         # Vercel deploys to the live domain
git checkout dev             # go back to dev for the next change
```

`--ff-only` keeps history linear and fails loudly if `main` has drifted, which it
shouldn't if nobody commits there directly.

## Deploying to Vercel

The site is static — no build step, no dependencies, no server. First-time setup:

1. In Vercel, **Add New → Project** and import this repository.
2. Framework Preset: **Other**. Leave Build Command empty and Output Directory as the
   repo root. There is nothing to install or compile.
3. Under **Settings → Git**, confirm the Production Branch is **`main`**.
4. Deploy.

After that it's automatic: pushes to `main` deploy to production, pushes to `dev` get
their own preview URL. To add a custom domain, use **Settings → Domains**.

To preview a production build locally, the Vercel CLI reads `vercel.json` the same way
the platform does:

```sh
npx vercel dev
```

### What `vercel.json` does

- **Security headers** on every response: `nosniff`, a `Referrer-Policy`, a
  `Permissions-Policy` denying camera/mic/geolocation, and a Content Security Policy.
- **`Cache-Control: max-age=0, must-revalidate`** on the HTML, so edits to bracketed
  copy appear immediately instead of sitting in a CDN cache.
- **`cleanUrls`** so `/index.html` also serves at `/`.

Two things to know about the CSP if you extend the page later:

- It allows inline `<style>` and `<script>` (the page needs both) but blocks scripts and
  styles loaded from other domains. If you add an analytics snippet or a font from a CDN,
  add that host to `script-src` / `style-src` / `font-src` or it will be blocked.
- Images are limited to `'self'` and `data:` URIs. Committing photos to this repo works
  as-is; hot-linking images from another domain needs that host added to `img-src`.
- `form-action 'none'` matches the no-signup-form design. If a form is ever added, that
  directive has to change too.

## Other hosts

Nothing here is Vercel-specific except `vercel.json`, which other platforms ignore.
The same files work on Netlify, Cloudflare Pages, or GitHub Pages — connect the repo
with no build command and the repo root as the publish directory. You'd need to
re-create the headers in that platform's own config to keep them.
