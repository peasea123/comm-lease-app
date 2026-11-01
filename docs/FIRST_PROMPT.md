# Paste this into a Claude Code session on github.com/peasea123/arc-listing-hub

Read `CLAUDE.md`, `AGENTS.md`, the docs under `docs/` (especially the new `docs/LEASING.md`), and
the active `tasks/` phase file. Then build **steps 1–2 of the Build order in `docs/LEASING.md`**:

1. Additive Drizzle migration: `listing_spaces`, `lease_proposals`, and contact kinds `tenant` and
   `tenant_rep`. Leasing terms live in `listings.commercial.leasing` and are editable on the Edit
   tab. Seed The Masonic Building's spaces and terms from the "Masonic Building values" section
   (idempotent).
2. Public `/apply` on each lease listing's domain (masonic-building.arc-cre.com/apply):
   - access-code gate; the code is set and stored hashed per listing
   - the Parts A–C wizard, with finger signatures for the applicant and each guarantor
   - PDFs: review summary, application, a password-encrypted FCRA authorization per guarantor
     (`@cantoo/pdf-lib`), and the TransactionDesk lease data sheet
   - a `lease_proposals` row with `path = application`, linked contacts and private documents
   - email to the broker, an applicant confirmation, and the fee instructions (cash or check payable to the landlord, per
     the listing's `feePayment`; no online payment for now)
   - port the wizard, signature and PDF patterns from github.com/peasea123/new-ee-pack and
     github.com/peasea123/chl77-petition (copy; don't add a dependency)

Hard rules:
- SSN, DOB and DL # exist only in memory during submit and in the emailed encrypted PDF. Never in
  the DB, Blob, logs, activity, audit or sessionStorage.
- Nothing landlord-specific in code; it all comes from the listing's leasing terms.

Include a script that renders every PDF from fake data into `tmp-pdf-out/` for review. Follow the
repo's PR + blast-radius workflow. `/apply` is a new public path; it must not touch the existing
listing pages or inquiry flow.
