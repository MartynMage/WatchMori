# watchmori.io

Marketing site and privacy policy for WatchMori, an anime tracking app for iPhone and iPad.

Static HTML, no build step. Cloudflare Pages serves it from the root of this repo, so anything on `main` is live.

## Layout

```
index.html          landing page
privacy/index.html  privacy policy
screenshots/        App Store screenshots used on the page
watchmori-icon.png  app icon (also used as the og:image)
favicon.*, icon-*   favicons and touch icons
_headers            security and cache headers (Cloudflare)
CNAME               left over from GitHub Pages, see below
```

Both pages are self-contained — the CSS sits in a `<style>` block in each file, and the only external request is the Nunito webfont. They share a palette and a nav, so a change to one usually needs the same change in the other.

## Running it locally

Opening `index.html` straight from disk mostly works, but `/privacy` and the root-relative paths only resolve properly over HTTP:

```bash
npx serve .
```

## Deploying

Cloudflare Pages is connected to this repo and builds on push to `main`. There is nothing to build, so the project settings are:

```
Framework preset   None
Build command      (empty)
Build output       /
```

Every branch that isn't `main` gets its own preview URL, which is a good way to look at a change on a real domain before it goes to production.

`_headers` is read by Cloudflare at deploy time. It sets a content security policy that allows only self-hosted files plus the Google Fonts stylesheet and font files — if the page ever needs a script, an embed or an external image, that policy has to be widened or the browser will silently block it.

`CNAME` is a GitHub Pages file and does nothing on Cloudflare. It is still here so GitHub Pages keeps answering during the DNS cutover; delete it once watchmori.io is served by Cloudflare and GitHub Pages is turned off in the repo settings.

## Screenshots

The files in `screenshots/` are the App Store screenshots, pulled from the listing at 750px wide and saved as WebP. To refresh them after a release, grab the current set from the [store page](https://apps.apple.com/us/app/watchmori/id6757887049) and overwrite the files, keeping the same names.

Two things to watch:

- The hero image is `shot-4.webp` cropped down to just the phone. The crop is done in CSS (`.device` in `index.html`) with hardcoded offsets, so a replacement screenshot framed differently needs those numbers redone.
- The four screenshots share a background but aren't one continuous image — the seams don't line up. That's why the gallery has gaps between them rather than butting them together.

## Things that go stale

The version number, download size and minimum iOS version are written into `index.html` by hand, in the hero eyebrow, the facts strip and the footer. The privacy policy has its own "last updated" date near the top of `privacy/index.html`. Worth a pass over both when you ship an update.
