---
name: Track a wallet's on-chain activity
description: Resolve a wallet handle or address and pull its transfers and decoded transactions from the Parsec Finance API.
api: graphql/parsec-finance-api.graphql
operations: [Address, Transfers, ApiTxs]
---

# Track a wallet's on-chain activity

Use the Parsec Finance GraphQL API to profile a wallet's recent on-chain behavior.

## Setup
- Endpoint: POST `https://api.parsec.finance/api/v2`
- Auth: send your key in the `api_key` header (prefix `par_ak_`, provisioned in the Parsec app under Settings -> Subscription; requires a Pro plan).
- Request body: `{ "query": "<graphql>", "variables": { ... } }`.

## Steps
1. If you have a Parsec handle rather than a raw address, resolve it with the `Address` operation (`name` -> `address`).
2. Pull token/native movements with the `Transfers` operation, filtering by `address`, `chain`, and a `since`/`before` Unix-seconds window; page with `limit` + `offset`.
3. Pull decoded transactions with the `ApiTxs` operation using `addresses: [<address>]`, optional `chains`, `functionCall`, `to`/`from`, and `since`/`before`; page with `limit` + `offset`.

## Rules
- All three are read-only GraphQL queries — no idempotency key is needed.
- Timestamps and `since`/`before` bounds are Unix seconds.
- On failure the response carries a GraphQL `errors[]` array; a non-200 HTTP status means the key or plan is invalid.
