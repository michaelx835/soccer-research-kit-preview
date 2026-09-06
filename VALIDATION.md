# Validation evidence — 2026-09-06

## Passed locally

- Fresh virtual environment, wheel build and package installation.
- Installed CLI demo, config validation, HTML/JSON reports and gzip archives.
- 13 unittest checks: dollar/cents units and depth, crossed book rejection,
  win/loss/open outcomes, insufficient/stale quotes, future-outcome independence,
  deduplication/restart, gzip round trip, single-instance locking, missing stats,
  idempotent demo, team/kickoff guard, blocked order endpoint, mock complete
  collection, retry/error redaction, provider application errors, cursor loop,
  generated-key RSA-PSS GET signature (some tests cover multiple cases).
- Real unauthenticated HTTPS request to Kalshi EPL totals list: two returned
  markets. Real orderbook request parsed four YES and 35 NO price levels.
  Counts are a point-in-time smoke result, not ongoing coverage.
- HTTPS certificate issue in fresh Mac Python was reproduced and fixed with
  certifi. Certificate verification remains enabled; redirects are disabled.

The deterministic synthetic replay has 6 fixtures, 30 snapshots, 4 entries,
3 closed positions (1 win), 1 unresolved position. Closed simulated P&L is
-$0.88, unresolved entry cost including assumed fees $0.69. These numbers
validate bookkeeping, not strategy performance.

## V0.1.1 follow-up evidence

- 23 regression tests pass, including incremental archive idempotence, interrupted
  manifest recovery, strict config, rule drift, explicit review and offline doctor.
- Real API-Football league/date discovery returned two EPL fixtures on 2026-09-06.
- Bounded three-cycle joint test: Everton–Manchester United fixture 1557390,
  regulation 1X2 plus six totals; 27/27 expected market snapshots, zero request
  or collection errors, zero cycle overruns. Cycle durations 11.92–12.17 seconds;
  observed fixture intervals approximately 60 seconds.
- Both team shot/SOT/corner fields and event arrays were captured. Team xG was
  missing in all six team observations. Three market observations had an empty
  book side; all 27 REST books lacked exchange source-generation timestamps.
- Discovery ranking initially omitted the correct Arsenal–Chelsea TOTAL candidate
  because generic text similarity favored shorter rules. Pair/date mentions now
  rank ahead of generic similarity; a regression test covers this failure.
- Live discovery-to-bind workflow generated a rule-pinned configuration. Existing
  config files are preserved and explicit review is required.
- A second, longer pregame-to-settlement test is running separately; see PILOT_RUN.md.

## Not established

- Full-match cross-provider sampling and official settlement coverage for the
  new package (the three-cycle integration check does not establish this).
- Customer onboarding by an independent human, Windows support, production SLA,
  high-frequency throughput or a statistically supported trading edge.
- Permission to resell provider data, or finalized customer license/support terms.

No production VM change or trade was made during this product build.
