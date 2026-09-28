# WatercarbonStone Website Redesign

A modern, responsive redesign of the [WatercarbonStone Fund Management](http://watercarbonstone.com/) website. Static HTML, CSS and a small amount of vanilla JavaScript. No build step, no dependencies.

> Design preview. The contact form is not connected to a backend yet, and the captcha uses Cloudflare's public test key. See [Going live](#going-live).

## What changed

- New layout and typography: a serif headline face, a navy and light-blue palette taken from the existing logo, and a light section for the fund to give the page rhythm.
- The dated stock photos now carry a navy tint so they match the palette.
- Risk warning set apart as its own callout.
- Sticky header, subtle scroll reveal, keyboard focus styles, and `prefers-reduced-motion` support.
- Fully responsive down to phone width.
- Contact form protected by a captcha (Cloudflare Turnstile) and a honeypot field.

All copy is kept word for word from the current site, because this is a regulated business and the wording should not change without the firm's sign-off. The current text has one typo worth fixing: "stock collapse send shockwaves" (probably "stock collapses send shockwaves").

## Structure

```
index.html      Page markup, styles and script (single file)
assets/
  logo.png      WatercarbonStone logo
  about.jpg     About section photo
  fund.jpg      Piano Fund section photo
```

## Run locally

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Going live

The site is static, so it can be uploaded to any host (cPanel, Netlify, Cloudflare Pages, GitHub Pages).

### 1. Contact form

The form currently shows a "design preview" message and sends nothing. It needs a small server side handler (PHP mail script, a form service, or a serverless function) that receives `name`, `email`, `tel`, `message` and sends the email.

### 2. Captcha

The page uses [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/), which is free and rarely shows a puzzle.

1. Create a Turnstile widget in the Cloudflare dashboard and get a site key and secret key.
2. In `index.html`, replace the test key on the `cf-turnstile` element:
   ```html
   <div class="captcha cf-turnstile" data-sitekey="YOUR_SITE_KEY" data-theme="dark"></div>
   ```
3. In the form handler, verify the `cf-turnstile-response` field server side by posting it with your **secret key** to `https://challenges.cloudflare.com/turnstile/v0/siteverify`. Reject the message if `success` is not `true`. The secret key must never appear in the page or in this repository.

The current site key (`1x00000000000000000000AA`) is Cloudflare's public testing key. It always passes and shows a "For testing only" banner.

The hidden `website` field is a honeypot: real visitors never see it, so a filled value means a bot.

### 3. Before launch

- Link the T&Cs and Policies pages in the footer.
- Add a favicon (the current site uses `/img/icon/favicon.png`).
- Confirm the regulatory wording with the firm.

## Design tokens

| Token | Value | Use |
| --- | --- | --- |
| `--bg` | `#0a1428` | Page background |
| `--accent` | `#5bb8e0` | Brand blue from the logo |
| `--paper` | `#f3f6fb` | Light fund section |
| Headings | Iowan Old Style / Palatino / Georgia | System serif stack |
| Body | System sans stack | No web fonts, so no extra requests |

## Ownership

The logo, photographs and text belong to WatercarbonStone. All rights reserved. This repository is private and intended for review by the firm.
