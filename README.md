# webrulers.in

Static site for Web Rulers, hosted free on GitHub Pages. No build step: plain HTML + one CSS file.

```
index.html               → https://webrulers.in
dearme/privacy/          → https://webrulers.in/dearme/privacy/   (Play Console privacy URL)
dearme/terms/            → https://webrulers.in/dearme/terms/
404.html                 → shown for unknown URLs
assets/style.css         → all styling; colours/spacing are tokens at the top
assets/clients/          → client logos for the "Our work" section
CNAME                    → tells GitHub Pages the custom domain
.nojekyll                → serve files as-is (no Jekyll processing)
```

## Preview locally

```bash
python3 -m http.server 8080   # then open http://localhost:8080
```

## Deploy (one time)

1. Create a **public** GitHub repo, e.g. `webrulers-site`, and push this folder:
   ```bash
   git init && git add . && git commit -m "Initial site"
   git branch -M main
   git remote add origin git@github.com:<username>/webrulers-site.git
   git push -u origin main
   ```
2. Repo **Settings → Pages**: Source = *Deploy from a branch*, `main` / `/ (root)`.
   The custom domain `webrulers.in` is picked up from the `CNAME` file.
3. DNS at your domain registrar. Remove any existing `A` records on `@` first
   (e.g. a parking page) and leave `MX`/`TXT` alone:

   | Type  | Name  | Value                         |
   |-------|-------|-------------------------------|
   | A     | `@`   | `185.199.108.153`             |
   | A     | `@`   | `185.199.109.153`             |
   | A     | `@`   | `185.199.110.153`             |
   | A     | `@`   | `185.199.111.153`             |
   | CNAME | `www` | `<username>.github.io`        |

4. Once DNS has updated (minutes to a few hours), tick **Enforce HTTPS** in Settings → Pages.

## Keeping the Dear Me legal pages in sync

The pages under `dearme/` mirror `DearMe/legal/*.md`, which the app also shows
in-app (`legal.*` keys in `en.json`). When the policy changes, update all three
together and bump `PRIVACY_VERSION` / `TERMS_VERSION` in `src/constants/legal.ts`
so users are asked to accept the new version.
