# CSDA-TESDA Workspace Matrix — RELEASE MANIFEST

## v1.0.0 — LIVING BASELINE (UNLOCKED FOR UPDATE)

- **Status:** UNLOCKED — instructor opened this baseline for updates; artifacts below track the
  latest shipped build. Re-freeze on request to pin a new hard checkpoint.
- **Updated:** 2026-09-23 (Asia/Manila) — relay-safe telemetry + 429-resilient broadcast fix

### Current artifacts

| File | Size | SHA-256 |
|---|---|---|
| `CSDA-TESDA Workspace Matrix.html` | 272,676 B | `9c85879714015634ac6dc2854bac6849b7076c18e8d0e9c496674304aa5426c3` |
| `CSDA-TESDA-Workspace-Matrix.zip` | 123,380 B | `b44ab6977a24a852a0cb9043c15c92bac829e8db969e358d312520a4d04593d8` |

> Checksums are re-stamped into this table on every update; run `sha256sum` on the two files
> beside this manifest to verify.

The HTML is byte-identical to the live deliverable and to `index.html` inside the zip
(GitHub repo bundle, 15 files: README, LICENSE (MIT), CODE_OF_CONDUCT, CONTRIBUTING,
SECURITY, .gitignore, package.json, tailwind.config.js, input.css, issue + PR templates).

### Change log

- **2026-09-23 — WALLPAPER ALL THE WAY BEHIND (task 34):** art layer moved out of the
  calendar card to the first element of `<body>` — `position: fixed; z-index: -1` page
  wallpaper behind everything. The calendar card keeps its pristine opaque `ios-card`
  surface (class restored exactly; in-card stacking rules retired). Studio preview now
  shows the clean calendar box floating on the wallpaper.
- **2026-09-23 — CALENDAR BACKGROUND STUDIO (task 33):** `🖼️ Calendar Background Studio`
  in the Instructor Console (above the announcement block) opens a full art editor:
  📷 Upload (auto-downscale ≤1600px JPEG) or image URL, fit modes Cover/Contain/Tile/
  Stretch, Zoom (100–300%), Pan X/Y, Opacity, Dim (readability), Blur, Rotate 90° steps,
  live preview with sample tiles on top, Apply & Save / Remove. Art renders in
  `#calendarBgLayer` strictly BEHIND the tiles (`z-index:0`, `pointer-events:none`,
  content stack `z-index:1`) — tiles never overlapped or blocked. Persists in
  `vgd_calendar_bg` per device (ntfy's 4KB/push cap cannot carry image data class-wide).
- **2026-09-23 — MOBILE HEADER CLIPPING FIX:** telemetry label words (`Downloads` /
  `Installs` / `Active Mobiles`) now `hidden sm:inline` — words hidden on phones,
  verbatim in DOM with chip tooltips retained; chip spacing `space-x-2 sm:space-x-4`;
  static fake seeds `1,420/1,180/432` → `0` (real-counts-only rule — values render
  device-derived immediately). Width proof: 320px at 375 viewport (was ~460px+).
- **2026-09-23 — RELAY-SAFE FIX (broadcasts/cancellations "not pushing through"):**
  root cause = 25 s presence heartbeats (3,456 msgs/device/day) exhausting ntfy.sh's
  **250 msgs/day per visitor IP** (classroom NAT shares one bucket) → every instructor
  publish 429'd and silently dropped. New cadence: free BroadcastChannel beats (25 s),
  cloud presence event-driven + budget-capped (45 min, ≤6/day), compact `p` self-row
  piggybacked on every state packet, 429 backoff (30 min) + critical-state retry outbox
  (≤8) that flushes when the window re-opens. Structural sync window = 60 min.
  Proven by a 3-device mock-relay simulation enforcing the real limits: 3/3 announcements
  + cancellation delivered, 24/250 day-quota used, 0 × 429, queued push delivered after
  forced exhaustion (old cadence counterfactual: 2,592 msgs/day = guaranteed starvation).
- **2026-09-23 — Creators' Terminal:** `#adminTerminalLaunchBtn` ("📟 Creators' Terminal",
  `font-mono`, obsidian block) in `#adminConsole` directly above the Broadcast command row;
  `hidden md:flex` responsive firewall (laptop+ only); `launchCreatorsTerminalOverlay()`
  routing stub with the verbatim diagnostic log.
- **2026-09-23 — Real telemetry + footer theming; v1.1.0 fold-in:** presence-based metrics,
  header telemetry grid, runtime Dark/Light toggle, contrast layer, body-docked dropdown,
  theme engine (DARK_HUD / NEUMORPHIC / DUAL_TONE).

### Feature state

- Single self-contained HTML (Tailwind compiled inline, offline-ready); engine verbatim
  (2026 calendar Sep 8 – Nov 26, 3 holidays, Mon–Fri, cancellation engine).
- Month / Week / Day views, viewport-anchored dropdown, ticker loop with merged legend.
- Date modal at top; `Log in to BSRS` beside the title; Mark Class Cancelled in CONSOLE.
- CONSOLE PIN `csda2026`; emergency cancellation (Sunday guard); cross-device sync
  (BroadcastChannel + storage + ntfy topic `vgd-csda-b92c7f5a18b64065`) with quota-safe
  relay budgeting and 429 backoff + state retry.
- "VGD message" broadcast pop-out + notification ask (`vgd_notif_pref`); Formspree feedback
  (`mkjgbnzk`) + Trainer Feedback Pipeline + Student Feedback Inbox.
- Theming: `ACTIVE_THEME = "DARK_HUD"` (NEUMORPHIC / DUAL_TONE) + runtime Dark/Light toggle
  (`vgd_theme_pref`); real presence telemetry (Downloads/Installs/Active Mobiles);
  Creators' Terminal launcher stub.

### Open flags (carried forward)

1. PIN literal: kept `csda2026` (spec notes say "password ('csda')" — confirm if literal `csda` is wanted).
2. LICENSE is MIT — earlier note about a license swap is still unresolved.
3. `[INSERT CONTACT METHOD]` placeholders in community files not yet filled.
4. PIN and relay topic are readable in source (class-tool trade-off — re-verify before public release).
5. `launchCreatorsTerminalOverlay()` is a routing stub — the actual terminal overlay is future work.
6. Spec's "✨ Broadcast & Push Sync Network" button maps to this console's `Broadcast`
   control (`registerAdminAnnouncement()`); launcher sits directly above its command row.
7. Free ntfy.sh quotas are shared per IP — a school-wide rollout may need a paid ntfy tier
   or self-hosted relay for headroom (current cadence fits a class day comfortably).
