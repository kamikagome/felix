# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **blockchain analysis workspace** focused on researching and analyzing the FELIX token ecosystem on Base. The workspace is designed for on-chain data investigation, smart contract analysis, and treasury management tracking.

**Primary Focus:**
- FELIX token contract analysis (`0xf30bf00edd0c22db54c9274b90d2a4c21fc09b07`)
- Treasury wallet monitoring (`0x778902475c0B5Cf97BB91515a007d983Ad6E70A6`)
- Base blockchain data queries
- Smart contract behavior documentation

## Repository Structure

```
/Users/genkisudo/Documents/felix/
├── felix.md                          # Project note with token address
├── FELIX_TOKEN_ANALYSIS.md          # Complete token contract analysis
├── TREASURE_WALLET_ANALYSIS.md      # Multisig wallet documentation
├── skills-lock.json                 # Locked agent skills
└── .claude/
    └── settings.local.json          # Project-specific permissions
```

## Key Tools & MCP Servers

### Herd MCP (Blockchain Analysis)

Pre-configured and allowed in this workspace. Use for:
- **Contract Analysis:** `contractMetadataTool` - Get ABI, functions, events, deployment info
- **Wallet Overview:** `getWalletOverviewTool` - Check holdings, wallet type, signers
- **Transaction Activity:** `getTransactionActivityTool` - Trace fund flows
- **Token Activity:** `getTokenActivityTool` - Check transfer history and balances

**Common Usage:**
```bash
# Analyze token contract
mcp__herd-mcp__contractMetadataTool
  - contractAddress: 0xf30bf00edd0c22db54c9274b90d2a4c21fc09b07
  - blockchain: base

# Check treasury wallet
mcp__herd-mcp__getWalletOverviewTool
  - walletAddress: 0x778902475c0B5Cf97BB91515a007d983Ad6E70A6
  - blockchain: base

# Trace token transfers
mcp__herd-mcp__getTokenActivityTool
  - holderAddress: 0x778902475c0B5Cf97BB91515a007d983Ad6E70A6
  - tokenAddress: 0xf30bf00edd0c22db54c9274b90d2a4c21fc09b07
```

### Dune SQL Analytics

Available for complex on-chain queries:
- **Query Building:** Use DuneSQL to aggregate transaction data
- **Dashboard Creation:** Visualize findings across multiple chains
- **Data Export:** Extract historical data for analysis

## Common Workflows

### 1. Contract Analysis Flow
```
1. Fetch contract metadata using Herd MCP
2. Extract key functions, events, and state variables
3. Document findings in markdown
4. Create visual summary tables
5. Explain security implications and design patterns
```

### 2. Wallet Monitoring Flow
```
1. Get wallet overview (type, signers, holdings)
2. Query transaction activity from the wallet
3. Check token transfers (inflows/outflows)
4. Track fund destinations
5. Update documentation with timeline
```

### 3. Token Flow Tracking
```
1. Identify token holders and distribution
2. Query transfer history over time
3. Detect patterns (daily transfers, batch operations)
4. Identify source/destination addresses
5. Document accumulation patterns
```

## Documentation Standards

When creating analysis documents:

- **Use markdown headings** to organize sections (H2/H3)
- **Create data tables** for token metrics, holdings, events
- **Include code blocks** for function signatures and key logic
- **Add context headers** with contract address and blockchain
- **Link relevant resources** (BaseScan, Clanker, CoinGecko)
- **Summarize findings** with ✅ checkmarks for key features

### Document Template
```markdown
# [Title] Analysis

**Contract/Wallet Address:** `0x...`  
**Blockchain:** Base  
**Status:** [Active/Inactive]  

## Overview
[1-2 sentence description]

## Key Holdings/Metrics
[Table of important data]

## How It Works
[Functional explanation]

## Security & Design
[Notable patterns and considerations]
```

## Permissions Configuration

The workspace has pre-approved permissions for:
- Herd MCP contract analysis tools
- Wallet overview tools  
- Transaction/token activity tools
- WebFetch for clanker.world

To add new permissions, modify `.claude/settings.local.json` and add entries to the `permissions.allow` array.

## Analyzing New Addresses

When analyzing a new contract or wallet:

1. **Start with Herd MCP** - Get full contract metadata
2. **Extract key data** - Functions, events, state variables
3. **Query transaction history** - See actual usage patterns
4. **Cross-reference** - Check deployments, signers, related addresses
5. **Document findings** - Create analysis markdown file
6. **Identify connections** - Link to token contracts, treasuries, etc.

## Common Addresses Reference

| Address | Type | Purpose |
|---------|------|---------|
| `0xf30bf00edd0c22db54c9274b90d2a4c21fc09b07` | Token | FELIX token contract (ClankerToken) |
| `0x778902475c0B5Cf97BB91515a007d983Ad6E70A6` | Wallet | FELIX treasury (Gnosis Safe 2-of-2) |
| `0xF95FD8F27824B001fE8c5f63764d386bEFda9435` | Address | FELIX distribution source (daily drip) |
| `0xfd77b6e259025ad609dfa7221f081b898496574c` | Pool | FELIX/WETH Uniswap V3 LP (1% fee) |
| `0x000000000000000000000000000000000000dead` | Dead | Burn address — 4.11% of supply burned here |

## Notes for Future Sessions

- **FELIX tokenomics:** 1.2% transaction fee (1% token tax + 0.2% Clanker fee); initial supply 100B FELIX
- **Burns:** Burned tokens go to `0x000...dead` (NOT zero address) — 4.11% = 4.109B tokens burned as of Apr 2026
- **Treasury:** 2-of-2 Gnosis Safe holding ~3.63B FELIX + 52.9 WETH; receives daily drip from distribution source
- **Dune queries:** `tokens.transfers.amount` is already normalized (human-readable token units) — do NOT divide by 1e18
- **All Dune queries:** Always filter `block_time >= TIMESTAMP '2026-01-01'` — FELIX launched Feb 2 2026, no earlier data
- **Cross-chain ready:** SuperchainTokenBridge integration for multi-chain operations