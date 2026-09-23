# Contributing

Thanks for helping improve the CSDA-TESDA Workspace Matrix.

## How to contribute

1. **Search existing issues** before opening a new one.
2. **Open an issue** describing the bug, the desired feature, and (if relevant) the device/browser used.
3. **Fork, branch, and open a pull request** against `main` once an issue is agreed upon (small typo/fix PRs may go direct).

## Project shape

- The entire application is **one self-contained `index.html`** — engine JS, markup, and the compiled Tailwind CSS inlined in the `<style>` block. Keep it self-contained (no CDN scripts, no external asset files; data-URIs are fine).
- The timeline engine constants (`START_DATE`, `END_DATE`, `HOLIDAYS`, `PHASE_MAP`) and the student-facing copy (alert strings, modal notices, feedback hub labels) are treated as **frozen content** — discuss in an issue before proposing wording changes.
- Views are rendered by `renderCalendar()` / `renderWeekView()` / `renderDayView()`; all cross-device traffic flows through `GLOBAL_NETWORK_EMITTER.broadcast()` — extend there rather than adding new transports.

## Style notes

- Vanilla JS (ES5-compatible constructs preferred), no framework, no build step required to run.
- Tailwind utility classes for layout; small custom classes live in the top `<style>` block (e.g. `.stripe-pattern`, `.fs-form`, `.alert-marquee`).
- After changing classes, recompile Tailwind (see README → Development) and re-inline the CSS.

## Pull request checklist

- [ ] `index.html` still runs standalone (open it directly in a browser).
- [ ] No secrets added (PINs, tokens, personal emails).
- [ ] Console/cancellation/feedback flows verified on at least one second device or private window.
