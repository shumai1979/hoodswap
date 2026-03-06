# 🟢 HoodSwap

> A decentralised token swap app built on **Robinhood Chain Testnet**, integrating directly with **Synthra V3** pools for real on-chain swaps.

🔗 **Live App:** [shumai1979.github.io/hoodswap](https://shumai1979.github.io/hoodswap)  
🔗 **Explorer:** [explorer.testnet.chain.robinhood.com](https://explorer.testnet.chain.robinhood.com)

---

## What is HoodSwap?

HoodSwap is a fully functional DEX frontend built natively for the Robinhood Chain Testnet. It connects directly to **Synthra V3** (the native Uniswap V3 fork on Robinhood Chain) and lets you swap real testnet tokens with real on-chain transactions — no mocks, no simulations.

---

## Features

- **Real swaps via Synthra V3** — uses `multicall` + `exactInputSingle` encoding confirmed from live on-chain transactions
- **60+ liquidity pools** — TSLA, AMZN, NFLX, AMD, PLTR, USDC, WETH and more
- **ERC-20 approval flow** — automatically detects when approval is needed before swapping
- **ETH ↔ token swaps** — handles WETH wrapping/unwrapping via `unwrapWETH9` multicall
- **Live pool prices** — reads `sqrtPriceX96` directly from Synthra V3 pool contracts
- **Fee tier selector** — 0.05%, 0.3%, 1%
- **Slippage control** — 0.1% to 2% or custom
- **Wrap / Unwrap ETH** — native WETH contract integration
- **Swap history** — reads Swap events from on-chain logs
- **Pools explorer** — browse all active pools with live prices
- **Wallet menu** — connect, copy address, view on explorer, disconnect
- **MetaMask integration** — auto-adds Robinhood Testnet network

---

## Supported Tokens

| Symbol | Name | Contract |
|--------|------|----------|
| ETH | Ethereum (native) | — |
| WETH | Wrapped ETH | `0x33e4191705c386532ba27cBF171Db86919200B94` |
| USDC | USD Coin | `0xbf4479C07Dc6fdc6dAa764A0ccA06969e894275F` |
| TSLA | Tesla | `0xC9f9c86933092BbbfFF3CCb4b105A4A94bf3Bd4E` |
| AMZN | Amazon | `0x5884aD2f920c162CFBbACc88C9C51AA75eC09E02` |
| NFLX | Netflix | `0x3b8262A63d25f0477c4DDE23F83cfe22Cb768C93` |
| PLTR | Palantir | `0x1FBE1a0e43594b3455993B5dE5Fd0A7A266298d0` |
| AMD | AMD | `0x71178BAc73cBeb415514eB542a8995b82669778d` |

---

## Synthra V3 Contracts

| Contract | Address |
|----------|---------|
| Factory | `0x911b4000d3422f482f4062a913885f7b035382df` |
| Router | `0x3Ce954107b1A675826B33bF23060Dd655e3758fE` |
| NFT Position Manager | `0x54A0EF7da351cb8fD1998E7945cc51E5825fB233` |

---

## How to Use

### Prerequisites
- [MetaMask](https://metamask.io) installed in your browser
- Testnet ETH from the [Robinhood Faucet](https://faucet.testnet.chain.robinhood.com)

### Steps
1. Open the app and click **Connect Wallet**
2. MetaMask will auto-add the Robinhood Chain Testnet network
3. Select your tokens and fee tier
4. If swapping an ERC-20 token, click **Approve** first (one-time per token)
5. Click **Swap** and confirm in MetaMask
6. Done — your balances update in real time

### Add tokens to your wallet
Copy the contract addresses above and add them manually in MetaMask or Rabby under the Robinhood Testnet network.

---

## Run Locally

```bash
git clone https://github.com/shumai1979/hoodswap
cd hoodswap
npx serve . -p 8080
# Open http://localhost:8080
```

No dependencies. Single HTML file. No build step required.

---

## Network Details

| | |
|---|---|
| Network | Robinhood Chain Testnet |
| Chain ID | 46630 |
| RPC | `https://rpc.testnet.chain.robinhood.com` |
| Explorer | `https://explorer.testnet.chain.robinhood.com` |
| Faucet | `https://faucet.testnet.chain.robinhood.com` |

---

## Technical Notes

The swap encoding was reverse-engineered from real Synthra V3 on-chain transactions. The router uses:

```
multicall(uint256 deadline, bytes[] data)  →  0x5ae401dc
  └─ exactInputSingle(tuple)               →  0x04e45aaf
       (tokenIn, tokenOut, fee, recipient, amountIn, amountOutMin, sqrtPriceLimitX96)
```

Note: unlike canonical Uniswap V3, Synthra's `exactInputSingle` struct does **not** include a `deadline` field — the deadline is only in the outer `multicall`.

When swapping to native ETH, the recipient is set to the router and a second `unwrapWETH9` call is added to the multicall array.

---

## Built With

- Vanilla HTML/CSS/JS — zero frameworks
- MetaMask `window.ethereum` provider
- Direct EVM calls via `eth_call` and `eth_sendTransaction`
- Synthra V3 on Robinhood Chain Testnet

---

Built with love for the Robinhood Chain ecosystem 🟢
