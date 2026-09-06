# V0.1 release acceptance

## Owner acceptance before first sale

1. Review support scope, code ownership and distribution license.
2. In a fresh customer-style environment run installation + offline demo.
3. Use separately supplied customer keys to discover a league/date.
4. Review one real fixture's GAME/TOTAL bindings and run through kickoff/halftime.
5. Confirm raw and normalized records, missing fields and actual request usage.
6. Interrupt and resume; verify no concurrent duplicate collector, durable records
   and manifest-readable gzip exports.
7. Confirm official settlement or explicitly unresolved replay exposure.

Suggested pilot support scope: macOS/Linux, Python 3.11+, 1–5 fixtures, one machine,
user-owned API subscriptions. No unlimited custom integrations, uptime SLA,
guaranteed league coverage or guaranteed financial result.

## Public showcase draft

Football Research Kit records football events and Kalshi orderbook snapshots in
a format designed for reproducible research. It includes explicit fixture review,
data-quality reporting and a small simulated replay example. Bring your own
provider subscriptions. The demo is synthetic; historical profitability is not a
product claim. We are preparing a limited technical pilot.

This draft has not been posted. No GitHub repository or public website has been
created. Publish only reviewed source/docs and synthetic demo artifacts, never
`runs/live`, local configs, PEM keys or production directories.
