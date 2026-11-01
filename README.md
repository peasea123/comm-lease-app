# Commercial Lease Application

Reusable, access-code-protected online application for prospective commercial tenants of any
building Philip Seagraves (Blue Raider Realty / ARC Real Estate Advisory) is leasing. Each property is
a config file in `properties/` served at `apply.arc-cre.com/<slug>`. Applicants complete a
mobile-friendly wizard and sign with a finger; the server emails a PDF packet (review summary,
application, encrypted background/credit authorizations, TransactionDesk lease data sheet). Nothing
is stored.

- Requirements: `docs/REQUIREMENTS.md`
- Property config format: `docs/property.schema.md` — first property: `properties/masonic-building.json`
- Plan and next steps: `docs/PLAN.md`
- Open decisions: `docs/OPEN_DECISIONS.md`
- First Claude Code prompt: `docs/FIRST_PROMPT.md`
- Prospect message template: `docs/PROSPECT_MESSAGE.md`

## Add a property
Copy a config in `properties/`, edit it, add its access code to `ACCESS_CODES` in Vercel, redeploy,
and send `apply.arc-cre.com/<slug>` + the code.
