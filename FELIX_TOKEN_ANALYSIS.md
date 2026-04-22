# FELIX Token Contract Analysis

**Contract Address:** `0xf30bf00edd0c22db54c9274b90d2a4c21fc09b07` (Base Blockchain)

**Gnosis Safe multisig treasury** 0x778902475c0B5Cf97BB91515a007d983Ad6E70A6 (Base)

## Overview

FELIX is a cryptocurrency token on the Base blockchain associated with an AI agent called **Felix Craft**—an autonomous AI-driven business entity built on the OpenClaw framework, serving as the CEO of The Masinov Company. It represents a new paradigm in AI-powered tokenomics.

## Contract Mechanics

### Fee Structure
- **Buy Tax:** 1%
- **Sell Tax:** 1%
- **Clanker Platform Fee:** 0.2%
- **Total Transaction Cost:** 1.2%

### Fee Distribution
Collected fees are allocated to:
1. **Liquidity provision** - Maintains trading pairs
2. **Treasury** - Funded in ETH for project development
3. **Automatic token burns** - Deflationary mechanism to reduce supply

### Anti-Bot Protection
- **80.01% starting sniper fee** that decays to **5% over 15 seconds**
- Deployed to prevent bot trading at launch

## Tokenomics

| Metric | Value |
|--------|-------|
| Initial Supply | 100,000,000,000 FELIX |
| Circulating Supply | ~95.89 billion FELIX |
| Tokens Burned | **4.11%** (4,109,686,909 FELIX → dead address) |
| Burn Value (USD) | ~$25,225 |
| Top Wallet | 21.9% of circulating supply |
| Total Holders | 20,705+ |
| Deployment Date | February 2, 2026 |
| Deployed By | @bankrbot on X |

## Deflationary Model

The token implements a **deflationary mechanism** where:
- Every transaction triggers automatic token burns
- As trading volume increases, more tokens are removed from circulation
- This increases scarcity over time, potentially supporting long-term value

## Trading & Liquidity

### Primary Exchanges
- **LBank** (primary trading platform)
- **Uniswap V3** (Base network)
- **Uniswap V4** (Base network)
- **Clanker** (native platform with staking/trading features)

### Trading Pair
- FELIX/WETH on Uniswap V3 (1% fee)

## Key Characteristics

✅ **Deflationary** - Automatic burns reduce supply  
✅ **Anti-bot Protection** - Sniper fee decay mechanism  
✅ **Multi-exchange Liquidity** - Available on multiple DEXs  
✅ **Community-driven** - Created via Clanker for AI agent tokenization  
✅ **Active Holders** - 5,384+ community members  

---

