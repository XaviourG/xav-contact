# xav-contact

Xaviour's personal link and contact card. One static page, hosted free on GitHub Pages.
No build step, no framework, no third-party requests.

Live: **https://xav.contact**

## What's in the box

```
index.html     The whole site: HTML + inline CSS + four small inline scripts
xaviour.vcf    Static vCard the "Add contact" button shares / downloads
xaviour-sydney.jpg  Hero photo (1400x577, EXIF/GPS stripped)
og.jpg         1200x630 link-preview image
favicon.svg    X monogram on the dark stage
fonts/         Anton, Geist + Geist Mono woff2, vendored (OFL). See fonts/SOURCE.md
.nojekyll      Tells GitHub Pages to serve files verbatim (no Jekyll)
robots.txt     Allows all crawlers, names the AI crawlers explicitly, points at the sitemap
sitemap.xml    The page and llms.txt, with the hero image
llms.txt       Plain-text profile for AI assistants (llmstxt.org format)
```

## Design

The page follows the Vectorise design language (`DESIGN.md` in
`vectorise-labs/vectorise-labs-marketing-site`): black page `#0d0d0d`, square corners, hairline
rules instead of boxes, Anton for the name, Geist bold for sentences, Geist Mono caps for every
label, blue for the answer and orange only for the problem. Section markers (blue bar, label,
hairline) open each section below the fold. The only Vectorise branding is the title line under
the name and a large, faint Vectorise mark behind the first screen.

The first screen (`.fold`) always fills the viewport (`100svh`) and holds three tiers: the photo,
name and title line; the actions (LinkedIn as a single row, then the roadmap panel, "Work in
software?") sitting low in thumb reach; and a grey "More" prompt under a hairline. Short screens
(720px and 600px tall) tighten spacing and the roadmap drawing so the fold still fits one screen.

The faint mark (`.mark-bg`) is a solid `#131313`, not an alpha colour, so overlapping strokes stay
one tone. It is sized from the screen width (about 7.5% bleeds off each side on a phone), starts
rotated 7deg, and on scroll moves at a quarter of the page's speed and turns to 45deg (the mark is
eight-fold symmetric, so that is aligned again). `.fold-main` clips it at the hairline above "More".
The effect is off under reduced motion.

Below the fold, in order: Connect (Contact card, Blog, X, Instagram, WhatsApp, Facebook, then
Let's talk as a full-width row to the Cal.com booking page), About with a fact list, the QR code
under "Scan", and a footer with the Share button.

## SEO and AI search

- `<head>` carries the description, canonical URL, Open Graph and X card tags, `rel="me"` links
  to each social profile, and a JSON-LD graph (`WebSite`, `ProfilePage`, `ImageObject`, `Person`).
- The About section is the visible version of the same facts. Keep it, the JSON-LD `Person`
  and `llms.txt` saying the same thing when any of them changes.
- Phone and email are deliberately not in the page, JSON-LD or `llms.txt`; they live only in
  `xaviour.vcf`.
- Update `dateModified` in the JSON-LD and `lastmod` in `sitemap.xml` on meaningful changes.

The roadmap panel links to https://vectoriselabs.com/scaling-roadmap/ (tagged
`utm_source=xav-contact`). Its canvas is a miniature of the site's bottleneck graphic: changes
flow through seven areas over five stages, bunch up and are lost at two orange bottlenecks (each named by its area, such as TESTING), then flush once the
bottlenecks lift to the team's stage. It pauses off screen and draws one still frame under
reduced motion.

## Links

| Where | URL |
|---|---|
| LinkedIn | https://linkedin.com/in/xaviourg |
| WhatsApp | https://wa.me/61416612608 |
| X | https://x.com/XaviourDG |
| Instagram | https://www.instagram.com/xaviour.greenhalgh/ |
| Facebook | https://www.facebook.com/xaviour.greenhalgh.2025 |
| Blog | https://vectoriselabs.com/blog/ |
| Let's talk (book a call) | https://cal.com/vectorise/intro |
| Email (vCard only) | xaviour@vectoriselabs.com |
| Phone (vCard only) | +61 416 612 608 |

## Edit your details

Contact values live in **two** places. Keep them in sync:

1. **`index.html`**: the links in `<main>`. Each is an anchor with an inline SVG icon,
   a mono label and a value.
2. **`xaviour.vcf`**: the file the "Add contact" button shares. Plain vCard 3.0 with CRLF line
   endings. After editing, re-CRLF it:
   `perl -i -pe 's/\r?\n/\r\n/g' xaviour.vcf`

### How the Add contact button works across platforms

| Environment | Path taken | UX |
|---|---|---|
| iOS Safari 16+ | JS upgrades the click to `navigator.share({ files: [vcf] })` | Native iOS share sheet opens with **Save to Contacts** as a first-class option |
| Android Chrome | Same `navigator.share()` path (Web Share API Level 2) | Native Android share sheet, then Contacts |
| Desktop / older iOS / unsupported browsers | The anchor's `href="xaviour.vcf"` runs unchanged | Browser downloads the file; user opens it in Contacts |

The single static `xaviour.vcf` is the source of truth for both paths.

## Photo

`xaviour-sydney.jpg` and `og.jpg` are cut from the master photo with Pillow, which drops the
EXIF block (the master carries GPS). Strip metadata from any replacement before committing.

## Deploy

Push to `main`. GitHub Pages serves the repository root over HTTPS.
