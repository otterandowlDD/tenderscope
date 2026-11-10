# Construction Project Radar — MVP (2026-11-10)

## Scope
This feature-branch prototype extends TenderScope's procurement view to **early-stage construction opportunities**, without asserting that investment news equals an open tender.

- Visit `radar.html` to browse source-linked and reviewed leads.
- `data/opportunities.json` intentionally starts empty; do not populate it with illustrative companies or unverified projects.
- Search text, province, project type, and project lifecycle stage; export filtered records as CSV.
- No new dependencies, backend, automation, API tokens, scheduled jobs, or production changes.

## Discovery sources — manual review first
| Source | Signal | Important caveat |
|---|---|---|
| BOI (boi.go.th) | promoted investments, announced capital projects | approval is not construction commencement |
| ONEP Smart EIA (eia.onep.go.th) | EIA filings and decisions | an EIA report is not an open construction tender |
| DIW (diw.go.th) | industrial establishment / expansion information | registered factories are not necessarily new builds |
| IEAT / public procurement | estate infrastructure and government notices | distinguish plan, invitation, award, contract |
| SET filings / corporate IR | board-approved CAPEX and expansions | announcements can change, be delayed, or be cancelled |

Use human verification and respect site terms and rate limits before any future automated collection.

## Record contract
JSON root: `{ "schema_version": "0.1.0", "as_of": "YYYY-MM-DD", "records": [] }`.

Each record should have:
- `project_id`: stable unique ID, not just a name
- `project_name`, `owner`, `province`, `project_type`: FACTORY, WAREHOUSE, DATA_CENTER, INFRASTRUCTURE, HIGH_RISE, OTHER
- `stage`: PLANNING, EIA, PERMITTING, TENDER, AWARDED, CONSTRUCTION
- `source_url`: direct traceable public source (http/https)
- `source_date`: date source was issued (ISO 8601)
- `review_status`: VERIFIED or UNREVIEWED
- `checked_at`: date last reviewed, ISO 8601
- `next_action`: actionable follow-up, e.g., identify EPC (never invent contacts)
- Optional: `location_detail`, `estimated_value_thb` (only if sourced), `source_document_type`, `opportunity_scope`, `confidence_notes`, `owner_contact_source`

### Acceptance checks before publishing a record
1. Verify the actual primary document and its publication date; record URL and checked-at date.
2. Confirm project name, owner, province, scope, and lifecycle stage; leave unknown fields explicit rather than guessing.
3. Deduplicate by project ID and source-specific identifiers; multiple documents may refer to one project.
4. Don't label `TENDER` on the basis of EIA, investment approval, or an old procurement plan.
5. Do not assert that a foundation package is available without confirmation.
6. Keep personal phone numbers, private tender invitations, credentials, and private attachments out of the public repository.
7. Recheck sources and date/deadline before commercial outreach.

## Local smoke-test procedure (not yet executed by this PR)
1. Serve repository root with `python3 -m http.server 8000`.
2. Open `http://localhost:8000/radar.html` and confirm zero records and no JavaScript errors.
3. Confirm filter controls and empty CSV export behavior.
4. Populate a temporary, independently verified record for testing **without committing invented/demo data as a real lead**; confirm filtering, source link and CSV escaping.
5. Check mobile viewport and accessibility.

## Next steps
- Implement schema validation and basic tests.
- Develop reviewed source adapters where permitted; keep collection distinct from validation and publication.
- Add dated ingestion logs, source document change detection, and a review queue.
- Integrate with TenderScope's existing dashboard only after review, then optionally expose read-only API for Owlter.
