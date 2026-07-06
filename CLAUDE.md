# CLAUDE.md — kimbytes.com

## What this is
Static website for the domain **kimbytes.com** — the public hub for Kim's apps:
- **App showcase / landing** (`public/index.html`, title "Kimbytes - AI-Powered Apps")
- **Legal + support pages** for multiple apps (privacy policies, terms, support)
- **`app-ads.txt`** for AdMob verification

Plain HTML/CSS, no build step, no framework. Firebase Hosting serves the `public/` folder directly.

## Location & Git
- **Local:** `/Users/kbartiquel/Documents/PROJECTS/kimbytes.com`
- **Repo:** `git@github.com:kbartiquel/kimbytes.com.git` — branch **`main`**
- History is short (Initial commit + a contact-email/style update). Commit + push normally.

## Hosting (Firebase) — MOVED July 6 2026 to its OWN project
- **Project: `kimbytes-website`** (fully separate from the SocMedAI/Viral Trends project). Site: **`kimbytes-website` → https://kimbytes-website.web.app**. `.firebaserc` default + `firebase use` + `"site"` in firebase.json all point here — a deploy from this folder **cannot** touch the Viral Trends app anymore.
- Deploy: `npx firebase deploy --only hosting` from this folder. Account: kimoytech@gmail.com.
- **Firebase Analytics** wired in `public/index.html` (measurementId `G-5J5ZCFGFDF`, gstatic module import at end of `<body>`).
- **Only `public/` is served.** Root-level `index.html`/`404.html` are legacy/unused — edit the `public/` versions.
- **⏳ Custom domain:** attach `kimbytes.com` to this site: [console → kimbytes-website → Hosting](https://console.firebase.google.com/u/0/project/kimbytes-website/hosting/sites/kimbytes-website) → Add custom domain → follow DNS steps. If the domain is still attached to the old `socmedai` project's site, remove it there first (console of project `socmedai` → Hosting → domains).
- (Interim site `kimbytes` in the socmedai project was deleted July 6 — this project is the only home now.)

## 🚨 Past incident (July 3–6, 2026) — why the site pin exists
Before the pin, a plain `firebase deploy` from here published the showcase to the **default site = `socmedai.web.app`**, wiping the `/api/socmedai/**` rewrites — the Viral Trends app lost settings/trends/AI/tracking for ~3 days (analytics hole Jul 3–6). Recovery, if the app site is ever clobbered again:
```bash
cd /Users/kbartiquel/Documents/PROJECTS/SocMedAI/FirebaseServer && npx firebase deploy --only hosting
```

## public/ structure (what's live)
| File / dir | Purpose |
|-----------|---------|
| `index.html` | Kimbytes app showcase / landing |
| `app-ads.txt` | AdMob ads.txt — `google.com, pub-1867109255730774, DIRECT, f08c47fec0942fa0` |
| `404.html` | Not-found page |
| `manifest.json`, `favicon.png`, `icon.png`, `icons/` | PWA/site icons |
| `ai-quiz-privacy-policy.html`, `ai-quiz-privacy-policy-ja.html`, `ai-quiz-terms.html` | AI Quiz / Quiz Maker legal |
| `photo_caption_policy.html`, `photo_caption_terms.html` | Caption app legal |
| `privacy.html`, `terms.html`, `support.html` | Generic/shared legal + support |
| `pptmaker/` | PPT Maker app pages |

## Notes / gotchas
- **Shared Firebase project** with SocMedAI — deploying `--only hosting` from THIS folder uses THIS `firebase.json` (single untargeted site). Run it from this directory, never from `SocMedAI/FirebaseServer` (that repo has app-site/admin-panel targets + functions).
- Adding a new app's legal page? Drop the HTML in `public/`, link it from `public/index.html`, commit, deploy hosting.
- **Sibling project:** `../kimbytesapps-links` (`github.com/kbartiquel/kimbytesapps-links`) — a related app-links/redirect site (own repo, own deploy).
- Per SocMedAI memory, `kimbytes.com` has historically been associated with `socmedai.web.app`; if the domain mapping ever looks off, check **Firebase Console → Hosting → custom domains** for the `socmedai` project.
