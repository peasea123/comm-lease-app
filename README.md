# Commercial Lease Application — moved into the listing hub

**Decision (2026-11-01):** tenant intake is built **inside the listing hub**
(github.com/peasea123/arc-listing-hub, listings.arc-cre.com), not as a separate app. Applications,
letters of intent and broker proposals all land in each listing's workspace, next to contacts,
offers and documents. The landlord reviews them in the owner portal.

- Current spec: **arc-listing-hub `docs/LEASING.md`**
- This repo keeps the original requirements as reference (wizard fields, PDFs, FCRA notes,
  prospect message). Where they conflict, `docs/LEASING.md` in arc-listing-hub wins.
- Claude Code: start in **arc-listing-hub**, not here. Prompt: `docs/FIRST_PROMPT.md`.
