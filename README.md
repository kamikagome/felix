# FELIX Token & Treasury Onchain Analytics Dashboard

## Executive Summary

This repository contains comprehensive onchain analytics for the **FELIX token** (0xf30bf00edd0c22db54c9274b90d2a4c21fc09b07) and its **Treasury Wallet** (0x778902475c0B5Cf97BB91515a007d983Ad6E70A6) on the Base blockchain. The integrated Dune dashboard provides real-time visibility into token economics, treasury flows, trading dynamics, and holder distribution through 7 optimized SQL queries and 16 visualizations.

**Dashboard:** https://dune.com/blockchain_explorators/felix-token-treasury-onchain-analytics-2026

---

## Dashboard Overview

## Query Architecture & Methodology
### Query 1: Token Supply & Burn Tracking
### Query 2: Daily Transfer Volume & User Activity
### Query 3: Treasury Fund Accumulation
### Query 4: DEX Trading Volume & Liquidity Pool Dynamics
### Query 5: Top Holders Distribution
### Query 6: Holder Growth Cohort Analysis
### Query 7: Market Cap & Price History (Derived)

1. **Transfer Amounts:** Pre-normalized to human-readable format (18 decimals already applied)
2. **Burn Address:** Canonical dead address `0x000000000000000000000000000000000000dead`
3. **Treasury Wallet:** Gnosis Safe at `0x778902475c0B5Cf97BB91515a007d983Ad6E70A6` (2-of-2 multisig)
4. **LP Pool:** Primary liquidity on Uniswap V3 FELIX/WETH (0xfd77b6e259025ad609dfa7221f081b898496574c)

---

## Technical Reference

### Token Contract Details

- **Contract:** ClankerToken (ERC-20 variant)
- **Address:** 0xf30bf00edd0c22db54c9274b90d2a4c21fc09b07
- **Deployment:** Block 41,631,867 (Feb 2, 2026)
- **Features:** Voting delegation, EIP-2612 permits, burn mechanisms, admin controls

### Treasury Contract Details

- **Contract:** Gnosis Safe (SafeL2)
- **Address:** 0x778902475c0B5Cf97BB91515a007d983Ad6E70A6
- **Deployment:** Block 41,839,324
- **Configuration:** 2-of-2 multisig (both owners required)
- **Signers:** 0x35977cf9b9a753d11e043452418d01f37d93084e, 0x114d78163fa1ab2488a5a2281317953c8679f508

---

### On-Chain Explorers

- [FELIX on BaseScan](https://basescan.org/token/0xf30bf00edd0c22db54c9274b90d2a4c21fc09b07)
- [Treasury on BaseScan](https://basescan.org/address/0x778902475c0B5Cf97BB91515a007d983Ad6E70A6)
- [FELIX on Clanker](https://www.clanker.world/clanker/0xf30Bf00edd0C22db54C9274B90D2A4C21FC09b07)

**Last Updated:** April 22, 2026  

