# Football Research Kit

### Collect the evidence before trusting the strategy.

A self-hosted, read-only research toolkit connecting football match data with Kalshi market snapshots. Bring your own provider access, review fixture identities, save an auditable dataset, and inspect its limitations before testing a trading idea.

**Status: V0.1.1 private preview / planned paid research beta.** This page is a publication draft. There is no public checkout yet. Software licensing, refund terms and support commitments must be finalized before accepting payment.

This is software for collecting and evaluating data—not a picks service, managed account, automatic trader or promise of positive returns.

## Why I built it

I have spent the past few months developing football-market collection and trading experiments. Football is the sport I personally enjoy most, so it became the place where I kept asking new questions—and discovering new ways a dataset could mislead me.

Some of the hardest problems were mundane: club-name variations, fixtures missing from one provider, a stale quote around a goal, and statistics that were absent rather than zero. Backtests needed to account for the spread, fees, actual displayed size and positions that remained open. GPT-assisted research did not make those problems disappear.

This package extracts a deliberately smaller, independent research workflow from that experience. The broader internal system has had months of development; this standalone distribution is new and has its own validation process. It does not contain every feature of my personal system.

## What you are buying

The planned beta is a downloadable, self-hosted software package, including Python source under the eventual customer license, installation instructions, configuration examples, a synthetic demo, data-quality reports and a fixed example replay.

| Introductory option | Proposed one-time price | Scope |
| --- | ---: | --- |
| Self-install research beta | **US$79** | Current software package and documentation; you install and operate it |
| Assisted setup beta | **US$199** | Same package, one standard Mac/Linux-machine setup, seven calendar days of installation assistance |

These are proposed pilot prices, not an active purchase offer. Final terms will specify permitted use, users/machines, updates, support and refunds. There is no lifetime-maintenance or future-major-version promise. See [the pilot terms draft](PILOT_TERMS_DRAFT.md).

Not included: API subscriptions, cloud hosting, trading capital, third-party historical data, personal production credentials, live order execution, GPT predictions, trading signals, custom strategy development or guaranteed coverage. Source delivery does not mean open-source licensing or permission to redistribute the package.

## Framework

```text
Your API-Football access          Kalshi market-data access
fixtures / events / statistics   market metadata / orderbooks
               \                 /
                Candidate discovery
                         |
              Human identity/rules review
                         |
               Bounded REST collection
                         |
            Raw records + normalized snapshots
                         |
           SQLite WAL + incremental gzip archives
                         |
          Quality report + example simulated replay
```

### Available in the preview

- **Discovery and review:** league/season/date discovery, text-ranked market candidates, explicit fixture-to-market bindings and recorded review evidence. Candidates are not automatically approved.
- **Match data:** fixture status, score and clock; full event arrays returned by the provider; per-team shots, shots on target, corners and xG when supplied. Missing statistics remain missing.
- **Market data:** metadata, market status and YES/NO bid ladders. Executable-side asks are derived from opposite-side bids; displayed prices and quantities are preserved.
- **Timing and diagnostics:** local receipt timestamps, request latency/errors, missing-market coverage, polling intervals and cross-market receipt skew.
- **Storage:** transactional SQLite, exact-record deduplication, collector locking, incremental UTC-day gzip chunks and an archive manifest.
- **Research:** standalone HTML/JSON quality reports and a fixed example replay with displayed-depth checks, fee assumptions, slippage and open-exposure accounting.

All provider operations in this package are GET-only. There are no balance, portfolio or order routes and no GPT calls.

### Important boundaries

The preview supports reviewed regulation-time 1X2 and half-goal totals. It does not support to-advance, extra-time or penalty markets. It is REST polling, not tick-by-tick or WebSocket capture.

Local receipt time is not necessarily the provider's source-update time. A fast HTTP response does not prove that the underlying information is fresh. Different markets are requested sequentially, not atomically. A snapshot may miss a price move between polls, and displayed depth does not guarantee a real fill.

There is no automatic alias approval, Telegram integration, GPT decision engine or live trading in this distribution. Missing xG cannot be reconstructed just by paying for a larger request quota.

## What you need to prepare

| Requirement | Offline demo | Real collection |
| --- | --- | --- |
| macOS or Linux, Python 3.11+ | Required | Required |
| Terminal familiarity | Basic | Basic configuration and log review |
| Internet | For installing dependencies | Throughout collection |
| API-Football account/key | Not needed | Your own direct-provider account |
| Kalshi access | Not needed | Access to the required market-data endpoints; optional signing credentials if needed |
| Disk and uptime | Small demo directory | Space for a growing database; machine awake and connected |
| GPT subscription/API credit | Not needed | Not needed |

