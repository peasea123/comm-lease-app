# Property config — `properties/<slug>.json`

One file per building/offering. The app serves each at `https://apply.arc-cre.com/<slug>`.
Adding a client or building = add a file + set its access code (see README). No code changes.

| Field | Type | Notes |
|---|---|---|
| `slug` | string | URL segment; must match the filename |
| `status` | `"open"` \| `"closed"` | closed shows "not accepting applications" |
| `property` | `{ name, address, county?, taxId?, yearBuilt?, listingUrl? }` | |
| `landlord` | `{ legalName, signer, signerTitle, noticeAddress, email?, shortName }` | `shortName` is used in sentences ("the Lodge", "the Owner") |
| `decision` | `{ body, schedule, cutoffNote? }` | who decides and when — shown to applicants and on the review summary |
| `broker` | `{ firm, agent, phone, email, represents }` | `represents`: Landlord / Tenant / Both |
| `delivery` | `{ packetEmail, ccEmails[] }` | where packets go |
| `terms.termMonths` | number | e.g. 36 |
| `terms.leaseType` | string | Modified Gross, NNN, Gross… |
| `terms.landlordPays` / `tenantPays` | string[] | expense split |
| `terms.rentEscalation` | `null` \| `{ percent, everyMonths }` | null = flat |
| `terms.advanceRentMonths` | `{ first, last }` | months of rent paid in advance at signing |
| `terms.securityDepositMonths` | number | deposit as months of rent |
| `terms.condition` | string | e.g. as-is, current configuration |
| `terms.improvements` | string | tenant improvement rules |
| `terms.personalGuaranty` | `{ required, ownershipThresholdPct }` | owners at/above the threshold must guarantee |
| `terms.applicationFee` | `{ amountUsd, refundable, covers, paymentUrl }` | `paymentUrl` = Stripe Payment Link for this fee (null hides the button) |
| `spaces[]` | `{ id, label, sf, monthlyRent, entrance?, mls?, realtracsUrl? }` | options the applicant can pick |
| `combinationRule` | `null` \| `{ allOf[], priceAs }` | e.g. all three suites price as "whole" |
| `propertyAcknowledgments[]` | string[] | extra checkboxes for this property (disclosures etc.) |
| `transactionDesk` | `{ lease, guaranty, disclosures[] }` | TAR form names the lease data sheet is built for |

**Derived (computed, never stored):** move-in funds = `monthlyRent × (first + last + securityDepositMonths)`;
term total = `monthlyRent × termMonths` (with escalation if set).
