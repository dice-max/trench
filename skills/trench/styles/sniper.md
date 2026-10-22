# sniper: new pairs, fast in, fast out

**Division rule (trench checks this on-chain at your trade's block):** a buy counts in-style only if the token's pair
is **less than 24 h old**. Sells always count.

## Buy filters (all must pass)

| Field (`trench feed` / `trench token`) | Rule | Why |
|---|---|---|
| `age_hours` | **< 20** (sweet spot 0.5-12 h) | 4 h margin so a slow fill still lands under 24 h |
| `liquidity_usd` | >= $20k, and >= 5% of `mcap_usd` | you must be able to exit |
| `mcap_usd` | $30k to $3M | below = rug lottery, above = late |
| `change_1h` | between -20% and +150% | don't buy a vertical candle as exit liquidity |
| `volume_24h` | >= `liquidity_usd` | real two-sided trading, not one wallet |

Then, before any swap:
- `trench check --side buy --token <addr> --size-usd <N>` must exit 0 (`fits` should include `sniper`).
- Run the Binance skill's swap security pre-check. Any honeypot, sell-tax or ownership red flag: skip.
- Hold at most 3 sniper positions, one per token. Never average down on a sniper entry.

## Size

- 1-2% of portfolio per entry, never above the owner's max position.
- Pay with USDT (`0x55d398326f99059fF775485246999027B3197955`) so PnL is easy to read.
- Quote first (`baw market-order quote`). If the quote's price impact or slippage is above 8%, halve the size or skip.

## Exits (check every loop, before new buys)

- **Stop:** -30% from entry: sell everything.
- **Take profit:** +60%: sell half. +150%: sell the rest, or trail it with a 25% stop from the peak.
- **Time stop:** 6 h after entry and still under +10%: sell.
- **Liquidity drain:** pool liquidity down 40% since entry: sell everything, now.

## Thesis template

```
trench thesis --token <addr> --side buy --conviction <1-5> --size-usd <N> --tx <hash> \
  --text "<age>h old, liq $<x>k, <the one reason>; stop -30%, TP +60/+150, 6h time stop"
```
