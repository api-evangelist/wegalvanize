---
name: Build and test an audit/assurance project
description: Create a HighBond project, populate its objectives, risks, and controls, then record control testing and raise issues with remediation actions.
api: openapi/wegalvanize-highbond-openapi-original.yml
operations: [createProject, createObjective, createRisk, createControl, getControlTests, updateControlTest, createIssue, createAction]
---

# Build and test an audit/assurance project (HighBond)

HighBond is a JSON:API v1.0 REST API. Base URL is region-specific
(`https://apis-us.highbond.com/v1`, or the `apis-{region}` host matching the
instance). Authenticate with `Authorization: Bearer <TOKEN>` using a HighBond
API token minted in Launchpad. All bodies are `application/vnd.api+json`.
Writes are **not idempotent** — do not blindly retry a failed create; re-list
to confirm state first.

## Steps

1. **Create the project** — `createProject` under `/orgs/{org_id}/projects`.
   Capture the returned numeric project ID.
2. **Add objectives** — `createObjective` for each engagement objective within
   the project.
3. **Add risks** — `createRisk`, linking each risk to its objective.
4. **Add controls** — `createControl`, mapping controls to the objective/risk
   they mitigate.
5. **Review control tests** — `getControlTests` to list the tests generated for
   a control; record test execution/results with `updateControlTest`.
6. **Raise issues** — where testing finds a gap, `createIssue` on the project.
7. **Assign remediation** — `createAction` to attach a remediation action to
   the issue.

## Error handling
- `401` bad credentials → refresh the bearer token.
- `403` → the token's user lacks permission on the org/resource.
- `422` → invalid JSON:API attributes/relationships; fix the body.
- `429` → rate limited (6 req/s default); back off and retry after a pause.