Windows is not supported in this preview. Start with one fixture, then a small 1–5-fixture pilot; no enterprise-scale capacity guarantee is made. Running on a laptop is fine for testing, but sleep or network loss interrupts capture. A VPS is optional, not part of the purchase.

Never send account passwords, API keys or private signing keys to the seller or post them in an issue. Share only redacted diagnostics. Provider access and data-use eligibility remain your responsibility.

## Start free: two different tests

### 1. Run the included synthetic demo

Unpack the beta archive, enter its `football-research-kit` directory, then run:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install .
football-research demo --out runs/demo
```

Open `runs/demo/report.html` in a browser. After dependency installation, this demo needs no network or keys. It contains six fabricated fixtures, including losses, missing/stale observations, insufficient depth and unsettled exposure. These are test cases, not historical performance.

### 2. Register for API-Football and test provider access

1. Register directly at [dashboard.api-football.com/register](https://dashboard.api-football.com/register) and verify your email.
2. In the dashboard, find your key under **Account → My Access**. Keep it private.
3. Use the dashboard's **Live Tester** to make a small request, such as `/countries`, then test fixtures for a league and season your plan permits.
4. Check returned `errors`, available seasons and remaining quota before attempting collection. An HTTP success alone does not prove the requested data exists.

The direct-provider signup and dashboard workflow are described in the [official beginner guide](https://www.api-football.com/news/post/how-to-get-started-with-api-football-the-complete-beginners-guide). This toolkit uses the direct API-Sports host/header, not RapidAPI credentials.

As checked on September 6, 2026, the free plan is **100 requests/day**, requires no credit card, and has restricted season availability. All-endpoint access is not a guarantee that a specific current fixture or statistic is available. Confirm your allowed seasons in the dashboard; do not assume the example configuration's season is included. [Official pricing](https://www.api-football.com/pricing).

Free access is suitable for credential checks and a few bounded requests. It may not support an overlapping current Kalshi fixture. In that case, use the synthetic demo for the complete workflow and permitted historical data for the provider-only check. Do not pair an old football fixture with a current Kalshi market.

### Why a complete live match may need a paid plan

At the default 60-second polling interval, one active fixture uses roughly three football requests per cycle: fixture, statistics and events. A 100-minute session therefore needs about **300 football requests**, before discovery, diagnostics or retries. Kalshi calls are separate: roughly two per tracked market per cycle.

The free daily allowance is consequently not enough for this example. A ten-cycle one-fixture smoke test is roughly 30 football calls while active, plus setup calls; check your remaining allowance first. The client throttles requests, but it is not a complete account-wide quota budgeter—other programs using the same key count too.

Upgrade only after checking actual coverage and usage. Provider prices and entitlements can change; use the official dashboard rather than treating this README as a subscription contract.

## First real collection: step by step

### A. Set credentials locally

In a Mac/Linux bash or zsh terminal, enter the football key without echoing it or placing its value in shell history:

```sh
printf 'Paste API-Football key (input hidden): '
read -r -s API_FOOTBALL_KEY
export API_FOOTBALL_KEY
printf '\n'
```

Keep that terminal session open. Do not paste the value into configuration or Git. For unattended operation, use your own protected environment/secret-management setup.

Kalshi public market-data endpoints worked without authentication in our preview check. If your required access needs signed requests, install `python -m pip install '.[authenticated]'` and set `KALSHI_API_KEY_ID` plus `KALSHI_PRIVATE_KEY_FILE` to the local PEM path. Never disable TLS verification to work around an access error.

### B. Configure a league and discover a date

```sh
cp config.example.json config.local.json
football-research validate --config config.local.json
```

Edit the new JSON file to select the correct provider league ID, permitted season and Kalshi GAME/TOTAL series. The empty `bindings` list is intentional: discovery does not authorize collection of an unreviewed match.

For example, replace the date below with your desired **UTC** date:

```sh
football-research discover --config config.local.json --date 2026-09-07 --out runs/live
```

Read `runs/live/review-2026-09-07.json`. Confirm both teams, home/away identity, kickoff, competition and men's/women's/reserve/youth status. Read settlement rules. A market's close time is not a reliable substitute for kickoff.

### C. Approve only the correct candidate

Use the zero-based candidate index and actual market ticker from the review file. This is a template, not a real ticker to copy unchanged:

```sh
football-research bind --config config.local.json \
  --review-file runs/live/review-2026-09-07.json --candidate 0 --kind totals \
  --market ACTUAL_REVIEWED_TICKER=2.5 \
  --evidence 'Verified both teams, kickoff and regulation rules against official URL on date' \
  --confirm-reviewed --output-config config.reviewed.json
