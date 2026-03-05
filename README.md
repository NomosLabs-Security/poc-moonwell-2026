# Moonwell Exploit PoC -- cbETH Oracle Misconfiguration

> **Educational Purpose Only** -- This PoC is created for security research and education
> purposes only. It is a simplified simulation, not a fork replay against mainnet.

## Overview
- **Date:** 2026-02-15
- **Loss:** ~$1.78M (bad debt)
- **Chain:** Base
- **Category:** Oracle Misconfiguration

## Vulnerability
Moonwell governance proposal MIP-X43 deployed a cbETH oracle that used only the raw cbETH/ETH exchange rate (~1.12) without multiplying by ETH/USD price (~$2,200). This valued cbETH at $1.12 instead of ~$2,464. Liquidation bots seized 1,096 cbETH within minutes. The five-day governance timelock prevented an immediate oracle fix. Oracle code was co-authored by Claude Opus 4.6.

## Reproduction
```bash
git clone https://github.com/NomosLabs-Security/poc-moonwell-2026
cd poc-moonwell-2026
forge install foundry-rs/forge-std --no-git
forge test -vvvv
```

## Key Contracts
- `VulnerableCbEthOracle`: Returns cbETH/ETH ratio directly (missing * ETH/USD)
- `FixedCbEthOracle`: Correct composite price with sanity bounds
- `SimpleLendingPool`: Compound-style lending with liquidation mechanics

## References
- [Rekt News -- Moonwell Rekt](https://rekt.news/moonwell-rekt)
- [CoinDesk -- Oracle Error on Moonwell](https://www.coindesk.com/tech/2026/02/18/ether-briefly-priced-at-usd1-on-defi-app-moonwell-glitch-triggering-usd1-8m-in-bad-debt)
- [Moonwell Governance Forum -- MIP-X43 Incident Summary](https://forum.moonwell.fi/t/mip-x43-cbeth-oracle-incident-summary/2068)

## License

MIT -- For educational use only.
