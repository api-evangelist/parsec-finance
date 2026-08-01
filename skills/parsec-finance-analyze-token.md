---
name: Analyze a token's holders and flows
description: Inspect a token's top holders, largest balance changes, and recent transfers via the Parsec Finance API.
api: graphql/parsec-finance-api.graphql
operations: [TokenHolders, TokenDeltas, Transfers]
---

# Analyze a token's holders and flows

Use the Parsec Finance GraphQL API to understand who holds a token and how balances are moving.

## Setup
- Endpoint: POST `https://api.parsec.finance/api/v2`
- Auth: `api_key` header (Pro plan).
- Request body: `{ "query": "<graphql>", "variables": { ... } }`.

## Steps
1. Get the largest holders with the `TokenHolders` operation using the token `address`, `chain`, and `limit`; each holder returns `balance`, `address`, and an `addressLabel`.
2. Find who is accumulating or distributing with the `TokenDeltas` operation using `address`, `chain`, `limit`, and a `lookback` window; read `delta`, `bought`, and `sold`.
3. Trace individual movements with the `Transfers` operation filtered to the token `address` and `chain`, paging with `limit` + `offset`.

## Rules
- Read-only GraphQL queries; `lookback` is a named window and time bounds are Unix seconds.
- Surface `addressLabel.label` when present to make holders human-readable.
- Handle GraphQL `errors[]` and non-200 statuses as auth/plan failures.
