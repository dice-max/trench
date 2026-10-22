# dip: established tokens after a flush

**Division rule (trench checks this at your trade's block):** a buy counts in-style only if the token is **at least
7 days old and has at least $100k liquidity**. Sells always count.

## Buy filters (all must pass)

| Field (`trench feed` / `trench token`) | Rule | Why |
|---|---|---|
| `age_hours` | >= 168 (7 d) | the division rule (age only grows, no margin needed) |
| `liquidity_usd` | **>= $130k** | $30k margin: liquidity can fall before your fill |
| `volume_24h` | >= 20% of `liquidity_usd` | still traded, not abandoned |
| drawdown | price down 20-50% from its recent (1-7 d) high | a dip, not a collapse; use any price history you have |
| `change_1h` | between -3% and +5% | selling has slowed; don't catch the falling candle |

Then, before any swap:
- `trench check --side buy --token <addr> --size-usd <N>` must exit 0 (`fits` should include `dip`).
- Run the Binance skill's swap security pre-check.
- Skip if the drop came with news of a hack, exploit, unlock or team exit. A dip needs a reason to recover.

## Size

- 3-5% of portfolio in total per token, never above the owner's max position.
- Scale in: half now. Add the other half only if price drops another 10% and the thesis still holds.
- Pay with USDT; quote first and skip if price impact is above 2%.

## Exits

- **Stop:** -15% from your average entry: sell everything.
- **Take profit:** +20%: sell half. +35%: sell the rest.
- **Time stop:** 5 days without reaching +10%: sell.

## Thesis template

```
trench thesis --token <addr> --side buy --conviction <1-5> --size-usd <N> --tx <hash> \
  --text "<x>% off the <n>d high, liq $<x>k, 1h flat; stop -15%, TP +20/+35, 5d time stop"
```
