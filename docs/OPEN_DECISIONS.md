# Open decisions — [CONFIRM]

Global decisions apply to the app; property decisions live in each `properties/<slug>.json`.

Defaults shown are what Claude Code will build if you don't answer. None of these block today's link.

1. **Screening vendor** (who actually runs the credit/background report). The app's authorization is
   vendor-neutral, so this can be decided after applications come in. Options: a commercial tenant
   screening service that accepts a signed authorization; a credit bureau business account; or an
   applicant-initiated service. *Default: decide later.*
2. **Fee per application or per guarantor?** *Default: $99 per application, covering up to two
   guarantors.*
3. **Rent escalation** (per property). *Masonic Building default: none — flat rent.*
4. ~~Security deposit~~ **Decided:** security deposit = one month's rent, plus first and last
   month's rent in advance — 3 months' rent due at lease signing.
5. **Application cutoff** (per property). *Masonic Building default: the Tuesday before the first-Monday meeting.*
6. **Domain.** *Default: apply.arc-cre.com.*
7. **Where packets go** (per property `delivery`). *Default: philip@chanpart.com.*
8. **Minimum ownership % that must guarantee** (per property). *Default: 20%.*
9. **Attorney review** of the FCRA authorization and acknowledgment wording before wide use.
   *Recommended; doesn't block a small first batch.*
