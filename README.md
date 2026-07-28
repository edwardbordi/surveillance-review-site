# Surveillance Review — website

Public marketing + download site for Surveillance Review.
Deployed on Vercel at **https://surveillance-review-site.vercel.app**.

Pushing to `main` deploys automatically. No build step — it's a static site.

## Structure

```
index.html      download page (OS detection, install steps, license-key note)
favicon.png
vercel.json     /download rewrite + clean URLs
```

## How the download links work

Buttons point at the **public releases repo**:

```
https://github.com/edwardbordi/surveillance-review-releases/releases/latest/download/SurveillanceReview.dmg
https://github.com/edwardbordi/surveillance-review-releases/releases/latest/download/SurveillanceReview-Windows.zip
```

`releases/latest/download/...` always resolves to the newest published release, so
**publishing a release is all it takes** — this site never needs editing to ship a
new version. The version pill reads the newest release tag from the GitHub API, so
the page and the app's update check can never disagree.

⚠️ Release asset filenames must match the constants at the bottom of `index.html`.

## Local preview

```
python3 -m http.server 8080
# http://localhost:8080
```

Downloads are blocked by browsers over plain `http://localhost` ("unverified
download") — that's a local-only artifact; it works over HTTPS in production.

## Notes

- The product source lives in a **separate private repo**. Nothing here should
  reference it, and no credentials belong in this repo — it's public.
- `.vercel/` is local Vercel link state and is gitignored.
