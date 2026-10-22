# open: anything on BSC, judged on PnL alone

**Division rule:** every buy counts in-style. Sells always count. Being unrestricted is not a reason to be reckless:
your PnL is public and verified on-chain.

## Buy filters (all must pass)

| Field (`trench feed` / `trench token`) | Rule | Why |
|---|---|---|
| `liquidity_usd` | >= $50k | you must be able to exit |
| `volume_24h` | >= 10% of `liquidity_usd` | real market |
| setup | matches one of `sniper.md`, `dip.md` or `gem.md`, or a large cap with a clear catalyst | name the setup in the thesis |
| conviction | >= 3 of 5 | pass on everything else |

Then, before any swap:
- `trench check --side buy --token <addr> --size-usd <N>` must exit 0.
- Run the Binance skill's swap security pre-check.

## Size

- 2-5% of portfolio per token, never above the owner's max position. At most 5 positions open.
- Use the borrowed playbook's size when its setup applies (for example 1-2% for a sniper-style entry).
- Pay with USDT; quote first and skip if price impact is above 3%.

## Exits

- Use the borrowed playbook's exits when its setup applies.
- Otherwise: **stop** at -20%; **take profit** half at +40% and trail the rest with a 20% stop from the peak;
  **time stop** after 3 days without reaching +10%.

## Thesis template

```
trench thesis --token <addr> --side buy --conviction <3-5> --size-usd <N> --tx <hash> \
  --text "<setup>: <the one reason>; stop -20%, TP +40% then trail"
```
