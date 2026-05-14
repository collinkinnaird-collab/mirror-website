# Mirror Fitness website

Marketing one-pager + legal docs for Mirror Fitness (iOS). Three static
HTML files plus one stylesheet — no build step, no dependencies. Deploys
to GitHub Pages on a custom domain.

## Files

- `index.html` — marketing one-pager (Apple wants a support/marketing URL too)
- `privacy.html` — Privacy Policy
- `terms.html` — Terms of Service
- `assets/style.css` — shared styles (mobile-first, dark-mode aware)
- `CNAME` — custom domain (currently `mirrorfitness.app` — change before pushing if you pick a different domain)

## Before first push

1. **Add the founder headshot.** `index.html` references `assets/collin.jpg`. Drag your headshot into `website/assets/` and rename it to `collin.jpg`. Recommend a roughly-square crop, at least 240×240 px (the site displays at 120×120 so 2× for retina is enough). If you skip this, the contact section shows a broken image icon — not catastrophic but not pretty.

## One-time setup

1. Decide on the domain. Default in `CNAME` is `mirrorfitness.app`. If you pick a different one, edit the `CNAME` file and any references in the HTML/CSS (none today — links are root-relative).
2. Register the domain. Recommended registrars: Cloudflare, Namecheap, Porkbun. `.app` TLDs are ~$15–20/year and are HTTPS-only by TLD policy, which dovetails with GitHub Pages' free Let's Encrypt cert.
3. Create a public GitHub repo (e.g. `mirror-website` under your account).
4. Push these files to it:

   ```sh
   cd /Users/collinkinnaird/Desktop/Mirror/Mirror/website
   git init
   git add .
   git commit -m "initial site"
   git branch -M main
   git remote add origin https://github.com/<your-username>/mirror-website.git
   git push -u origin main
   ```

5. In the repo's Settings → Pages, set Source to "Deploy from a branch", Branch = `main`, Folder = `/ (root)`. Save.
6. In the same Pages panel, the Custom Domain field should auto-populate from `CNAME`. Check "Enforce HTTPS" once it becomes available (it takes a few minutes after DNS resolves).
7. At your DNS provider, point the domain at GitHub Pages:
   - Four A records for the apex domain:
     - `185.199.108.153`
     - `185.199.109.153`
     - `185.199.110.153`
     - `185.199.111.153`
   - One AAAA record for the apex (IPv6, optional but recommended):
     - `2606:50c0:8000::153`
     - `2606:50c0:8001::153`
     - `2606:50c0:8002::153`
     - `2606:50c0:8003::153`
   - One CNAME for `www.` → `<your-username>.github.io`
8. Wait for DNS to propagate (usually 5–30 minutes). Open `https://mirrorfitness.app/` in a browser to verify.

## URLs to paste into App Store Connect

Once the site is live:

- **Marketing/Support URL**: `https://mirrorfitness.app/`
- **Privacy Policy URL**: `https://mirrorfitness.app/privacy.html`
- **EULA / Terms of Service URL** (optional in App Store Connect for free apps): `https://mirrorfitness.app/terms.html`

## Editing notes

- All three pages share `assets/style.css` — change once, change everywhere.
- The footer + top nav are repeated in each HTML file (no templating engine). Acceptable for three pages; if it grows, consider adding a static-site generator.
- "Last updated: …" date appears at the top of `privacy.html` and `terms.html`. Bump it whenever you make a material change to either policy.

## Future updates checklist

When you ship a feature that materially changes data handling — e.g. progress photos, HealthKit, IAP — edit the relevant `privacy.html` section (and `terms.html` if needed), bump the "Last updated" date, commit, push. GitHub Pages auto-publishes within ~30 seconds.
