# Base44 Dev Environment

## Project Overview

Seal is a decentralized secrets management (DSM) service by Mysten Labs, built on Sui blockchain.
The repo contains a Rust workspace (`crates/`) with a key-server binary, a crypto library, and a
seal CLI, plus an example frontend (`examples/frontend/`) demonstrating allowlist and subscription
access control patterns.

## What Runs in the Preview

The **example frontend** (`examples/frontend/`) is a Vite + React + TypeScript app that runs on
port 3000. It demonstrates Seal's allowlist and subscription features on Sui testnet. It does not
require a local key-server — it uses external key servers via the `@mysten/seal` SDK and connects
to Sui testnet directly.

The **key-server** (`crates/key-server/`) is a Rust binary (port 2024) that requires external
credentials: `MASTER_KEY` (BLS12-381 scalar, base64), `KEY_SERVER_OBJECT_ID` (Sui ObjectID), and
`NETWORK` (testnet/mainnet/devnet). It also needs a Sui RPC endpoint and PostgreSQL. It is not
needed for the frontend to run.

## Setup

- Frontend uses pnpm (lockfile v9). Dependencies install on container startup via
  `pnpm install --frozen-lockfile`.
- Vite dev server binds `0.0.0.0:3000`. The `__VITE_ADDITIONAL_SERVER_ALLOWED_HOSTS` env var
  (set by the platform) allows the preview hostname.
- Vite proxies `/aggregator*/v1` and `/publisher*/v1` to external Walrus testnet endpoints.

## Verification

- `curl http://localhost:3000/` should return the Seal Example Apps HTML page.
- The page shows two cards: "Allowlist Example" and "Subscription Example".
- Full functionality (encrypt/decrypt) requires a Sui wallet connected to testnet with balance.
