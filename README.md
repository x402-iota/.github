# x402-iota-sdk

A comprehensive x402 payment gateway SDK for the IOTA blockchain with Express and Next.js support.

## Overview

The x402 protocol enables HTTP-based crypto payments using the HTTP 402 "Payment Required" status code. This SDK provides a complete solution for implementing x402 payments on IOTA blockchain.

## Architecture

This monorepo contains the following packages:

- **@x402-iota/core** - Shared types, utilities, and business logic
- **@x402-iota/server-express** - Express middleware for x402 payments
- **@x402-iota/server-nextjs** - Next.js middleware for x402 payments
- **@x402-iota/client** - Client SDK for making x402 payments
- **@x402-iota/facilitator** - Facilitator service for payment verification

## Prerequisites

- Node.js 18+
- Bun 1.0+
- TypeScript 5.0+

## Installation

```bash
# Install bun if you haven't already
curl -fsSL https://bun.sh/install | bash

# Install dependencies
bun install

# Build all packages
bun run build
```

## Quick Start

### Express Server

```typescript
import express from "express";
import { x402Middleware } from "@x402-iota/server-express";

const app = express();

app.use(
  x402Middleware({
    facilitatorUrl: "http://localhost:3001",
    payToAddress: "0x...",
    network: "iota-evm-testnet",
    asset: "0x...", // USDC contract address
    routes: {
      "/api/premium": { amount: "1000000", description: "Premium API" },
      "/api/reports/*": { amount: "5000000" },
    },
  })
);

app.get("/api/premium", (req, res) => {
  res.json({ message: "Premium content" });
});

app.listen(3000);
```

### Next.js Middleware

```typescript
// middleware.ts
import {
  createX402Middleware,
  createX402MatcherConfig,
} from "@x402-iota/server-nextjs";

export const middleware = createX402Middleware({
  facilitatorUrl: "http://localhost:3001",
  payToAddress: "0x...",
  network: "iota-evm-testnet",
  asset: "0x...",
  routes: {
    "/api/protected": { amount: "1000000" },
  },
});

export const config = createX402MatcherConfig(["/api/protected/:path*"]);
```

### Client

```typescript
import { X402Client } from "@x402-iota/client";
import { ethers } from "ethers";

const wallet = new ethers.Wallet(privateKey, provider);
const client = new X402Client({
  signer: wallet,
  network: "iota-evm-testnet",
});

// Automatically handles 402 responses and payments
const data = await client.get("http://server/api/premium");
console.log(data);
```

## Development

```bash
# Install dependencies
bun install

# Build all packages
bun run build

# Run tests
bun test

# Clean build artifacts
bun run clean
```

## Supported Networks

- IOTA Mainnet
- IOTA Testnet
- IOTA EVM Mainnet
- IOTA EVM Testnet

## Features

- ✅ HTTP 402 Payment Required protocol
- ✅ EIP-712 structured data signing
- ✅ EIP-3009 gasless token transfers
- ✅ Express and Next.js middleware
- ✅ TypeScript with strict type checking
- ✅ On-chain and facilitator-based verification
- ✅ Replay attack prevention
- ✅ Route pattern matching (exact, prefix, parameters)

## License

MIT
