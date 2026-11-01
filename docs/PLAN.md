# Plan — get a working application link to prospects today

## Who does what
- **Philip**: accounts and decisions (GitHub repo, Stripe link, Vercel, DNS, final wording).
- **Claude Code (cloud)**: builds the app in this repo from `docs/REQUIREMENTS.md`.
- **Claude (Cowork session)**: writes requirements, reviews Claude Code's output, tests with fake
  data, then fills CF 422 / CF 443 in TransactionDesk for each applicant.

## Today

| # | Step | Who | Time |
|---|---|---|---|
| 1 | ~~Create GitHub repo~~ done. | Philip | — |
| 2 | ~~Push docs + `CLAUDE.md`~~ done. | Claude | — |
| 3 | Create a Stripe **Payment Link**: "Commercial Lease Application & Screening Fee", $99, one-time, collect name + email. Paste the link to Claude (it goes in `properties/masonic-building.json`). | Philip | 10 min |
| 4 | Start a Claude Code cloud session on the repo and paste `docs/FIRST_PROMPT.md`. | Philip | 2 min |
| 5 | Claude Code builds the app and opens a PR. | Claude Code | ~2–3 hrs |
| 6 | Answer the `[CONFIRM]` items in `docs/OPEN_DECISIONS.md` while it builds. | Philip | 15 min |
| 7 | Create a Vercel project from the repo; set env vars (REQUIREMENTS §7, incl. `ACCESS_CODES`); add domain `apply.arc-cre.com`; CNAME at IONOS. | Philip (Claude can do Vercel parts) | 15 min |
| 8 | End-to-end test with fake data on a phone: every PDF, the encrypted PDF password, fee link, confirmation email. | Claude + Philip | 30 min |
| 9 | Send prospects `apply.arc-cre.com/masonic-building` + its access code (text/email template in `docs/PROSPECT_MESSAGE.md`). | Philip | 5 min |

## After each application arrives
1. Open the packet; decrypt the authorization PDF; confirm the $99 posted in Stripe.
2. Order the background/credit report from the screening vendor (decision #1) using the signed
   authorization.
3. Ask Claude to set up the TransactionDesk transaction: `<Tenant> - <Property address> <Space> Lease`,
   contacts (Landlord and its signer from the config; Tenant; Guarantors), the config's lease and
   guaranty forms (CF 422 + one CF 443 per guarantor) and any disclosures (CF 305 for the Lodge); fill from the lease data sheet; stage the Authentisign signing
   **without sending**.
4. Bring the review summary (+ report results) to the decision body (the Lodge: first-Monday meeting).
5. Approved → send the signing. Declined based on a report → send an FCRA **adverse action notice**
   (name/contact of the reporting agency, that it didn't make the decision, right to a free copy
   within 60 days and to dispute). Claude can draft it.

## Timing note (Masonic Building)
Nov 2, 2026 is tomorrow's first Monday — too soon for these applicants. Plan for the
**Monday, December 7, 2026** meeting; set an application cutoff [CONFIRM, e.g. Dec 1].

## Adding another client or building later
1. Copy `properties/masonic-building.json` to `properties/<new-slug>.json` and edit it
   (`docs/property.schema.md`). Claude can draft it from the listing or listing agreement.
2. Create a Stripe Payment Link for that property's fee if it differs; put the URL in the config.
3. Add the slug's access code to `ACCESS_CODES` in Vercel and redeploy.
4. Send `apply.arc-cre.com/<new-slug>` + code. Set `status: "closed"` when the space is leased.
