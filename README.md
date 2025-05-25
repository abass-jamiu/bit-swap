# BitSwap Protocol

**Lightning-Fast Bitcoin-Native DEX on Stacks**
BitSwap is a decentralized exchange (DEX) purpose-built for the Bitcoin Layer 2 ecosystem via the [Stacks](https://stacks.co) blockchain. It enables secure, trustless token swaps and liquidity provisioning backed by Bitcoin finality.

## Overview

BitSwap is a high-performance **automated market maker (AMM)** that empowers users to:

* Swap Bitcoin-backed tokens with minimal fees and slippage.
* Provide liquidity and earn protocol fees.
* Maintain custody of funds while interacting with a transparent and verifiable smart contract.

BitSwap's design takes full advantage of the **Stacks chain’s smart contract capabilities**, while inheriting Bitcoin's immutability and security.

## Key Features

* ✅ **Bitcoin-Native**: Built on Stacks for Bitcoin anchoring and finality.
* ⚡ **AMM Engine**: Uses a constant product formula (x·y=k) for pricing.
* 🔐 **Trustless Execution**: Fully on-chain token swaps and liquidity operations.
* 📉 **Low Slippage**: Advanced slippage protection for secure trades.
* 💰 **Liquidity Incentives**: LP shares, fee accrual, and proportional distribution.
* ⚙️ **Configurable Fees**: Protocol fee tunable by the contract owner.
* 🧠 **Minimal Gas Usage**: Efficient logic and safe math with Clarity smart contracts.
* 🧾 **Pool Admin Tools**: Pool creation, pausing, resumption, and inspection APIs.

## Architecture

### High-Level Design

```plaintext
        ┌─────────────────────┐
        │   Wallet / UI App   │
        └────────┬────────────┘
                 │
                 ▼
        ┌─────────────────────┐
        │  BitSwap Smart Contract (Clarity) │
        └────────┬────────────┘
   ┌─────────────┴─────────────┐
   ▼                           ▼
Fungible Token A        Fungible Token B
 (SIP-010)                (SIP-010)
```

### Smart Contract Modules

* `ft-trait`: Interface definition for SIP-010 compliant tokens.
* `create-pool`: Initializes a trading pool with two distinct tokens.
* `add-liquidity`: Supplies token pairs to a pool and mints LP shares.
* `remove-liquidity`: Withdraws token reserves in proportion to LP shares.
* `swap-exact-tokens`: Swaps one token for another using constant-product logic.
* `get-pool-info`: Retrieves reserves and configuration of any pool.
* `get-exchange-rate`: Calculates the current token exchange rate using pool reserves.
* `pause/resume-pool`: Temporarily suspend/resume trading activity.
* `set-protocol-fee`: Update the protocol fee (max 100%).

## Example Usage

###reate a New Pool

```clarity
(create-pool <token-x-contract> <token-y-contract>)
```

###dd Liquidity

```clarity
(add-liquidity u0 <token-x> <token-y> u1000 u1000 u500)
```

###xecute Swap

```clarity
(swap-exact-tokens u0 <token-x> <token-y> u100 u95 true)
```

###emove Liquidity

```clarity
(remove-liquidity u0 <token-x> <token-y> u200 u90 u90)
```

## Error Codes

| Code   | Description                 |
| ------ | --------------------------- |
| `u100` | Not authorized              |
| `u101` | Invalid amount              |
| `u102` | Insufficient balance        |
| `u103` | Pool not found              |
| `u104` | Invalid pool configuration  |
| `u105` | Slippage tolerance exceeded |
| `u106` | Zero liquidity              |

## Extending BitSwap

You can extend the core AMM protocol to support:

* Flash swaps
* Time-weighted average prices (TWAP)
* Multi-hop routing (via a DEX router contract)
* DAO-controlled fee adjustment mechanisms

## Security Considerations

* Uses `as-contract` calls to ensure protocol custody.
* Checks all input validations for tokens, slippage, and balances.
* Fails securely using `asserts!` and `unwrap!` for all key operations.

## Deployment Notes

* Requires tokens to implement the SIP-010 standard.
* The deploying wallet becomes the protocol administrator (`CONTRACT-OWNER`).
* All sensitive admin operations are restricted to this address.

## Contributions

Contributions, audits, and peer reviews are welcome. Please submit issues or pull requests through the project's GitHub repository.
