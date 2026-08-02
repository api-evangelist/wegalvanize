---
name: Ingest data into HighBond Results
description: Create a Results collection and analysis, define a table, upload records, then triage records by updating their status.
api: openapi/wegalvanize-highbond-openapi-original.yml
operations: [getCollections, createCollection, createAnalysis, createTable, uploadRecords, getRecords, updateRecordStatus]
---

# Ingest data into HighBond Results

HighBond Results organizes data as collections → analyses → tables → records.
JSON:API v1.0; `Authorization: Bearer <TOKEN>`; bodies are
`application/vnd.api+json`. Target the correct `apis-{region}.highbond.com`
host. Writes are not idempotent.

## Steps

1. **Find or create a collection** — `getCollections` to check for an existing
   collection; otherwise `createCollection`.
2. **Create an analysis** — `createAnalysis` inside the collection.
3. **Define a table** — `createTable` under the analysis, defining its columns.
4. **Upload records** — `uploadRecords` to load rows into the table.
5. **Read back** — `getRecords` to confirm the ingested rows and paginate with
   the `next_page_url` field.
6. **Triage** — `updateRecordStatus` to advance individual records through the
   review workflow (e.g. open → resolved).

## Notes
- Rate limit is 6 requests/second for most endpoints; batch uploads rather than
  looping per-row.
- Errors are JSON:API `errors[]` documents; inspect `status`/`detail`.
