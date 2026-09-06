# Reddit post draft

Owner note, not part of the post: this is a pre-launch interest-check version, not a claim that checkout is open. Check the chosen community's rules and obtain moderator approval where required. Add a public documentation link only after publishing and reviewing it. Do not attach private logs, keys, customer data or provider archives. The proposed prices below need final license/support/refund terms before payments are accepted.

---

## Title

I spent months building football-market research tools. The hardest part wasn't the model—it was knowing whether the data was usable.

## Post

Disclosure: I built this, and I'm considering a small paid beta. I'm looking for feedback from people who actually collect or research football market data, not selling picks.

Over the past few months, I've spent a lot of time trying to understand football markets on Kalshi. I started where a lot of people probably start: watching matches, forming a theory, and wondering whether I could test it instead of arguing with myself about it.

What happens to the favorite's price after the underdog scores? Does a quiet first half make an under worth buying? Can you build a position over time and hedge it later?

Those sounded like strategy questions. A surprising amount of the work turned out to be plumbing.

The same club has different names across providers. A match can exist on one feed but not the other. Missing xG is not the same thing as zero xG. A price recorded before a goal is not a price you could necessarily buy after it. And an attractive backtest can look very different once you include spreads, fees, depth and the positions that never found an exit.

I've also experimented with GPT-assisted analysis. It didn't remove the need to get those basics right. A convincing explanation and a tradeable advantage are different things.

So I've started separating the useful infrastructure from my own trading experiments. The first standalone package is **Football Research Kit**: a local, read-only collector and research workflow for API-Football and Kalshi.

It currently lets you:

- Discover candidate fixture/market matches, then explicitly review the identities and settlement rules.
- Record match state, returned events and available team statistics alongside Kalshi orderbook snapshots.
- Keep raw responses and timestamps, with SQLite storage and incremental daily gzip archives.
- Inspect missing data, collection gaps and cross-market timing differences in an HTML report.
- Run a small example replay that includes spreads, displayed depth, fee assumptions and unresolved positions.

It does **not** place orders. It does not come with my accounts, API keys, private historical dataset or a profitable strategy. The replay is a simulation, not evidence that those orders would have filled.

The internal work has taken months; the standalone package is much newer. It's a command-line beta, not a polished consumer app. The preview has 23 automated tests and a short joint live-provider check on one fixture. That's encouraging engineering progress, but it's not evidence of flawless season-long coverage. Longer collection runs and independent onboarding are part of the next validation steps.

I'm considering these introductory prices:

- **$79 one-time:** the self-hosted research software package, source distribution under the final customer license, setup docs, synthetic demo and example reports.
- **$199 one-time:** the same package plus setup on one standard supported machine and seven calendar days of installation help.

Provider subscriptions and hosting are separate. Future major versions, live execution and unlimited support are not included promises. I'm checking interest before opening payments and finalizing the terms.

You'd need a Mac or Linux machine, Python 3.11+, some comfort with a terminal, and your own API access for real collection. The included synthetic demo needs no accounts. API-Football also has a free plan for initial access tests, but its request and season limits mean it isn't a free all-day live feed. [Official plan details](https://www.api-football.com/pricing).

I'm also working on collectors and execution tools for other sports and crypto. Those are separate projects, not features bundled into this football beta. Football comes first because it's the sport I personally enjoy following—even when a match makes me regret checking the score.

Next on my list: easier identity review, clearer coverage diagnostics and more complete-match validation. Higher-frequency capture and additional research workflows are possibilities after that, not promises to sell the current version on.

If you've built something similar, what was the most frustrating part? And if you would actually use a kit like this, would the value be in collection, matching, replay, or help getting it running?

I'd rather hear “this is missing the one thing I need” now than package the wrong product.
