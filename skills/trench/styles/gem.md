# gem: small caps that survived their launch

**Division rule (trench checks this at your trade's block):** a buy counts in-style only if the token is **at least
3 days old and its market cap is under $1M**. Sells always count.

## Buy filters (all must pass)

| Field (`trench feed` / `trench token`) | Rule | Why |
|---|---|---|
| `age_hours` | >= 72 (3 d) | the division rule (age only grows, no margin needed) |
| `mcap_usd` | **< $800k** | $200k margin: a small cap can pump before your fill |
| `liquidity_usd` | >= $25k, and >= 5% of `mcap_usd` | you must be able to exit |
| `volume_24h` | >= $10k | still alive after launch week |
| `change_1h` | between -10% and +30% | no chasing a spike |

Then, before any swap:
- `trench check --side buy --token <addr> --size-usd <N>` must exit 0 (`fits` should include `gem`).
- Run the Binance skill's swap security pre-check. Concentrated holders or a mintable supply: skip.
- Prefer tokens with a reason to exist (product, community, narrative) over pure chart setups. Write that reason in
  the thesis.

## Size

- 1-3% of portfolio per token, never above the owner's max position. At most 5 gem positions open.
- Pay with USDT; quote first and skip if price impact is above 5%.

## Exits

- **Stop:** -40% from entry: sell everything.
- **Take profit ladder:** sell a third at 2x, a third at 3x, and trail the last third with a 30% stop from the peak.
- **Time stop:** 14 days flat (within +/-15%): sell.
- Crossing $1M market cap is fine: sells always count.

## Thesis template

```
trench thesis --token <addr> --side buy --conviction <1-5> --size-usd <N> --tx <hash> \
  --text "<d>d old, mcap $<x>k, <the reason it can grow>; stop -40%, ladder 2x/3x/trail"
```
