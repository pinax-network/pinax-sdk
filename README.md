# Pinax API `@pinax/api`

> Power your apps & AI agents with real-time blockchain data.

[![npm version](https://img.shields.io/npm/v/@pinax/api.svg)](https://www.npmjs.com/package/@pinax/api)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

## Overview

The `@pinax/api` provides a type-safe TypeScript client for the [Pinax API](https://docs.pinax.network). Access blockchain data including:

- **Token Transfers** - ERC-20, SPL, TRC-20 and native token transfers
- **DEX Swaps** - Uniswap, Jupiter, Raydium and other DEX swap events
- **Token Metadata** - Symbol, name, decimals, supply
- **Balances** - Real-time token holdings
- **Prices** - Current USD prices and OHLCV data
- **Liquidity Pools** - DEX pool information
- **NFTs** - Collections, holders, items, sales, transfers
- **Polymarket** - Markets, activity, positions, users
- **Hyperliquid** - Markets, users, vaults, liquidations, HIP-4 outcomes

### Supported Networks

The SDK provides typed chain constants for type-safe network selection:

- **EVM Chains**: Ethereum, ArbitrumOne, Unichain, Base, Optimism, Polygon, BNB Chain & Avalanche.
- **Solana**: Mainnet.
- **Tron**: Mainnet.

## Quick Start

### Installation

```bash
npm install @pinax/api
```

### Authentication

Get your API key from [pinax.network](https://pinax.network).

```typescript
const client = new PinaxAPI({
  apiToken: "YOUR_API_KEY_HERE" // or set PINAX_API_KEY in your environment
});
```

### Basic Usage

```typescript
import { PinaxAPI, EVMChains } from "@pinax/api";

const client = new PinaxAPI({
  apiToken: "YOUR_API_KEY_HERE"
});

// Get EVM token transfers using chain constants
const transfers = await client.evm.tokens.getTransfers({
  network: EVMChains.Ethereum,
  limit: 10,
});

console.log(transfers.data);
```

## Examples

### Get EVM Transfers

Retrieve ERC-20 and native token transfers for a specific address:

```typescript
// Get transfers to Vitalik's address
const transfers = await client.evm.tokens.getTransfers({
  network: EVMChains.Ethereum,
  to_address: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045", // Vitalik's address
  limit: 10,
});

for (const transfer of transfers.data ?? []) {
  console.log(`
    Block: ${transfer.block_num}
    From: ${transfer.from}
    To: ${transfer.to}
    Token: ${transfer.symbol} (${transfer.contract})
    Amount: ${transfer.value}
  `);
}
```

### Get EVM Swaps

Retrieve DEX swap events from Uniswap and other protocols:

```typescript
// Get swaps from the USDC/WETH pool
const swaps = await client.evm.dexs.getSwaps({
  network: EVMChains.Ethereum,
  pool: "0x88e6a0c2ddd26feeb64f039a2c41296fcb3f5640", // Uniswap V3 USDC/WETH pool
  limit: 10,
});

for (const swap of swaps.data ?? []) {
  console.log(`
    Block: ${swap.block_num}
    Pool: ${swap.pool}
    Input: ${swap.input_value} ${swap.input_token?.symbol}
    Output: ${swap.output_value} ${swap.output_token?.symbol}
    Protocol: ${swap.protocol}
    Summary: ${swap.summary}
  `);
}
```

### Get Token Balances

```typescript
// Get token balances for a wallet
const balances = await client.evm.tokens.getBalances({
  network: EVMChains.Ethereum,
  address: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045", // Vitalik's address
});

for (const balance of balances.data ?? []) {
  console.log(`${balance.symbol}: ${balance.value}`);
}
```

## Development

### Building from Source

```bash
# Clone the repository
git clone https://github.com/pinax-network/pinax-api-sdk.git
cd pinax-api-sdk

# Install dependencies
bun install

# Generate types from OpenAPI spec
bun run generate

# Build the package
bun run build
```

## Related Resources

- [Pinax API Documentation](https://docs.pinax.network)
- [Pinax](https://pinax.network) - Get your API key

## License

Apache 2.0 - see [LICENSE](LICENSE) for details.
