# Security Policy

## Configuration secrets in `index.html`

This application is a single self-contained HTML file by design, which means its
configuration travels **in the page source** and is readable by anyone who can
open the page:

| Value | What it protects |
|---|---|
| `ADMIN_CONSOLE_PIN` | Access to the instructor CONSOLE (announcements, class cancellation) |
| `CLOUD_RELAY_TOPIC` | The shared ntfy.sh channel carrying class broadcasts, cancellations, and feedback |
| `INSTRUCTOR_FORM_ENDPOINT_ID` | The Formspree form that receives student feedback + attachments |

**Before publishing this repository publicly or distributing the link widely:**

1. Change `ADMIN_CONSOLE_PIN` to a private value and rotate it at the start of
   each term (or after any suspected leak).
2. Change `CLOUD_RELAY_TOPIC` to an unguessable topic name and redeploy to
   every device — the topic name is the *only* access control on the relay.
   Anyone knowing it can read or inject class traffic.
3. Keep the Formspree form id client-side-only (it is designed for browser use),
   but enable reCAPTCHA or domain restrictions in the Formspree dashboard if the
   form is abused. Student feedback may contain personal data — handle Formspree
   notification emails accordingly.

## Reporting a vulnerability

Please report security issues **privately** — do not open a public issue with
exploit details.

- Contact: **[INSERT CONTACT METHOD]** (project maintainer)
- Or use GitHub's **Private security advisories** on this repository
  (Security → Advisories → Report a vulnerability).

You will receive a response within 7 days. Please include reproduction steps
and affected devices/browsers.

## Supported versions

The `main` branch is the only supported version. There are no versioned
releases yet; deployments should track `main`.