**Sources:**
- [FELIX on Clanker](https://www.clanker.world/clanker/0xf30Bf00edd0C22db54C9274B90D2A4C21FC09b07)
- [FELIX on CoinGecko](https://www.coingecko.com/en/coins/felix-3)
- [FELIX on Coinbase](https://www.coinbase.com/price/felix-base-0xf30bf00edd0c22db54c9274b90d2a4c21fc09b07)

---

## Smart Contract Deep Dive (via Herde MCP)

### Contract Architecture

**Contract Name:** `ClankerToken` (ERC-20 variant)  
**Deployed:** Block 41,631,867 on Base  
**Deployment TX:** [0x3003c359209608e6a0ac7305f40e1796f9a21d66986892098b345b78d223aa18](https://basescan.org/tx/0x3003c359209608e6a0ac7305f40e1796f9a21d66986892098b345b78d223aa18)

**Current Token Metrics (On-Chain):**
- **Price:** $0.00000609 USD
- **Market Cap:** $584,300
- **Initial Supply:** 100,000,000,000 FELIX
- **Circulating Supply:** ~95.89 billion tokens
- **Burned:** 4,109,686,909 FELIX (4.11%) → `0x000000000000000000000000000000000000dead`

---

### Core Functional Areas

#### 1. **Token Management**
Core ERC-20 functionality with burn mechanisms:

- **`burn(amount)`** - Token holders can destroy their tokens, reducing supply
- **`burnFrom(account, amount)`** - Approved spender can burn tokens from another account
- **`crosschainBurn(from, amount)`** - SuperchainTokenBridge can burn tokens for cross-chain operations
- **`crosschainMint(to, amount)`** - SuperchainTokenBridge can mint tokens to addresses
- **`transfer(to, amount)`** / **`transferFrom(from, to, amount)`** - Standard ERC-20 transfers

#### 2. **Admin & Governance**
Owner/admin control with upgrade capability:

- **`updateAdmin(newAdmin)`** - Change the contract administrator
- **`verify()`** - Original admin can verify the contract (unlocks full functionality)
- **`updateMetadata(newMetadata)`** - Admin can update token metadata
- **`updateImage(newImageUrl)`** - Admin can change the token's image/branding
- **`allData()`** - View current admin, verification status, and metadata

#### 3. **Delegation & Voting Power**
Full governance/voting support:

- **`delegate(delegatee)`** - Users can delegate voting power to other addresses
- **`delegateBySig(delegatee, nonce, expiry, v, r, s)`** - Delegate via cryptographic signature
- **`getVotes(account)`** - Check current voting power
- **`getPastVotes(account, blockNumber)`** - Check historical voting power
- **`getPastTotalSupply(blockNumber)`** - Historical total supply snapshot

#### 4. **ERC-2612 Permit (Gasless Approvals)**
Modern gas-efficient approvals:

- **`permit(owner, spender, value, deadline, v, r, s)`** - Approve spending via signed message (no separate transaction needed)
- Eliminates need for two-step approval process
- Follows EIP-2612 standard

---

### Key Events & State Tracking

| Event | Emitted When | Purpose |
|-------|--------------|---------|
| **Transfer** | Tokens moved | Standard ERC-20 transfers |
| **Approval** | Spending approved | Spender authorized to use tokens |
| **CrosschainMint** | Bridge mints | Tokens created via SuperchainBridge |
| **CrosschainBurn** | Bridge burns | Tokens destroyed via SuperchainBridge |
| **DelegateChanged** | Delegate updated | User changes voting power recipient |
| **DelegateVotesChanged** | Vote power changes | Voting weight adjusted after delegation |
| **DelegateVotesChanged** | Vote power changes | Voting weight adjusted after delegation |
| **UpdateAdmin** | Admin changes | New admin assigned |
| **UpdateImage** | Image updated | Token branding changed |
| **UpdateMetadata** | Metadata updated | Token info modified |
| **Verified** | Contract verified | Original admin verified contract |
| **EIP712DomainChanged** | Domain changes | Domain separator updated |

---

### Operational States & Restrictions

**1. Unverified State (Initial)**
- Contract starts in unverified state
- Limited functionality available
- Awaiting verification from original admin

**2. Verified State (Active)**
- Original admin calls `verify()` to unlock full features
- All functions become available
- Cross-chain operations enabled

**3. Admin-Only Functions**
- `updateAdmin()`, `updateMetadata()`, `updateImage()`, `verify()`
- Revert with `NotAdmin` error if caller is not admin
- Only admin address can execute

---

### View Functions (Read-Only, No Gas)

**Token Information:**
- `name()` - Returns "FELIX"
- `symbol()` - Returns "felix"
- `decimals()` - Returns 18
- `totalSupply()` - Current circulating supply
- `balanceOf(account)` - Token balance for an account

**Admin & Verification:**
- `admin()` - Current admin address
- `originalAdmin()` - Original admin (can verify)
- `isVerified()` - Whether contract is verified
- `imageUrl()` - Token image URL
- `metadata()` - Token metadata

**Allowances & Voting:**
- `allowance(owner, spender)` - Spending approval amount
- `delegates(account)` - Current voting delegate
- `nonces(account)` - For permit signature tracking
- `supportsInterface(interfaceId)` - ERC-165 interface support

---

### Special Features & Architecture

✅ **Cross-Chain Enabled** - SuperchainTokenBridge integration for multi-chain minting/burning  
✅ **Full Governance** - Vote delegation with block-based historical tracking  
✅ **Gasless Approvals** - EIP-2612 permit mechanism for better UX  
✅ **Metadata Management** - Updateable token name, image, and metadata  
✅ **Access Control** - Admin-only functions with verification gate  
✅ **ERC-165 Support** - Interface detection for composability  
✅ **Voting Power Checkpoints** - Historical voting power queries  

---

### Security & Design Patterns

- **Admin Verification Pattern** - Requires explicit admin verification to activate
- **Delegation Checkpoints** - Voting power tracked at block level for historical queries
- **Signature-Based Actions** - Supports EIP-712 signed messages (permit, delegateBySig)
- **Burn Mechanism** - Deflationary through `burn()` and `burnFrom()` functions
- **Bridge-Compatible** - Ready for Superchain cross-bridge operations