# Treasure Wallet Analysis - 0x778902475c0B5Cf97BB91515a007d983Ad6E70A6

**Wallet Type:** Gnosis Safe (SafeL2) - 2-of-2 Multisig  
**Blockchain:** Base  
**Deployed:** Block 41,839,324  
**Status:** Active (Last transfer: Apr 21, 2026)

---

## Wallet Overview

This is a **multisignature treasury wallet** holding substantial FELIX token reserves. It requires **2 out of 2 signatures** to execute transactions, meaning both owners must approve any action.

### Holdings Summary

| Asset | Amount | USD Value | Percentage |
|-------|--------|-----------|-----------|
| **FELIX** | 3,632,826,822.55 | ~$21,793.75 | 3.79% of circulating supply |
| **WETH** | 52.90 | ~$127,433.70 | 4.42% |
| **ETH** | 0.20 | ~$481.79 | 0.02% |
| **USDC** | 435.00 | ~$434.94 | 0.02% |
| **100+ Other Tokens** | Various | ~$0.02 | 0.29% |
| **TOTAL HOLDINGS** | - | **~$149,144** | 100% |

### Owners (Signers)

**2-of-2 Multisig Configuration:**

1. **Signer 1:** `0x35977cf9b9a753d11e043452418d01f37d93084e` (EOA)
2. **Signer 2:** `0x114d78163fa1ab2488a5a2281317953c8679f508` (EOA)

**Requirement:** Both signers must approve transactions  
**Pending Transactions:** 0 (No queued actions)

---

## Smart Contract Architecture (SafeL2)

### What is SafeL2?

SafeL2 is an **optimized version of Gnosis Safe** designed for Layer 2 chains (like Base). It provides:
- **Multi-signature wallet functionality** - Requires multiple signatures for transactions
- **Governance control** - Owner management, module enablement
- **Safe execution** - Delegated calls, fallback handlers
- **Module support** - Can enable/disable external modules for extended functionality

### Core Smart Contract Functions

#### 1. **Transaction Execution**
```
execTransaction()
- Executes signed transactions after verification
- Emits SafeMultiSigTransaction event
- Checks signatures against nonce and hash
- Returns success status
```

#### 2. **Owner Management**
```
addOwnerWithThreshold(newOwner, newThreshold)
- Add new owner and update signature requirement
- Emits AddedOwner + ChangedThreshold events

removeOwner(ownerToRemove, newThreshold)
- Remove owner from signer list
- Emits RemovedOwner + ChangedThreshold events

swapOwner(oldOwner, newOwner)
- Replace one owner with another
- Emits RemovedOwner + AddedOwner events
```

#### 3. **Module Management**
```
enableModule(moduleAddress)
- Whitelist a module for automated transactions
- Emits EnabledModule event

disableModule(moduleAddress)
- Disable a previously enabled module
- Emits DisabledModule event
```

#### 4. **Administrative Functions**
```
setFallbackHandler(fallbackHandler)
- Set default handler for unexpected calls
- Enables advanced delegation patterns

receive()
- Accepts native ETH/currency transfers
- Emits SafeReceived event
```

### View Functions (Read-Only)

- `getOwners()` → Array of owner addresses (2 signers)
- `getThreshold()` → Signature requirement (2 out of 2)
- `isOwner(address)` → Check if address is authorized signer
- `getModulesPaginated()` → List enabled modules (if any)
- `nonce()` → Current transaction counter
- `domainSeparator()` → EIP-712 signing domain hash

---

## Asset Flow & History

### FELIX Token Accumulation

**Source Address:** Primarily `0xF95FD8F27824B001fE8c5f63764d386bEFda9435`
- This appears to be the **FELIX project treasury or rewards distributor**
- Sends FELIX tokens in regular batches to this wallet

**Pattern:** 
- Transfers occur approximately every 1-2 days
- Variable amounts (ranging from ~700K to ~2.18B FELIX)
- Consistent sender indicates automated distribution system

**Recent Transfer Examples:**
- Apr 21, 2026: +6,277,143 FELIX ($0.038)
- Apr 20, 2026: +5,773,255 FELIX ($0.035)
- Apr 19, 2026: +2,946,095 FELIX ($0.018)
- Continuing back to Feb 8, 2026 (initial deposit: 2.18B FELIX)

**Total FELIX Accumulated:** ~3.63 Billion tokens  
**Time Period:** ~2.5 months of regular accumulation

### Other Assets

**WETH Holdings:**
- 52.90 WETH (~$127K)
- Value: Largest non-FELIX holding
- Function: Likely used for gas + liquidity

