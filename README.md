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
https://github.com/edwardbordi/surveillance-review-releases/releases/latest/download/SurveillanceReview-Intel.dmg
https://github.com/edwardbordi/surveillance-review-releases/releases/latest/download/SurveillanceReview-Setup.exe
```

`releases/latest/download/...` always resolves to the newest published release, so
**publishing a release is all it takes** — this site never needs editing to ship a
new version. The version pill reads the newest release tag from the GitHub API, so
the page and the app's update check can never disagree.

### Why Windows gets the installer and not the zip

`SurveillanceReview-Windows.zip` is still published on every release, but this page
does not link it, on purpose.

Windows attaches a `Zone.Identifier` mark to anything downloaded from the internet,
and Explorer copies that mark onto every file it extracts from a zip. The .NET
runtime refuses to load zone-marked assemblies, so a zip install dies at startup
with a `clr` / `Python.Runtime.dll` error before the app draws anything. Clearing it
requires an Unblock step *before* extracting — a manual step whose only failure mode
is a Python traceback, which is not an install path we can hand to a customer.

An installer writes files itself and marks nothing, so the failure cannot occur. The
zip remains for machines where running an installer isn't allowed; anyone who needs
it is told where it is rather than finding it here by accident.

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
