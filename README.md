# xav-contact

Xaviour's personal link and contact card. One static page, hosted free on GitHub Pages.
No build step, no framework, no third-party requests.

Live: **https://xav.contact**

## What's in the box

```
index.html     The whole site: HTML + inline CSS + three small inline scripts
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
hairline) open each section. The page is personal: no Vectorise logo, with Vectorise named
only in the title line under the name.

Order: full-width photo, name and title line, the Connect grid (LinkedIn across the top, then
Contact card, Blog, X, Instagram, WhatsApp, Facebook), the compact roadmap panel ("Work in
software?"), the About bio with a fact list, and a footer with the Share button.

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
