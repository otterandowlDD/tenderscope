# TenderScope — Procurement Intelligence

Independent, evidence-first construction procurement intelligence product. **Not part of Owlter's runtime or repository.** Owlter may integrate through a future versioned API without being a required dependency.

## V1 deployed scope

- Static, responsive dashboard with search, status filter, CSV export and source-linked project details.
- One historical IEAT Smart Park spillway case study sourced from the plan and award documents. **Not a live tender feed.**
- No customer accounts, payments, scraper, hosted backend, external API, or automatic tender verification yet.
- Zero build dependencies; Vercel serves `index.html` and `data/projects.json`.

## Architecture boundary

```
IEAT/e-GP → [future Collector] → [future Validator] → data/projects.json → Frontend
                                                      ↳ future versioned API → optional Owlter consumer
```

If source collection is unavailable, the deployed site continues serving its last reviewed snapshot. The collector must never mark a project `TENDER_OPEN` without evidence of current invitations and a checked deadline. Plan IDs and procurement IDs may differ; deduplicate by source document and record provenance.

## Future modules

- `collector/`: rate-limited public notice ingestion; no credentials in frontend.
- `validation/`: date normalization, cp874/TIS-620 decoding, document-stage classification and manual review.
- `api/v1/`: future contract for read-only procurement records.
- `web/`: independent frontend, currently at project root.

## Data corrections

The currently displayed case is historical (May 2026), award announced. Contract signature and subcontracting availability are **unverified**. The planned budget and award price are different document-stage values, not profit data. Before outreach, review both official source documents.

## Deployment

Connect this repo to a separate Vercel project `tenderscope`. No environment variables or build command are necessary.
