# xav-contact

Xaviour's personal link and contact card. One static page, hosted free on GitHub Pages.
No build step, no framework, no third-party requests.

Live: **https://xaviourg.github.io/xav-contact**

## What's in the box

```
index.html     The whole site: HTML + inline CSS + two tiny inline scripts
xaviour.vcf    Static vCard the "Add contact" button shares / downloads
xaviour.jpg    Headshot (480px square)
favicon.svg    The Vectorise compass mark on the dark stage
fonts/         Geist + Geist Mono woff2, vendored (OFL). See fonts/SOURCE.md
.nojekyll      Tells GitHub Pages to serve files verbatim (no Jekyll)
```

## Design

The page follows the Vectorise visual system (`space-sales/visual/brand-identity.md` and
`layout-patterns.md`): dark stage `#0a0f14`, card surface `#0f1419`, one accent blue `#60A5FA`,
Geist for words, Geist Mono uppercase wide-tracked for every kicker and label, glows instead of
shadows, no gradients, no bounce. One dominant element (the identity block), then the primary
action, then two full-width highlights (LinkedIn, Vectorise), then a compact grid for everything
else.

## Links

| Where | URL |
|---|---|
| LinkedIn | https://linkedin.com/in/xaviourg |
| WhatsApp | https://wa.me/61416612608 |
| Substack | https://substack.com/@xaviourg |
| X | https://x.com/XaviourDG |
| Telegram | https://t.me/xaviourg |
| Email | xaviour@vectoriselabs.com |
| Phone | +61 416 612 608 |
| Vectorise | https://vectoriselabs.com |

## Edit your details

Contact values live in **two** places. Keep them in sync:

1. **`index.html`**: the link tiles in `<main>`. Each tile is an anchor with an inline SVG icon,
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

## Headshot

`xaviour.jpg` is a 480px JPEG cut from the master headshot. To regenerate from a new master:

```
sips -s format jpeg -s formatOptions 82 -Z 480 master.png --out xaviour.jpg
```

## Deploy

Push to `main`. GitHub Pages serves the repository root over HTTPS.