**ETH Native:**
- 0.20 ETH (~$481)
- Used for transaction gas fees

**USDC:**
- 435 USDC (~$435)
- Stablecoin for potential withdrawals

**Meme/Community Tokens:**
- 100+ different tokens with minimal value
- Likely airdrops or test tokens
- Total value: <$1

---

## Security & Design

### Multisig Benefits

✅ **Shared Control** - No single owner can move funds  
✅ **Fraud Prevention** - Requires collusion to compromise  
✅ **Approval Process** - Both signers must consent to transactions  
✅ **Transparency** - All transactions are verifiable on-chain  

### Operational States

**1. Uninitialized** → Not yet set up
**2. Active** → Ready to execute transactions (current state)
**3. With Modules** → Can enable automated actors
**4. With Fallback Handler** → Can delegate special calls

### Key Datapoints

- **Created:** Block 41,839,324
- **Deployment TX:** `0x6fca104b0c945e0ec41740f96ac531bb16b44b492af27a6b7ec6fba5df1809a3`
- **Total Transactions:** 4 on-chain
- **Modules Enabled:** None currently
- **Fallback Handler:** Not set
- **Transaction Nonce:** Current counter for execution tracking

---

## Functional Flows

### Flow 1: Receiving FELIX Tokens
```
1. External sender (0xF95FD8F27824B001fE8c5f63764d386bEFda9435)
   ↓
2. Calls transfer() on FELIX contract
   ↓
3. Token balance increases in SafeL2 wallet
   ↓
4. Transfer event emitted on blockchain
```

### Flow 2: Executing a Withdrawal (2-of-2 Required)
```
1. Signer 1 creates transaction proposal
   ↓
2. Hash created with: to, value, data, nonce, etc.
   ↓
3. Signer 1 approveHash(hash) OR signs offline
   ↓
4. Signer 2 calls execTransaction() with both signatures
   ↓
5. SafeL2 verifies both signatures valid
   ↓
6. Transaction executes (send tokens, call contract, etc.)
   ↓
7. SafeMultiSigTransaction event emitted with full details
```

### Flow 3: Enabling a Module
```
1. Both signers approve enableModule() call
   ↓
2. Module address whitelisted in safe storage
   ↓
3. Module can now execute transactions via 
   execTransactionFromModule()
   ↓
4. EnabledModule event emitted
```

---

## Contract Events

The SafeL2 contract emits **17 key events** for transparency:

| Event | Triggered When |
|-------|---|
| **SafeMultiSigTransaction** | Multisig-verified transaction executed |
| **SafeModuleTransaction** | Module executes transaction |
| **SafeReceived** | Native currency (ETH) received |
| **AddedOwner** | New signer added |
| **RemovedOwner** | Signer removed |
| **ChangedThreshold** | Signature requirement changed |
| **EnabledModule** | Module whitelisted |
| **DisabledModule** | Module disabled |
| **ChangedFallbackHandler** | Fallback handler updated |
| **UpdateAdmin** | Admin changed |
| **Approval** | Signature approval given |

---

## What This Wallet Is Used For

Based on the analysis:

1. **FELIX Treasury/Staking Pool** - Accumulates FELIX tokens
2. **Community Rewards Distribution** - Receives tokens to distribute
3. **Liquidity Provider** - Holds WETH for DeFi interactions
4. **Multi-Party Fund Management** - 2-of-2 setup for controlled access
5. **Secure Asset Custody** - Uses industry-standard Gnosis Safe

---

## Contract Version & Standards

- **Contract Name:** SafeL2
- **Version:** (Check with `VERSION()` function)
- **Standards:** EIP-712 (signing), EIP-165 (interface detection)
- **Factory:** Created via Gnosis Safe proxy factory
- **Upgradeable:** Yes (proxy pattern)

---

## Key Takeaways

🔒 **Highly Secure** - 2-of-2 multisig requires both owners to approve  
💼 **Institutional Grade** - Uses battle-tested Gnosis Safe code  
📊 **Active Treasury** - Regular FELIX token inflows (~3.6B accumulated)  
⚡ **Multi-Asset** - Supports multiple tokens (FELIX, WETH, USDC, etc.)  
🛡️ **Modular Design** - Can enable modules for advanced use cases  
📝 **Transparent** - All transactions logged on-chain with events  

---

**Deployment TX:** [View on BaseScan](https://basescan.org/tx/0x6fca104b0c945e0ec41740f96ac531bb16b44b492af27a6b7ec6fba5df1809a3)