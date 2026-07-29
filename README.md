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

## Hosting

Any static host works. Two easy options:

- **GitHub Pages** — Settings → Pages → deploy from branch, root folder. `index.html`
  is served automatically.
- **Netlify / Cloudflare Pages** — drag the folder into the dashboard, or connect the
  repo with no build command and the repo root as the publish directory.
