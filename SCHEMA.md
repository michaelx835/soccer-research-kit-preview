# Schema v1

SQLite `records` stores `kind`, local Unix UTC receipt `ts`, SHA-256 digest and JSON
`payload`. Unique digests prevent replaying the identical row twice. A later
identical market response has a new receipt time and is intentionally retained.

| Record | Fields / interpretation |
|---|---|
| raw | provider, path, params, requested_ts, observed_ts, latency_ms, HTTP status, full successful JSON response |
| snapshots | fixture_id, kickoff, teams, league_id, score, status/minute, stats, events, markets, settlements, expected_tickers, config_digest |
| errors | provider/path or fixture/ticker, coarse error class and HTTP status; no credentials or raw error bodies |
| health | cycle length, recorded fixtures, poll overrun |
| discovery | raw fixture inventory and unreviewed market candidates for the selected UTC date |

Each market contains `ticker`, `kind`, explicit `line` or `outcome`, event ticker,
status, receipt `observed_ts` and bid ladders `book.yes`/`book.no`. Prices are
dollars in [0,1]; quantities are contract units, including fractional sizes.
YES ask = 1 − NO bid; NO ask = 1 − YES bid. Levels are sorted and units validated.
`source_ts=null` means the orderbook response does not establish exchange quote age.

`fixture_observed_ts`, `stats_observed_ts`, `events_observed_ts` are separate receipt
times. They must not be interpreted as the exact times events happened. Native
provider timestamps remain in raw records. Full event arrays are snapshots of
what the provider returned, not guaranteed complete event history.

Stats are keyed by provider team ID and original metric type. Missing metrics
are absent/null, not zero. Empty `{}` means no usable stats response, not no shots.
Quality counts shots/SOT/corners/xG per team; denominators include all snapshots,
including pregame periods when stats may legitimately be unavailable.

`settlements` uses official market result or settlement value when returned;
local football score is not a settlement oracle. Unknown/void/unavailable results
remain unresolved unless the response supplies a numeric settlement value.

Archive files are JSONL gzip grouped by kind and UTC receipt date. Read only files
listed in `archives/manifest.json`. SQLite is authoritative and exports can be rebuilt.
