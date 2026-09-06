# Approving one fixture

Discovery produces candidates only. No similarity threshold authorizes a match.
An operator compares the returned market text/rules with the provider fixture and
records evidence. Never use market close time as a substitute for kickoff.

Add an object like the following to `bindings`. All identifiers are synthetic:

```json
{
  "reviewed": true,
  "evidence": "Verified both clubs, regulation-time rules and kickoff against official fixture page on YYYY-MM-DD; include URL",
  "fixture_id": 123,
  "league_id": 39,
  "home_id": 10,
  "away_id": 11,
  "kickoff": "2026-09-07T19:00:00Z",
  "series": "KXEPLTOTAL",
  "event_ticker": "KXEPLTOTAL-SYNTHETIC",
  "scope": "regulation",
  "kind": "totals",
  "markets": [{"ticker": "KXEPLTOTAL-SYNTHETIC-3", "line": 2.5}]
}
```

For 1X2 use `kind: "1x2"` and markets with `outcome: "home"`, `"draw"`, or
`"away"` instead of `line`. A fixture can have separate GAME and TOTAL bindings.
Line numbers are actual goal thresholds, not inferred from ticker suffixes.
The collector checks IDs, series, event membership and kickoff within 5 minutes.
If the provider reschedules or corrects identities, review the binding again.

The JSON `reviewed` field is a human assertion, not cryptographic verification.
The toolkit cannot prove your interpretation of market settlement rules is right.
Scope is regulation only; to-advance, extra time and penalties are excluded.

No provider aliases or customer-specific mappings from the production system are
distributed. Explicit per-fixture bindings avoid propagating an incorrect alias.
Prospective automation needs its own identity tests and revision audit trail.

## CLI review workflow

After reading a candidate and its market rules, select its zero-based position
in the discovery `candidates` array. The following uses a synthetic ticker:

```sh
football-research bind --config config.local.json \
  --review-file runs/live/review-2026-09-07.json --candidate 0 --kind totals \
  --market KXEPLTOTAL-SYNTHETIC-3=2.5 \
  --evidence 'Both teams, kickoff, squads and regulation rules checked against official fixture URL on date' \
  --confirm-reviewed --output-config config.reviewed.json
```

For 1X2, use `--kind 1x2` and `--market TICKER=home`, `=draw` or `=away`.
Repeat `--market` to select multiple markets from the same candidate. To add
another event, use the previous output as `--config` and choose a new output
filename. Existing files are never overwritten by `bind`.

The generated market specifications pin `rules_sha256` from discovery. Collection
rejects that market if its rules change. The doctor command checks approved IDs,
kickoff and rules before a session. Manual bindings may omit the hash but lose
that protection. The config schema rejects secrets, unknown fields, duplicate
tickers, conflicting fixture identities, naive timestamps and integer-goal lines.