```

For 1X2 use `--kind 1x2` and `--market ACTUAL_TICKER=home`, `=draw` or `=away`; repeat `--market` for the selected outcomes. GAME and TOTAL events have separate bindings. To append another binding, use the previous output as input and a new output filename.

The command does not overwrite existing files. Generated bindings pin the observed rule hash; collection rejects a changed rule or mismatched identity rather than silently remapping it. Manual approval can still be wrong. See [binding details](BINDINGS.md).

### D. Diagnose, then run a small session

```sh
football-research doctor --config config.reviewed.json --out runs/live
football-research doctor --config config.reviewed.json --out runs/live --online
football-research collect --config config.reviewed.json --out runs/live --cycles 10
football-research report --out runs/live --fee-rate 0.07
football-research archive --out runs/live
```

The first doctor run is offline; `--online` consumes provider requests. Run reporting after collection stops to avoid the run lock. Keep demo and real sessions in separate directories. Ctrl-C preserves committed SQLite records; use the same collection command and directory to continue.

The `0.07` fee coefficient is an explicit example assumption, **not a universal Kalshi fee schedule**. Verify applicable fees before interpreting any replay. Report generation is not a real-order check.

### E. Inspect the output before using it

Open `runs/live/report.html`. Check request errors, expected versus recorded markets, missing statistics, collection gaps and cross-market timing skew. Keep the raw records and configuration alongside your analysis so another person can reproduce the inclusion rules.

Archives are incremental UTC-day gzip chunks. Follow the manifest; do not combine every file with a wildcard and accidentally include obsolete exports. SQLite history is not automatically deleted. The default free-disk safety threshold is 256 MiB, which is a stop safeguard, not a storage recommendation. Back up completed runs and monitor available disk.

## What the example replay actually demonstrates

The shipped rule observes a first-half 0–0 at minutes 25–29 and tests Over2.5 NO at 61–70 cents, subject to spread/depth requirements. It uses one entry per fixture, a 15-minute hold and a bounded exit window. If no eligible exit appears, it carries the simulated position toward official settlement; unresolved positions remain OPEN.

Entry/exit VWAP, a default one-cent slippage assumption and rounded simulated fees are included. See the [technical README](README.md) for the exact rule.

This rule is a reproducibility example, not the reason to buy the product. A positive result would still require timing checks, independent forward validation and execution evidence. A displayed-depth simulation is not a completed trade.

## Validation and remaining work

The preview passed **23 automated tests**, including archive recovery, identity checks and synthetic replay cases, plus a clean-package installation test. A short joint live check recorded **27/27 expected market snapshots across three cycles on one fixture**, with no request errors in that small check. Team xG was missing, illustrating why coverage must be reported rather than assumed.

This is limited engineering evidence—not a season-wide uptime, data completeness or profitability claim. Complete-match soak validation and independent user onboarding remain release gates. See [validation notes](VALIDATION.md) and [release checklist](RELEASE.md) for the evolving evidence.

## Roadmap: separate from what ships today

1. **Before general release:** complete-match acceptance, independent first-user installation, clearer failure diagnostics and finalized customer terms.
2. **Next research iteration:** easier fixture review, broader coverage tests, budget visibility and more reusable export/replay workflows.
3. **Later candidates:** higher-frequency capture, WebSocket support where appropriate, additional providers and separately validated modeling/execution integrations.

Roadmap items are not included delivery promises or guaranteed dates. I am also developing collectors and execution components for other sports and cryptocurrency markets. Those remain separate projects; buying this football research beta does not purchase a tennis, MLB, crypto or live trading bot. Football is the initial focus because it is my personal interest, not because I claim it is easy to beat.

## Data rights, security and support

You use your own subscriptions and remain subject to provider terms. No third-party raw historical archive is being resold with this package. API-Football restricts resale and may require additional rights for particular uses or publication; software ownership does not grant those data rights. [Provider terms](https://www.api-football.com/terms).

Neither API-Football nor Kalshi sponsors or endorses this project. This is research software, not financial advice. Do not deploy strategies with money you cannot afford to lose.

For a beta issue, provide package/Python version, operating system, the command, a redacted error and ideally a synthetic reproduction. Never include credentials or an unreviewed private database. Installation assistance does not include account management, strategy selection or guaranteed third-party uptime.

## Further reading

- [Technical quick start](README.md)
- [Schema and field meanings](SCHEMA.md)
- [Fixture binding guide](BINDINGS.md)
- [Architecture/extraction audit](AUDIT.md)
- [Validation](VALIDATION.md)
- [Release checklist](RELEASE.md)
- [Dependencies and rights notice](NOTICE.md)

If this page is moved to the repository root as its README, update its relative documentation links accordingly. Before publication, remove this editorial sentence and verify the final offer, support contact and public links.
