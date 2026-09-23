# CSDA-TESDA Workspace Matrix

**Single-file training tracker & workspace matrix for Visual Graphics Design NC III (TESDA) — Cordillera School of Digital Arts, Inc.**

Month / Week / Day calendar views, COC 1–5 module phase tracking, task verification, personal session lecture notes, an instructor **CONSOLE** (PIN-gated) with emergency class cancellation & global announcements, a **Student Feedback & Portfolio Upload Hub** (Formspree), cross-device sync over a relay channel, a TESDA **BSRS** portal shortcut, and an installable phone launcher (San Beda Boosters Club seal) — all inside one self-contained `index.html`. No build step is required to run it.

---

## Features

- **Three calendar views** (dropdown toggle): Month roadmap grid, "This Week" list, and a daily session timeline — with live clock markers and placeholder session slots.
- **Training roadmap engine**: `START_DATE` Sep 8, 2026 → `END_DATE` Nov 26, 2026 (Mon–Fri), statutory holidays (Oct 2, Nov 1, Nov 2), and seven phase tracks (Orientation → COC 1 Logo → COC 2 Print → COC 3A UX → COC 3B UI → COC 4 Packaging → COC 5 Exhibit).
- **Task verification & personal notes** per date, persisted in `localStorage` (`vgd_submissions`, `vgd_daynotes`).
- **Emergency class cancellation**: instructor cancels/resumes any non-Sunday date; cancelled days override phase tracks instantly for every launcher (red striped cells, `CANCELLED` badges, 🚨 re-scheduling notice modal, double alarm chime for brand-new *today* cancellations).
- **Global class announcement** banner (rotating ticker) on every device.
- **CONSOLE** (🔐 in the nav, PIN unlock): announcements, cancellation control, and a Student Feedback Inbox.
- **Student Feedback & Portfolio Upload Hub** (fixture inside every date modal): name / email / message form (Formspree AJAX) with portfolio image upload (multipart file delivery) and `_subject`/`_replyto` metadata.
- **Cross-device sync relay**: BroadcastChannel + `localStorage` for same-device tabs, plus an [ntfy.sh](https://ntfy.sh) topic relay so instructor broadcasts, cancellations, and feedback travel between devices (with late-joiner state reconciliation).
- **PWA launcher identity**: favicon, iOS `apple-touch-icon`, and web app manifest with the San Beda Boosters Club seal (embedded data-URIs — no extra asset files).

## Quick start

### Run locally
Open `index.html` in any modern browser. That's it. (Relay features need internet; everything else is offline-capable.)

### Deploy to Vercel
1. Upload this folder (or just `index.html`) as a Vercel project — the file **must** be named `index.html`.
2. Open the deployment URL in Safari (iOS: **Share → Add to Home Screen**) or Chrome (Android: **⋮ → Install app**) to install the launcher.

### Deploy with GitHub Pages
Push this repo to GitHub → **Settings → Pages → Deploy from a branch** → `main` / root. `index.html` at the repository root serves as the site.

### Push this folder to GitHub
```bash
cd CSDA-TESDA-Workspace-Matrix
git init
git add .
git commit -m "Initial commit — CSDA-TESDA Workspace Matrix"
git remote add origin https://github.com/<your-account>/CSDA-TESDA-Workspace-Matrix.git
git branch -M main
git push -u origin main
```
(Or create the repository on github.com and drag-drop the folder contents in the web uploader.)

## Configuration (edit `index.html`)

| Constant | Purpose | Notes |
|---|---|---|
| `ADMIN_CONSOLE_PIN` | Unlock key for the instructor **CONSOLE** | **Change it** before sharing widely (default in source: `csda2026`) |
| `CLOUD_RELAY_TOPIC` | Shared ntfy.sh sync topic | **Change it** to an unguessable value; it is the only access control for the class channel |
| `INSTRUCTOR_FORM_ENDPOINT_ID` | Formspree form id for feedback email + attachments | Replace with your own form id from [formspree.io](https://formspree.io) |
| `SESSION_TIME_SLOTS` | Placeholder session hours for Week/Day views | Replace with the real timetable when available |
| `START_DATE` / `END_DATE` / `HOLIDAYS` | Training window & statutory skips | Keep as-is for the 2026 term |

⚠️ These values live **in the page source** — see [SECURITY.md](SECURITY.md) before publishing the repository publicly.

## Data & sync architecture

- **Local-first**: submissions, notes, feedback outbox, and master config persist in `localStorage` (device-scoped).
- **Same device**: `BroadcastChannel("vgd_master_broadcast")` + `storage` events sync open tabs instantly.
- **Cross device**: every broadcast is published to the `CLOUD_RELAY_TOPIC` ntfy.sh channel (SSE push + 45 s reconciliation poll + `sync-request` handshake on load). Packets ride through one choke point (`GLOBAL_NETWORK_EMITTER.broadcast`), so cancellations, announcements, and feedback all flow the same way. Attachments larger than the relay's 4 KB ceiling are deferred (`attachmentDeferred`) to the Formspree channel.
- **Feedback delivery** is three-fold: Formspree email (with real file attachment), the relayed CONSOLE inbox, and the local outbox.

## Development

The Tailwind CSS build is compiled and **inlined** into the `<style>` block of `index.html` (single-file constraint). To restyle:

```bash
npm install
npx tailwindcss -i input.css -o dist/tw.css --content index.html --minify
```

Then replace the marker region in `index.html` with the compiled CSS (or re-run your own inline pipeline). Vanilla JS only — no framework, no bundler.

## Community

- [Code of Conduct](CODE_OF_CONDUCT.md)
- [Contributing](CONTRIBUTING.md)
- [Security Policy](SECURITY.md)

## License

Released under the [MIT License](LICENSE) — © 2026 Cordillera School of Digital Arts, Inc.
