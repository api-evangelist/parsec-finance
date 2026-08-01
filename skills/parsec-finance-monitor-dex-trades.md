---
name: Monitor DEX trades and trade metrics
description: Stream DEX trades, roll them into binned metrics, and pull OHLCV candles from the Parsec Finance API.
api: graphql/parsec-finance-api.graphql
operations: [Trades, TradeMetricsBins, GetCandles]
---

# Monitor DEX trades and trade metrics

Use the Parsec Finance GraphQL API to watch on-chain DEX activity for a pair or venue.

## Setup
- Endpoint: POST `https://api.parsec.finance/api/v2`
- Auth: `api_key` header (Pro plan).
- Request body: `{ "query": "<graphql>", "variables": { ... } }`.

## Steps
1. Pull the trade firehose with the `Trades` operation, filtering by `pair`, `venue`, `side`, `maker`/`taker`, and a `since`/`before` window; cap rows with `limit`.
2. Aggregate activity with the `TradeMetricsBins` operation over `chains`, `venues`, `pairs`, and `takers` at an `interval`, reading `volumeUsd`, `nTrades`, and `uniqueTakers` per bin.
3. Chart price with the `GetCandles` operation for a `pair` on a `venue` at an `interval`.

## Rules
- Read-only GraphQL queries; page with `limit` (+ `offset` where supported).
- Time bounds (`since`/`before`, `startTs`/`endTs`) are Unix seconds.
- Handle GraphQL `errors[]` and non-200 statuses as auth/plan failures.
