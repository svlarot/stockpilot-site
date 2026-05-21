# StockPilot — website

Public, GitHub-Pages-hosted landing page + Terms of Service for the
StockPilot mobile app. Operated by **Bluenex** (a Philippine sole
proprietorship registered with the DTI). Three HTML files, one CSS file,
one SVG favicon, zero JavaScript, zero third-party requests from the page
itself.

**The privacy policy is hosted in a separate repo,
[`svlarot/stockpilot-legal`](https://github.com/svlarot/stockpilot-legal),
served at <https://svlarot.github.io/stockpilot-legal/>.** That repo is
the canonical source for the privacy policy. This repo's `privacy.html`
is a meta-refresh redirect to that URL so any old bookmark or external
link still resolves cleanly.

Live URLs:

- Home: https://svlarot.github.io/stockpilot-site/
- Terms of service: https://svlarot.github.io/stockpilot-site/terms.html
- Privacy policy (canonical, separate repo): https://svlarot.github.io/stockpilot-legal/

Support email used throughout: `support@bluenex.org`.

## File tree

```
.
├── index.html       hero card with brand, one-line description, two big links
├── privacy.html     privacy policy in semantic HTML
├── terms.html       terms of service in semantic HTML
├── styles.css       ~200 lines, system-font, dark/light auto, single max-width container
├── favicon.svg      inline-coloured "S" mark; matches the app's brand teal
└── README.md        this file
```

## How to update the legal text

1. Edit `privacy.html` or `terms.html` in place.
2. Bump the "Last updated" date at the top of the page.
3. Commit and push to `main`. GitHub Pages rebuilds in 30–90 seconds.
4. If the change is material, mention it in the app's release notes so
   users know to re-read the policy on their next launch.

## How to deploy from scratch

This repo is wired to GitHub Pages from `main` branch, root path. The
deployment was bootstrapped with:

```bash
gh repo create stockpilot-site --public \
  --description "StockPilot website + privacy and terms" \
  --source . --remote origin --push

gh api -X POST /repos/svlarot/stockpilot-site/pages \
  -f 'source[branch]=main' \
  -f 'source[path]=/'
```

To check Pages build status:

```bash
gh api /repos/svlarot/stockpilot-site/pages | jq '.status, .html_url'
```

## Design notes

- **No JavaScript.** Three files, three navigations, the browser does
  everything.
- **No external fonts or scripts.** `system-ui` font stack. Zero
  third-party network requests from the page itself — appropriate for a
  privacy policy page.
- **No cookies, no analytics, no consent banner.** The site itself
  collects nothing, so nothing to consent to.
- **Dark mode** via `@media (prefers-color-scheme: dark)` — auto-follows
  the user's OS theme.
- **Mobile responsive** via a single `max-width: 720px` container.
- **SEO basics**: `<title>`, `<meta name="description">`, `<meta
  name="theme-color">`, `<link rel="canonical">`, and the `<meta
  property="og:*">` set for clean link previews.

## Custom domain (later, optional)

If you want `stockpilot.bluenex.org`:

1. Add a `CNAME` file at the repo root containing `stockpilot.bluenex.org`.
2. Add a DNS CNAME record `stockpilot.bluenex.org → svlarot.github.io`.
3. In the repo's Pages settings, enter `stockpilot.bluenex.org` as the
   custom domain and tick "Enforce HTTPS" once GitHub provisions a TLS
   certificate.

Not part of the initial deployment.

## What this site does NOT do

- It does not host the app binary (Google Play does that).
- It does not run a contact form (the email link does the job).
- It does not have a blog, changelog, or release notes feed.
- It does not embed third-party scripts of any kind.
- It does not require Jekyll, Hugo, or any build step. `index.html`,
  `privacy.html`, and `terms.html` are served as-is.
