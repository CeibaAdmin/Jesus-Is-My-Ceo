# Jesus Is My CEO

Landing page for **Jesus Is My CEO** — a 10-week in-person lifegroup on faith-driven
leadership and stewardship, running as part of Church by the Glades' fall lifegroup
semester. It's for Christian leaders of every kind — business owners, managers, ministry
and nonprofit leaders, and anyone growing into leading well — and works through an
eight-session video study.

The page explains the discipleship idea (Jesus holds authority over everything we lead;
we operate as stewards) and invites people to keep the conversation going through the
**Questions of the Week** section.

> **Affiliation note.** The group *uses* the Faith Driven Entrepreneur video study and
> guide as a resource, but is **not** registered with, affiliated with, or a group of
> Faith Driven Entrepreneur. It's an independent lifegroup of Church by the Glades. Keep
> any FDE mention to a plain material credit — never language implying membership,
> registration, or an organizational relationship.

### Questions of the Week

Each session closes with a couple of questions. Members write their answers down; the
facilitators post the questions in the `#questions` section. Anyone can submit an answer
through an external **Google Form** (link-out button), the facilitators **review every
submission**, and approved answers are posted on the page as `<details>` blocks — the same
accordion pattern used by the FAQ. There is no automatic publishing: nothing appears until
a facilitator adds it to `index.html`.

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

Already supplied: the mission-section group photo, both facilitator headshots and bios,
the contact email and phone, the "no cost to join" answer, and the Questions of the Week
Google Form link. Still outstanding:

| Placeholder | Where | What it needs |
| --- | --- | --- |
| `· [Business Name]` | Leaders, `<p class="leader__role">` | Ceiba Accounting and The Balanced Coach are the two facilitators' businesses, but which belongs to Kelly and which to Alex is still unconfirmed — see the HTML comment right above the leader cards |
| `[DOMAIN]` | `<head>` | Needed for an absolute `og:image`, plus `og:url` and a canonical link. Not visible to visitors, so not urgent. |

Still missing entirely (no placeholder in the file — this needs a real meeting day, time,
and location added to the hero logistics strip and the contact section's closing
sentence, which currently just say "Details coming soon" / "reach out for the exact date
and location").

Two things have alternates already written into the code as HTML comments, ready to
swap:

- **Hero tagline** — currently *"Lead the way God intended."* Two other
  options sit in a comment directly above it.
- **Anchor scripture** — currently Psalm 24:1, which lines up with the curriculum's
  Session 2 ("God Owns My Business"). 1 Corinthians 4:2, Luke 16:10–12, and the guide's
  own two verses (2 Timothy 2:2 and Hebrews 10:24–25) are in a comment beside it.

## Curriculum attribution

The eight session titles on the page come from the Faith Driven Entrepreneur video study,
taught by Henry Kaestner and J.D. Greear. **The one-line descriptions under each title are
ours, not theirs** — the guide's own discussion questions are their copyrighted material
and are deliberately not reproduced on the page.

The page credits FDE by name only as the source of the study material, in the curriculum
section. The same section states plainly that the group is **independent and not
affiliated with FDE**. Keep it that way: a material credit is fine; anything implying
membership, registration, or an organizational tie is not.

## Images

Two directories, with different jobs:

| Path | Contents | Deployed? |
| --- | --- | --- |
| `images/` | Optimized WebP the page actually loads, plus `og-cover.jpg` | Yes |
| `assets/source/` | Full-resolution PNG masters, kept for future re-cropping | **No** — excluded by `.vercelignore` |

Serving the multi-megabyte originals would have made the page unusable on a phone, so
nothing in `assets/source/` is ever deployed.

Sizes were chosen for the widest case each image actually renders at, doubled for
high-DPI screens:

- `images/leaders/*.webp` — facilitator headshots
- `images/group-meeting.webp` — 1120×896 (5:4 native)
- `images/og-cover.jpg` — 1200×630, cropped from the group photo and biased upward so
  the standing figures' heads aren't cut off. JPEG rather than WebP because some social
  crawlers still don't decode WebP.

### Re-encoding after you add or replace a master

There is no `cwebp`, ImageMagick, `vips`, Pillow, or `sharp` in this project — encoding
runs through headless Chromium's canvas, which produces WebP natively and needs nothing
installed. It's a one-time operation per image, not a build step, so no script is
committed. Ask Claude to re-encode and it will regenerate from `assets/source/`.

If you replace an image yourself, keep the `width`/`height` attributes on the `<img>` in
step with the new file. They're what keeps the page from shifting as images load
(verified: card heights are identical before and after images finish loading).

Every `<img>` also carries `loading="lazy"`, `decoding="async"`, and descriptive alt text.

## Design notes

Colors are CSS custom properties at the top of the `<style>` block, so the palette can
be retuned in one place:

```
--navy   #0B1F3A    --gold   #B8964A    --cream  #F7F4EE
--slate  #5A6472    --gold-ink #836826
```

There are deliberately two golds. `--gold` is used on navy backgrounds;
`--gold-ink` is the same brass darkened for use on white and cream, because the
lighter gold only reaches 2.8:1 there — below the accessible minimum. If you retune
the palette, keep both and check contrast in each direction.

The layout is mobile-first and fluid — type scales with `clamp()`, so there are only two
breakpoints doing real work (roughly 620px and 900–1000px). Scroll fade-ins and the
sticky-nav shadow are progressive enhancement: with JavaScript disabled the page still
renders completely, and animations are suppressed automatically for anyone with
"reduce motion" enabled.

**There is no signup form on the page, by design.** Members are recruited through Church by
the Glades and added by the facilitators, so every "join" path points to email, phone, or
the church. The one form in play — the **Questions of the Week** answer form — lives on
Google Forms and is reached by an external link-out, not embedded, so the page hosts no
form of its own.

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

`.vercelignore` keeps `assets/source/` out of the deployment, so the 21 MB of masters
never reach the CDN.

Three things to know about the CSP if you extend the page later:

- It allows inline `<style>` and `<script>` (the page needs both) but blocks scripts and
  styles loaded from other domains. If you add an analytics snippet or a font from a CDN,
  add that host to `script-src` / `style-src` / `font-src` or it will be blocked.
- Images are limited to `'self'` and `data:` URIs. Committing photos to this repo works
  as-is; hot-linking images from another domain needs that host added to `img-src`.
- `form-action 'none'` matches the no-on-page-form design. The Questions of the Week answer
  form is an external link-out to Google Forms (a plain `<a>`), which navigation CSP doesn't
  restrict — so it needs no change. Only an *embedded* form or an on-page `<form>` that POSTs
  would require touching `form-action` (and `frame-src` for an iframe embed).

## Other hosts

Nothing here is Vercel-specific except `vercel.json`, which other platforms ignore.
The same files work on Netlify, Cloudflare Pages, or GitHub Pages — connect the repo
with no build command and the repo root as the publish directory. You'd need to
re-create the headers in that platform's own config to keep them.
