# hemantkumar.dev — Portfolio

> Source for [hemantkumar.dev](https://hemantkumar.dev), served via GitHub Pages.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | Portfolio landing page |
| `hk.css` | Shared design system (used across all HK sites) |
| `CNAME` | Custom domain — tells GitHub Pages to serve on `hemantkumar.dev` |
| `README.md` | This file |

---

## Design system — hk.css

All sites under `hemantkumar.dev` share a single stylesheet. To use it on any new site:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="hk.css">
```

Add the theme flash-prevention script to `<head>` and the toggle script before `</body>`:

```html
<!-- In <head> -->
<script>
  (function () {
    var s = localStorage.getItem('hk-theme');
    var p = window.matchMedia('(prefers-color-scheme: dark)').matches;
    if (s === 'dark' || (!s && p)) document.documentElement.classList.add('dark');
  })();
</script>

<!-- Before </body> -->
<script>
  document.getElementById('theme-toggle').addEventListener('click', function () {
    var isDark = document.documentElement.classList.toggle('dark');
    localStorage.setItem('hk-theme', isDark ? 'dark' : 'light');
  });
</script>
```

### Section pattern

Every section follows the same structure — label pill above, bordered box wrapping content:

```html
<section class="section">
  <p class="label">Section Title</p>
  <div class="box">
    <!-- rows go here -->
  </div>
</section>
```

### Key CSS classes

| Class | Purpose |
|-------|---------|
| `.label` | Amber pill tag above a section box |
| `.box` | Single border wrapping section content |
| `.project` | Clickable project row (3-col grid) inside `.box` |
| `.feature-row` | 2-col label + description row inside `.box` |
| `.score-row` | Score bar row inside `.box` |
| `.pipe-row` | Numbered pipeline step row inside `.box` |
| `.setup-row` | Hoverable setup option row inside `.box` |
| `.connect-inner` | Padded wrapper for connect section inside `.box` |
| `.skills-grid` | 2-col skills grid inside `.box` |
| `.formula-block` | Tinted formula block inside `.box` |

---

## GitHub Pages setup

### DNS — AWS Route 53

Four A records on the apex domain pointing to GitHub Pages:

| Type | Name | Value |
|------|------|-------|
| `A` | `hemantkumar.dev` | `185.199.108.153` |
| `A` | `hemantkumar.dev` | `185.199.109.153` |
| `A` | `hemantkumar.dev` | `185.199.110.153` |
| `A` | `hemantkumar.dev` | `185.199.111.153` |

> Route 53 doesn't allow a CNAME on the apex domain, so A records are used instead of a CNAME.

Verify DNS at any time:

```bash
dig hemantkumar.dev
# Should show A records → 185.199.x.x
```

### GitHub Pages settings

Repo → **Settings → Pages**

| Setting | Value |
|---------|-------|
| Source | Deploy from branch |
| Branch | `main` / `/ (root)` |
| Custom domain | `hemantkumar.dev` |
| Enforce HTTPS | ✅ Enabled |

### CNAME contents

```
hemantkumar.dev
```

---

## Deployment

To update the site:

```bash
# edit index.html or hk.css
git add .
git commit -m "docs: update portfolio"
git push origin main
```

Changes go live within ~1 minute.

---

## Migrated from AWS

Previously hosted on AWS S3 + CloudFront + Terraform. Moved to GitHub Pages because:

- AWS hosting cost ~$1–3/month for a static file
- AWS skills are better demonstrated by [snarky-squirrel](https://snarky-squirrel.hemantkumar.dev) and [links.hemantkumar.dev](https://links.hemantkumar.dev)
- GitHub Pages is free and zero-maintenance for static content

AWS resources decommissioned: CloudFront distribution, S3 bucket, associated IAM policies.
Route 53 hosted zone kept — still needed for subdomains.

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Site not loading after DNS change | Wait 5–15 min for GitHub to verify DNS and provision TLS cert |
| GitHub DNS check stuck | Remove custom domain in Settings → Pages, save, re-enter, save again |
| `hk.css` styles not applying | Confirm `hk.css` is at the repo root alongside `index.html` |
| Old content showing | Hard-refresh (`Cmd/Ctrl + Shift + R`) or wait for CDN cache to clear |

---

## Related

| Site | Repo | Stack |
|------|------|-------|
| [hemantkumar.dev](https://hemantkumar.dev) | `hkcodebase/portfolio` | GitHub Pages |
| [snarky-squirrel.hemantkumar.dev](https://snarky-squirrel.hemantkumar.dev) | `hkcodebase/snarky-squirrel` | GitHub Pages + AWS Bedrock |
| [links.hemantkumar.dev](https://links.hemantkumar.dev) | — | Lambda + DynamoDB + API Gateway |
