
# Pump.fun Rust SDK for Solana

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Rust Edition](https://img.shields.io/badge/Rust-Edition%202021-orange)](https://www.rust-lang.org/)
[![Solana](https://img.shields.io/badge/Solana-Blockchain-9945FF)](https://solana.com)

Type-safe Rust SDK to interact with the Pump.fun program on Solana. Build Solana apps that create tokens, and buy/sell on bonding curves using Anchor-powered instruction builders. Designed for reliability, clear ergonomics, and smooth integration into Rust backends or bots.

Looking for a quick walkthrough? See Getting Started: `GETTING_STARTED.md`.

## Table of Contents

- Overview
- Features
- Installation
- Quick Start
- Usage: Create, Buy, Sell
- Examples
- Program Information
- SDK Structure
- Compatibility
- Error Handling
- Contributing
- License
- Disclaimer
- Keywords

## Overview

The `pumpdotfun_sdk` crate streamlines common Pump.fun flows on Solana: token creation with metadata, and trading via bonding curves with slippage protection. It includes PDA utilities, constants, states, and instruction builders, making it simple to assemble transactions in Rust.

## Features

- Create new tokens with metadata and bonding curves
- Buy tokens from bonding curves with slippage protection
- Sell tokens to bonding curves with slippage protection
- Built-in utilities for account management and PDA derivation
- Type-safe Rust implementation using the Anchor framework

## Installation

Add the SDK to your Rust project. If using a local path:

```toml
[dependencies]
pumpdotfun_sdk = { path = "../pumpfun-rust-sdk" }
solana-client = "2.3.4"
solana-sdk = "2.3.1"
```

Or use it directly within this repository by following Getting Started: `GETTING_STARTED.md`.

## Quick Start

Initialize the client and SDK:

```rust
use std::sync::Arc;
use pumpdotfun_sdk::PumpDotFunSdk;
use solana_client::rpc_client::RpcClient;

let rpc_client = Arc::new(RpcClient::new("https://api.devnet.solana.com".to_string()));
let sdk = PumpDotFunSdk::new(rpc_client);
```

## Usage

### Create a new token

```rust
use pumpdotfun_sdk::instructions::create::{CreateAccounts, CreateArgs};
use solana_sdk::{signature::Keypair, signer::Signer};

let mint_keypair = Keypair::new();
let user_keypair = Keypair::new();

let create_accounts = CreateAccounts {
    mint: mint_keypair.pubkey(),
    user: user_keypair.pubkey(),
};

let create_args = CreateArgs {
    name: "My Token".to_string(),
    symbol: "TOKEN".to_string(),
    uri: "https://example.com/metadata.json".to_string(),
    creator: user_keypair.pubkey(),
};

let instruction = sdk.create(create_accounts, create_args);
```

### Buy tokens

```rust
use pumpdotfun_sdk::instructions::buy::{BuyAccounts, Buy};
use solana_sdk::native_token::LAMPORTS_PER_SOL;

let buy_accounts = BuyAccounts {
    mint: mint_pubkey,
    user: user_keypair.pubkey(),
};

let buy_args = Buy {
    amount: 100_000_000,
    max_sol_cost: LAMPORTS_PER_SOL / 1000,
    slippage: 10,
};

let instructions = sdk.buy(buy_accounts, buy_args)?;
```

### Sell tokens

```rust
use pumpdotfun_sdk::instructions::sell::{SellAccounts, Sell};

let sell_accounts = SellAccounts {
    mint: mint_pubkey,
    user: user_keypair.pubkey(),
};

let sell_args = Sell {
    amount: 50_000_000,
    min_sol_output: LAMPORTS_PER_SOL / 1_000_000,
    slippage: 10,
};

let instructions = sdk.sell(sell_accounts, sell_args)?;
```

## Examples

This repository includes runnable examples:

- `examples/simple_example.rs` — minimal create and buy flow

For a scripted setup and more detail, see Getting Started: `GETTING_STARTED.md`.

## Program Information

- Program ID: `6EF8rrecthR5Dkzon8Nwu78hRvfCKubJ14M5uBEwF6P`
- Network: Solana (Mainnet program; develop on Devnet)
- Framework: Anchor

## SDK Structure

```
src/
├── lib.rs              # Main SDK entry point
├── instructions/       # Instruction builders
│   ├── create.rs       # Token creation
│   ├── buy.rs          # Token purchasing
│   └── sell.rs         # Token selling
├── constants.rs        # Program constants
├── errors.rs           # Error definitions
├── pda.rs              # PDA derivation utilities
└── states/             # Account state definitions
    └── global.rs       # Global state structure
```

## Compatibility

- Rust Edition 2021
- Solana crates: `solana-client 2.3.x`, `solana-sdk 2.3.x`
- Anchor-based instruction builders

## Error Handling

```rust
use pumpdotfun_sdk::errors::ErrorCode;

match sdk.buy(accounts, args) {
    Ok(instructions) => { /* ... */ }
    Err(ErrorCode::InvalidSlippage) => {
        eprintln!("Slippage must be non-negative");
    }
    Err(ErrorCode::GlobalNotFound) => {
        eprintln!("Global state not found - program may not be initialized");
    }
    Err(e) => {
        eprintln!("Other error: {:?}", e);
    }
}
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Open a pull request

## License

MIT — see `LICENSE` for details.

## Disclaimer

This SDK is for educational and development purposes. Test thoroughly on devnet before mainnet usage. The authors are not responsible for financial losses.

## Keywords

Pump.fun, Pump fun, Rust SDK, Solana SDK, Solana Pump.fun, bonding curve, token creation, buy tokens, sell tokens, Anchor, PDA, Solana program, Rust crypto, DeFi, memecoin tooling
