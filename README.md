# Qurbi — deployment guide

This is a single repo that serves the whole `qurbi.net` site: a landing page at the root, and
both tools underneath it as subpaths.

```
/               → landing page (index.html, og-image.png)
/schools/       → School Finder
/hospitals/     → Hospital & Doctor Finder
CNAME           → tells GitHub Pages this repo serves qurbi.net
```

One repo, one GitHub Pages site, one domain — simpler to manage than two separate repos, and the
nav bar at the top of every page (Qurbi · Schools · Hospitals) lets people move between the two
tools without knowing separate URLs.

The routing API key is already embedded in both tools' `index.html` (same OpenRouteService key
throughout), so there's nothing to paste there.

## Step 1 — Buy the domain

If you haven't already: register `qurbi.net` (or whichever you land on) at Namecheap, Cloudflare
Registrar, GoDaddy, etc. — a handful of dollars a year.

## Step 2 — Create the repo and upload everything

1. Go to https://github.com and sign in.
2. Click **+** → **New repository**. Name it `qurbi` (or anything — the name doesn't affect the
   live URL once the custom domain is set), set it **Public**, click **Create repository**.
3. On the repo page, click **uploading an existing file**.
4. Drag in **the whole folder structure** — `index.html`, `og-image.png`, and `CNAME` at the top
   level, plus the `schools/` and `hospitals/` folders with their contents. GitHub's upload box
   accepts dragging folders directly and will preserve the paths.
5. Commit the changes.
6. Go to **Settings** → **Pages** (left sidebar). Under "Build and deployment", set **Source** to
   **Deploy from a branch**, branch **main**, folder **/ (root)**, **Save**.

## Step 3 — Point the domain at GitHub Pages

1. At your domain registrar's DNS settings, add these **four A records** for the apex domain
   (`qurbi.net`, no subdomain prefix):
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
   (If you'd rather use `www.qurbi.net`, add a **CNAME record** for `www` pointing to
   `yourusername.github.io` instead, and adjust the `CNAME` file in the repo to `www.qurbi.net`.)
2. Back in the repo's Settings → Pages, under "Custom domain," enter `qurbi.net` and Save. GitHub
   will verify DNS (can take a few minutes up to ~24 hours) and then let you check **Enforce
   HTTPS** — do that once it's available; it's free automatic SSL.
3. Visit `https://qurbi.net` once it's live and confirm the landing page loads, and that clicking
   through to Schools and Hospitals works.

## Step 4 — Set up analytics (one GoatCounter site for the whole domain)

1. Go to https://www.goatcounter.com and sign up (free, no credit card) or sign in if you already
   have an account from before.
2. Pick a site code, e.g. `qurbi` (becomes `qurbi.goatcounter.com`).
3. In **all three** `index.html` files (root, `schools/`, `hospitals/`), find
   `REPLACE_WITH_YOUR_GOATCOUNTER_CODE` (near the end, just before `</body>`) and replace it with
   your code — the same one in all three files:
   ```html
   <script data-goatcounter="https://qurbi.goatcounter.com/count"
           async src="//gc.zgo.at/count.js"></script>
   ```
4. Commit each change. Because it's one site across the whole domain, your GoatCounter dashboard
   will show a path breakdown (`/`, `/schools/`, `/hospitals/`) automatically — you get combined
   traffic *and* per-tool numbers without juggling two separate accounts.

## Step 5 — Test before you share

- Load `https://qurbi.net`, click into both tools from the nav bar, and run a real search in each
  — confirm drive times populate with real numbers, not just the "(estimated — live routing
  unavailable)" fallback.
- Paste the live URL into LinkedIn's (or wherever) post composer and check the preview card shows
  the right image and title.
- Try it on a phone.
- Check GoatCounter shows a pageview after you load the site yourself.

## What's still worth doing

- **Add a copyright/license line to the footer** if you want a clearer legal basis against
  someone copying the site wholesale — say the word and I'll add it to all three pages.
- **Doctor and hospital data are still samples** — 51 doctors across 3 of 8 hospitals, and the
  school list is ~35 schools. Both are designed to grow; nothing breaks by adding more entries to
  the data files later.
- **`www.qurbi.net` redirect** — optional; most registrars/DNS providers can redirect
  `www.qurbi.net` → `qurbi.net` (or vice versa) so both work no matter which one someone types.
