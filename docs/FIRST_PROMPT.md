# Paste this into the first Claude Code session

Build the reusable commercial lease application described in `docs/REQUIREMENTS.md`. Read
`CLAUDE.md`, `docs/property.schema.md` and `properties/masonic-building.json` first. The app must
work for any building or client: every property is a JSON config in `properties/`, served at
`/<slug>`, with its own access code from the `ACCESS_CODES` env var. Nothing property-specific in code.

Same architecture as two existing apps of mine — PIN-gated Next.js wizard, signature_pad finger
signatures, pdf-lib PDFs built in memory, emailed via Resend, nothing stored:
- github.com/peasea123/new-ee-pack (Griffin's new-hire packet: wizard, signature pad, PDF layout
  helpers, session cookie, submit-route hardening)
- github.com/peasea123/chl77-petition (Lodge petition: background-check authorization step and a
  reviewer cover sheet)
Reuse their patterns and code where it fits (copy, don't depend on them).

Priorities, in order — I need to send the Masonic Building link to prospects today:
1. Config loading + zod validation, `/` landing page, `/<slug>` access-code gate.
2. Wizard steps 1–7 with validation and finger signatures for the applicant and each guarantor.
3. PDFs: review summary, full application, encrypted per-guarantor background/credit authorization,
   lease data sheet for TransactionDesk, application.json.
4. Submit route + Resend email to the property's packetEmail + applicant confirmation + fee link.
5. Mobile polish and README (setup, env vars, how to add a property).

Never log or email SSN, DOB or driver's license numbers outside the encrypted authorization PDF.
Unresolved items go in `docs/OPEN_DECISIONS.md` with the stated default. Include a script that
renders all PDFs from fake sample data into `tmp-pdf-out/` for review, plus a second sample property
config (fake, e.g. `properties/sample-retail.json` with status "closed") to prove nothing is
hard-coded. Work on a branch and open a PR when the MVP works end to end.
