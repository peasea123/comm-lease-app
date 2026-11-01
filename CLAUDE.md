# Claude Code instructions — comm-lease-app

Reusable commercial lease application for any building/client. Read `docs/REQUIREMENTS.md`,
`docs/property.schema.md`, `properties/*.json`, `docs/OPEN_DECISIONS.md` and `docs/PLAN.md` before
writing code.

- Stack: Next.js (App Router) + TypeScript strict + Tailwind, pdf-lib (+ `@cantoo/pdf-lib` for the
  encrypted authorization PDF), signature_pad, zod, Resend. No database, no file storage. Deployed on
  Vercel.
- Next.js may be newer than your training data: read `node_modules/next/dist/docs/` before relying on
  an API, and heed deprecation notices.
- Reuse patterns from github.com/peasea123/new-ee-pack and github.com/peasea123/chl77-petition.
- **Nothing property-specific in code.** Names, rents, terms, fees, decision process,
  acknowledgments and form names come from `properties/<slug>.json`. Access codes come from the
  `ACCESS_CODES` env var, never from git.
- SSN, DOB and driver's license numbers: only in the encrypted authorization PDF. Never log them or
  put them in emails, JSON, sessionStorage or error messages.
- Validate every request server-side with the same zod schema the wizard uses; rate-limit submits.
- Mobile-first, accessible, large tap targets.
- Don't invent business facts: unresolved items go in `docs/OPEN_DECISIONS.md` with their default.
- Before each commit: lint, typecheck, build, and run the sample-PDF render script.
- Work on a branch, push after every commit, open a PR; the owner merges.
