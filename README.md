# xav-contact

A single-page static contact card for handing out at events via QR code.
Hosted free on GitHub Pages. No build step, no framework — just `index.html`.

Live URL after deploy: **https://xaviourg.github.io/xav-contact**

## What's in the box

```
index.html     The whole site: HTML + inline CSS + tiny inline JS
favicon.svg    "X" mark
.nojekyll      Tells GitHub Pages to serve files verbatim (no Jekyll)
README.md      This file
```

## Edit your details

All contact values live in one `CONTACT` block near the top of `<body>` in
`index.html`. Change values there; the page, the vCard download, and the
Cal embed all read from it. Phone numbers are E.164 (e.g. `+61416612608`);
`whatsapp` omits the leading `+` for the `wa.me/` URL.

### Cal embed

The Cal config exactly mirrors what `vectoriselabs.com` uses — booking on
the EU Cal instance, not `cal.com`:

```js
cal: {
  link:      "vectoriselabs",            // user/team slug
  namespace: "vectorise-trial",
  origin:    "https://app.cal.eu",        // EU instance — NOT cal.com
  embedJs:   "https://app.cal.eu/embed/embed.js"
}
```

To point at a different Cal account, swap `link` and (if it's on the
cal.com main instance) change both `origin` and `embedJs` to
`https://app.cal.com` / `https://app.cal.com/embed/embed.js`.

## Run locally

```sh
cd xav-contact
python3 -m http.server 8000
```

Open http://localhost:8000 — or visit `http://<your-LAN-IP>:8000` from a
phone on the same Wi-Fi to test all the `tel:` / `mailto:` / `wa.me` /
`t.me` taps and the vCard download end-to-end.

## Deploy to GitHub Pages

One-time setup:

```sh
cd xav-contact
git init
git add .
git commit -m "init"
```

Create a new **public** repo on GitHub called `xav-contact` (no README,
no .gitignore — empty), then:

```sh
git remote add origin git@github.com:xaviourg/xav-contact.git
git branch -M main
git push -u origin main
```

On GitHub:

1. Repo → **Settings** → **Pages**
2. **Source**: *Deploy from a branch*
3. **Branch**: `main` / `(root)` → **Save**
4. Wait ~60 seconds. Refresh the Pages settings page until it shows the
   live URL: `https://xaviourg.github.io/xav-contact/`

Any future `git push` to `main` redeploys within a minute.

## Generate a QR code

If you have Homebrew:

```sh
brew install qrencode
qrencode -o qr.png -s 12 -m 2 https://xaviourg.github.io/xav-contact
```

Or paste the URL into any QR generator (qrcode-monkey.com, qr.io, etc.).
Print it small — phones handle dense codes fine.

## Tips

- **Custom domain** (optional): add a `CNAME` file containing
  `contact.vectoriselabs.com`, point a DNS `CNAME` record at
  `xaviourg.github.io`, then set the custom domain in Pages settings.
- **Add Instagram back** later: copy any existing `.row` link in
  `index.html`, change the `href` to `https://instagram.com/<handle>`,
  and update the label/value.
- **Cal.com themes**: the embed forces dark theme via `theme: "dark"` in
  the init script.
