# Commercial Lease Application — Requirements

**What it is:** a reusable, access-code-protected, mobile-first online application for prospective
commercial tenants, for any building or client Philip Seagraves (Blue Raider Realty / ARC Real
Estate Advisory) is leasing. Each property is a JSON config file in `properties/` and is served at
`/<slug>`. The applicant completes a wizard, every signer signs with a finger on the signature pad,
and the server builds a PDF packet in memory and emails it to that property's `delivery.packetEmail`.
**Nothing is stored** — same privacy model as `new-ee-pack` (Griffin's onboarding) and
`chl77-petition` (Lodge petition). The mailbox is the system of record.

- Config format: `docs/property.schema.md`. First property: `properties/masonic-building.json`
  (101 W Market St, Smithville TN; landlord Center Hill Lodge No. 77).
- **Nothing property-specific may be hard-coded** — names, rents, terms, fee, decision process,
  acknowledgments and TransactionDesk form names all come from the config. Use the landlord's
  `shortName` in sentences ("approved by the Lodge" / "approved by the Owner").

---

## 1. Users and flow

1. Broker sends a prospect the link `apply.arc-cre.com/<slug>` + that property's access code.
2. `/` is a neutral landing page (ARC branding, "use the link you were given"); it never lists
   properties. `/<slug>` asks for the access code; codes are per property (see §7).
3. Wizard (progress kept in the browser tab only).
4. Final step: review, acknowledgments, signatures (applicant signer + every guarantor).
5. Submit → server re-loads the config by slug, validates with zod, builds PDFs, emails the packet,
   and sends the applicant a confirmation email (no sensitive data).
6. Confirmation screen: application fee button (if `applicationFee.paymentUrl` is set), with
   `prefilled_email` and `client_reference_id` = application ID; then "what happens next" from
   `decision.schedule`.

Application ID: `{SLUG-PREFIX}-YYYYMMDD-XXXX` (random), on every PDF and in the email subject.
Config `status: "closed"` → the page says the property isn't accepting applications.

## 2. Wizard steps and fields

Required unless marked (opt). One zod schema shared by client and server.

### Step 1 — Space and use
- Space wanted: options from `spaces[]`, multi-select. For each show SF, monthly rent, term total,
  **move-in funds** (rent × (first + last + deposit months)), entrance, and a "See the listing" link
  (`realtracsUrl` or `property.listingUrl`). Apply `combinationRule` when it matches.
- Desired lease start date.
- Intended use (what the business will do in the space).
- Days/hours of operation; employees on site; expected customer visits per day.
- Planned alterations/improvements? Yes/No → description and estimated budget.
- Signage needs (opt); special utility needs (opt).
- Hazardous materials, cooking or open flame? Yes/No → describe.
- Business licenses or permits required for this use (opt).

### Step 2 — Tenant (the business)
- Applying as: existing business / new business (being formed) / individual (sole proprietor).
- Legal name; DBA (opt); entity type; state and date of formation; EIN (opt if not yet issued);
  Secretary of State control # (opt).
- Business mailing address, phone, email, website (opt).
- Years in business; current location (opt) + current landlord name/phone (opt) + why relocating (opt).

### Step 3 — Business plan and finances
- Business description: products/services, target customers, competition.
- Confirms the **move-in funds** (dollar amount for the selected space) will be available at lease
  signing — Yes/No — and their source.
- Start-up/move-in budget and **sources of funds** (personal savings, bank/SBA loan,
  investors/partners, existing business cash flow, other — with amounts).
- Projected monthly revenue for the first 12 months; main **sources of revenue**; months to break even.
- Existing contracts or customers supporting the projection (opt).
- Most recent year's revenue and net income (opt for new businesses).
- Optional uploads (photo/PDF): business plan, tax return, bank statements, P&L. Total under 25 MB.

### Step 4 — Owners and personal guarantors (1–4 people)
Shown when `terms.personalGuaranty.required`. Every owner at or above `ownershipThresholdPct` must be
listed and must guarantee. For each:
- Full legal name; title; ownership %; email; mobile phone.
- Date of birth; **SSN**; driver's license # and state.
- Current address + years there; previous address if under 2 years.
- Own or rent home; monthly housing payment.
- Employer/occupation and annual employment income.
- **Other income** rows: source (rental, investments, retirement, spouse, other) + annual amount.
- Assets (cash, real estate equity, retirement, other) and total liabilities — rough figures.
- Disclosures (Yes/No + explain): bankruptcy (7 yrs); foreclosure/repossession; judgments or liens;
  evicted or sued by a landlord; currently in a lawsuit.
- Agrees to personally guarantee the lease — must be Yes to submit online.

### Step 5 — References
- Bank: name, branch, contact name/phone (no account numbers).
- Prior or current landlord: name, phone, property address.
- Two business/professional references: name, relationship, phone.

### Step 6 — Acknowledgments (one checkbox each; all required)
Standard set, text built from the config (show the actual numbers):
1. This is a **{termMonths}-month lease with rent paid in monthly installments**. It is not
   month-to-month, and the tenant cannot end it early because plans change.
2. Approval is a **credit decision** by the Landlord, and **personal guaranties** from the owners are
   required (when `personalGuaranty.required`).
3. Rent is as listed for the space(s) selected, for the premises **{condition}**.
4. Improvements: {improvements}.
5. **Move-in funds** due at lease signing, before possession: first month's rent and last month's
   rent in advance plus a security deposit of {n} month(s)' rent — **${amount}** for the selected space.
6. Decisions: {decision.schedule} Submitting an application does not reserve the space or create a lease.
7. The **${fee} application and screening fee** is {non-refundable | refundable}.
8. Broker relationship: {broker.firm} represents the {broker.represents}.
9. Each entry in `propertyAcknowledgments[]`.
10. Consent to do business electronically and sign electronically (TN UETA / E-SIGN).
11. Certification that the information is true and complete.

### Step 7 — Signatures
- Applicant signer (authorized officer/owner) signs the application certification.
- **Each guarantor** signs on the same device in turn: (a) background and credit check authorization
  (§4) and (b) guaranty acknowledgment ("I will be asked to sign a personal guaranty, {transactionDesk.guaranty}").
- Record with each signature: printed name, timestamp (America/Chicago), IP, user agent.

## 3. PDF packet (emailed to `delivery.packetEmail`, cc `delivery.ccEmails`)

| # | PDF | Contents | SSN |
|---|---|---|---|
| 1 | **Review summary** (1–2 pp) for the decision body | Property, applicant, space, rent, term total, move-in funds + confirmed?, intended use, plan highlights, funding sources, guarantors with income/assets/disclosure flags, red-flags list, decision block (Approved / Declined / Approved with conditions, date, signature lines for the landlord signer) | none |
| 2 | **Commercial lease application** | Every answer, acknowledgments, applicant signature, audit trail | last 4 only |
| 3 | **Background & credit check disclosure + authorization**, one per guarantor | Stand-alone FCRA disclosure page, then the signed authorization with identity data | full — **encrypted PDF** |
| 4 | **Lease data sheet** (for TransactionDesk) | Everything to fill the config's lease and guaranty forms (default CF 422 / CF 443): landlord, tenant legal name/entity/notice address, guarantors, premises/space/SF/entrance, term, start date, monthly rent and schedule, term total, first/last month and deposit, move-in total, use clause, expense split, improvements clause | none |
| 5 | `application.json` | All answers except SSN/DOB/DL #, plus the property slug and computed figures | none |
| + | Uploads | as provided | — |

Email subject: `Lease application {ID} — {Property name} — {Tenant legal name} — {Space}`.

## 4. Background/credit authorization — compliance notes

- FCRA: the **disclosure must be a stand-alone document** and **written authorization** is required
  before pulling a consumer report on an individual guarantor.
- Authorized parties: "{landlord.legalName} (Landlord), its agent {broker.firm}, and any consumer
  reporting agency they engage," to evaluate this commercial lease application and personal guaranty.
- Include the CFPB "Summary of Your Rights Under the FCRA" link, and that reports may include
  credit, eviction, civil judgment, bankruptcy and criminal records.
- Adverse action notices are a broker process step (docs/PLAN.md), not app code.
- **SSN, DOB and DL #** appear only in PDF #3, **password-encrypted** with `@cantoo/pdf-lib`
  (`SCREENING_PDF_PASSWORD`). Never log them, never put them in email bodies, JSON, sessionStorage or
  errors.
- Have an attorney review the authorization and acknowledgment wording before wide use.

## 5. Payment

v1: Stripe **Payment Link** per property (`terms.applicationFee.paymentUrl`), no webhook; broker
reconciles by `client_reference_id`. v2 (later): Stripe Checkout + webhook.

## 6. Non-functional

- Next.js (App Router) + TypeScript strict + Tailwind, pdf-lib (+ `@cantoo/pdf-lib`), signature_pad,
  zod, Resend; patterns from `new-ee-pack` / `chl77-petition`.
- Property configs validated with zod at build time (a bad config fails the build) and loaded
  server-side; unknown slug → 404.
- Mobile-first, accessible; Month/Day/Year dropdowns for dates; resume after refresh
  (sessionStorage, never SSN/DOB/DL).
- ARC Real Estate Advisory branding (navy/teal/gold); page title "{property.name} — Lease Application".
- Hosting: Vercel (like Philip's other apps), domain `apply.arc-cre.com`.

## 7. Environment variables

| Variable | Purpose |
|---|---|
| `ACCESS_CODES` | JSON map of slug → access code, e.g. `{"masonic-building":"1234"}` (codes stay out of git) |
| `SESSION_SECRET` | Signs the session cookie (session is scoped to one slug) |
| `RESEND_API_KEY`, `FROM_EMAIL` | Email delivery (verified arc-cre.com sender) |
| `SCREENING_PDF_PASSWORD` | Opens the encrypted authorization PDFs |
| `DEFAULT_PACKET_EMAIL` | Fallback recipient if a config omits `delivery` |

## 8. Out of scope for v1
Database/storage, admin dashboard, automatic TransactionDesk entry (no public API — the broker's
Claude session fills the forms from the lease data sheet), online lease signing (TransactionDesk /
Authentisign later), Stripe webhooks. Possible later: pull spaces from the listings site
(listings.arc-cre.com) instead of a JSON file.
