# Gitly Web

The web version of **Gitly** — your GitHub, beautifully yours. This is the same
One UI-styled GitHub client as the Android app, rebuilt as a fully
responsive web app that works on PC, laptop, tablet and phone, with a
redesigned desktop experience.

## What changed for the web

- **Desktop layout** (screens >= 900px): a frosted left sidebar replaces the
  phone's bottom pill navigation — brand, Home, Repos, Commits, Activity,
  Issues, Alerts, and Settings pinned to the bottom. Content sits in a
  centered column that widens on large screens.
- **Redesigned login**: a split screen on desktop — an accent-gradient brand
  panel on the left, the sign-in card on the right. On phones it collapses
  back to the original single-column One UI look.
- **Desktop polish**: hover states for every tappable element, centered modal
  dialogs instead of bottom sheets, toasts docked bottom-right, thin custom
  scrollbars, keyboard focus outlines, and an SVG favicon.
- **Mobile preserved**: below 900px the app is exactly the One UI experience
  from the Android build.
- **Web-safe app logic**: `app.js` is the app's own code with a single guard
  added so the Android APK update popup never shows in a browser. Everything
  else (GitHub API calls, uploads, clipboard, downloads) already had browser
  fallbacks.

## File map

| File | Purpose |
|---|---|
| `index.html` | Redesigned app shell (sidebar, split login) |
| `web.css` | The desktop redesign layer — loads after `app.css` |
| `app.css` | Original One UI stylesheet (unchanged) |
| `app.js` | App logic (unchanged + 1 web guard) |
| `fonts/OneGitSans.ttf` | App typeface |
| `icon.svg`, `logo.jpg` | Icons |
| `.nojekyll` | Tells GitHub Pages to serve files as-is |

## Run locally

Any static file server works:

    python3 -m http.server 8000

then open http://localhost:8000.

## Deploy on GitHub Pages

### Option A — from the GitHub website (no tools needed)

1. Create a new repository on github.com (e.g. `gitly-web`). Do **not** add a
   README from the template picker.
2. On the repo page click **Add file → Upload files**, drag this whole folder's
   contents in (everything: `index.html`, `web.css`, `app.css`, `app.js`,
   `fonts/`, `icon.svg`, `logo.jpg`, `.nojekyll`, `README.md`), then
   **Commit changes**.
3. Open **Settings → Pages**. Under "Build and deployment", set
   **Source** to **Deploy from a branch**, branch **main**, folder **/(root)**,
   and Save.
4. Wait ~1 minute. Your site goes live at
   `https://<your-username>.github.io/gitly-web/`.

> If you upload a zip instead, unzip it first — GitHub Pages will not extract
> archives. Note that `.nojekyll` is a hidden file; with the "Upload files"
> web UI it is included automatically, but verify after committing.

### Option B — from the command line

    git init
    git add .
    git commit -m "Gitly web — first release"
    git branch -M main
    git remote add origin https://github.com/<your-username>/gitly-web.git
    git push -u origin main

Then enable Pages as in step 3 above (Settings → Pages → Deploy from a
branch → main → /(root)).

### Optional — a top-level site

Deploying to a repo named exactly `<your-username>.github.io` puts the site at
`https://<your-username>.github.io/` with no sub-path. The app uses hash
routing (`#/home`), so it works from either a sub-path or the root with no
configuration.

### Optional — custom domain

Settings → Pages → Custom domain: enter your domain, add a `CNAME` file in the
repo containing the domain, and point your DNS at GitHub Pages (an `A`/`AAAA`
record to the Pages IPs, or a `CNAME` to `<your-username>.github.io`). Enable
"Enforce HTTPS" once the certificate is issued.

## Sign-in notes

- Sign in with a GitHub personal access token (scopes: `repo`, `read:user`,
  `notifications`, `gist`). The link on the login screen pre-fills the scopes.
- The token is stored only in your browser's localStorage on the device you
  used to sign in. Sign out clears it.
- Since this is a static site with no backend, the token is sent directly from
  your browser to api.github.com — fine for personal use, but avoid signing in
  on shared computers.

## Updating the site later

Just edit files and push (or re-upload through the web UI). Pages redeploys
automatically on every commit to the selected branch.
