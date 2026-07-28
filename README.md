# KYC Desk — email agent walkthrough

An interactive, single-file HTML demo of an email-driven KYC agent for PRC counterparty
due diligence. A compliance analyst writes an ordinary email in plain English; the agent
reads it, highlights what it understood, and answers in the same thread.

The demo exists to show one thing clearly: **why a company search and a person search
end differently.**

| | Person search | Company search |
|---|---|---|
| Volume | ~6 records | ~318 records across 8 related tables |
| Deliverable | A verdict | A dataset |
| Delivery | Written into the email body | Excel workbook attached |
| Protection | Nothing leaves the thread | AES-256 encrypted (ECMA-376 agile) |
| Password | n/a | Generated per file, sent out-of-band on Teams |
| Lifecycle | Evidence retained in the case file | File and password expire after 14 days |

Both paths accept plain-English follow-ups in the same thread — including fragments like
"he" or "the two subsidiaries", which the agent resolves from earlier messages.

## Viewing it

Open `index.html` in a browser. No build step, no dependencies, no network calls — it is
one self-contained file.

Hosted on GitHub Pages: **Settings → Pages → Source: "Deploy from a branch"**, then pick
this branch with folder `/ (root)`. `index.html` sits at the root, so the site is served
directly with no extra configuration. `.nojekyll` is present so Pages publishes the files
as-is instead of running them through Jekyll.

## Driving the demo

- **Company search / Person search** — top right, switches tracks. Each starts over at step 1.
- **Next / Back**, the numbered step rail, or the **← →** arrow keys move through the walkthrough.
- On the company **Deliver** step the attached workbook opens locked. Type the password or
  press **Paste from Teams**, then click through the sheet tabs. A wrong password is rejected.
- The **◐** button switches light and dark; the page also follows the OS setting on load.

## Walkthrough steps

**Company search** — Compose → Parse → Confirm scope → Retrieve → Build workbook →
Encrypt → Deliver and unlock → Converse.

**Person search** — Compose → Parse → Confirm scope → Screen → Answer inline → Converse.

The narration strip at the bottom explains each step, and calls out in red wherever the two
guides diverge.

## Reading the highlights

On the **Parse** steps the analyst's own sentence is marked up with what the agent extracted.
Colour encodes the slot category, not emphasis:

- **Subject entity** — who or what is being checked
- **Identifier** — the value that pins them down (USCC, DOB, national ID, jurisdiction)
- **Check scope** — which checks were asked for, and over what window
- **Delivery & deadline** — output format, protection, and when it is due

The agent activity panel on the right mirrors the same slots as structured fields, so you
can see the sentence and the parse side by side. Nothing was filled in on a form.

## Notes

Every name, company, unified social credit code, case number and address is fabricated for
the demo, as are the source counts. Email addresses use the reserved `.example` domain. The
UI is a mockup — there is no mail server, no data source and no encryption actually running
behind it.
