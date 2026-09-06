# Dependencies, data rights and distribution

Core runtime: Python 3.11+ standard library (PSF-licensed Python distribution),
plus certifi (MPL-2.0) for a portable CA certificate bundle with HTTPS validation.
Packaging: setuptools (MIT). Optional signed HTTP authentication: cryptography
(Apache-2.0 OR BSD-3-Clause); its installed dependencies have their own licenses.
Dependencies are installed from the customer's environment, not vendored here.
Source/rights provenance is limited to the local modules examined in AUDIT.md.

No open-source or commercial redistribution license for this project's own code
has been selected. This is a private release candidate for owner review. Before
external delivery the owner must select written customer license/support terms.
Do not interpret availability of this preview as granting public redistribution.

All included demo data is newly fabricated. API-Football data may not be directly
resold without permission. Customers provide their own subscriptions. Kalshi
commercial redistribution rights must be checked separately. No bundled data
license or supplier affiliation is claimed.

Reference documents checked during implementation:

- https://www.api-football.com/terms
- https://www.api-football.com/documentation-v3
- https://docs.kalshi.com/api-reference/market/get-markets
- https://docs.kalshi.com/api-reference/market/get-market-orderbook
- https://docs.kalshi.com/getting_started/quick_start_authenticated_requests

Provider schemas and terms change. Current API docs do not guarantee every
league supplies live statistics, every book exposes a source timestamp, or
every public endpoint is available without authentication.
