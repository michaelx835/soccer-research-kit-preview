# Local extraction audit — 2026-09-06

No VM deployments, restarts, trading parameter changes or customer data exports
were performed to build this preview.

Inspected local production-related modules:

| Module | Finding | V0.1 decision |
|---|---|---|
| soccer_1x2_ws_collector.py | Depends on production context snapshots, auth module, process loop and local file naming | Do not copy; V0.1 uses self-contained REST polling; WS deferred |
| entity_mapping_store.py | Good provider ID / series scoping and reload design; store is customer-specific | Ship explicit fixture approval config, no production mapping database |
| legging_replay.py | Useful depth/VWAP and timing ideas; assumes specialized input and two-leg research | Implement documented one-entry example with open exposure and official settlement handling |

Implementation is newly written in this package using standard-library modules
and certifi for verified HTTPS on fresh Python installations.
Optional authentication imports cryptography; no vendored SDK, cloud CLI,
production Python environment, API response archive, positions or user journals
were copied. Source review here is not a legal ownership opinion.

## Architecture

```text
Customer environment keys + reviewed config
                |
       GET-only provider adapters
                |
       raw responses + error records
                |
  explicit fixture identity checks + normalized snapshots
                |
     transactional SQLite / daily gzip export
                |
   quality report + frozen simulated replay / HTML
```

Main deliberate reductions from the production system: no GPT name approval,
no automatic entry/exit orders, no strategy router, no online prediction, no
Telegram, no multi-sport support, no arbitrary automatic date matching. These
boundaries make installability and data interpretation testable.

## Remaining release gaps

- API-Football and Kalshi were jointly tested using the owner's available environment
  credentials on one live fixture; independent customer onboarding remains untested.
- Multi-hour live soak test, provider quota behavior and recovery across a real
  match reschedule require acceptance before paid launch.
- A clean-machine install does not substitute for a second human following README.
- Data-rights and code-ownership checks need owner review before commercial distribution.
- V0.1.1 replaced full export with incremental record-ID batches and atomic
  manifests. Replay now streams chronological records rather than loading all
  snapshot payloads; percentile summaries still retain numeric observations.
