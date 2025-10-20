# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About SelfCrypto

SelfCrypto is a multi-chain web3 wallet based on Enkrypt wallet, supporting EVM-compatible chains. It features custom name resolution services and token naming services. The project is built as a browser extension using Vue 3.

## Project Structure

This is a Yarn 3 monorepo workspace with the following package organization:

### Core Packages

- **`packages/extension`** - Main browser extension (Vue 3 + TypeScript)
  - `src/providers/` - Chain-specific providers (ethereum, bitcoin, kadena, polkadot)
  - `src/ui/` - Vue 3 UI components and pages (action, onboard, provider-pages)
  - `src/libs/` - Shared libraries (keyring, storage, network state management)
  - `src/manifest/` - Browser manifest configurations

- **`packages/signers/`** - Chain-specific signing implementations
  - `bitcoin/` - Bitcoin transaction signing
  - `ethereum/` - Ethereum transaction signing
  - `kadena/` - Kadena transaction signing
  - `polkadot/` - Polkadot transaction signing

- **`packages/keyring`** - Key management and account handling across all chains

- **`packages/storage`** - Persistent storage using localforage

- **`packages/extension-bridge`** - Message passing between extension contexts (background, content script, popup)

- **`packages/types`** - Shared TypeScript types

- **`packages/utils`** - Shared utility functions

- **`packages/request`** - RPC request handling with middleware support

- **`packages/name-resolution`** - ENS, SID, and Unstoppable Domains name resolution

- **`packages/hw-wallets`** - Hardware wallet integration

- **`packages/swap`** - Token swap functionality

### Provider Architecture

Each chain provider implements the `BackgroundProviderInterface` and follows this pattern:

1. Provider class (e.g., `EthereumProvider` in `packages/extension/src/providers/ethereum/index.ts`)
   - Manages RPC requests and middleware
   - Uses EventEmitter for notifications
   - Integrates with keyring for signing

2. Network definitions in `networks/` subdirectory
   - Each network is a class extending base network type
   - Contains RPC endpoints, chain IDs, explorers, etc.

3. Methods in `methods/` subdirectory
   - Each RPC method is a separate file
   - Methods handle wallet operations (signing, accounts, transactions)

4. UI routes in `ui/routes/` subdirectory
   - Provider-specific UI pages and navigation

## Development Commands

### Installation & Build
```bash
yarn install                  # Install all dependencies
yarn build:all               # Build all packages (uses ultra-runner)
```

### Development Workflow
```bash
yarn watch                   # Watch all packages
yarn watch-extension         # Watch extension package only
cd packages/extension && yarn watch  # Alternative to watch extension
```

### Extension-Specific Commands
```bash
cd packages/extension
yarn build:chrome            # Build for Chrome
yarn build:firefox           # Build for Firefox
yarn build:operaedge         # Build for Opera/Edge
yarn watch                   # Watch mode for Chrome
yarn watch:firefox           # Watch mode for Firefox
```

### Testing & Linting
```bash
yarn test                    # Run tests across all packages
yarn lint                    # Run linting across all packages

# For specific package tests
cd packages/extension
yarn test                    # Run extension tests (ts-mocha)

cd packages/keyring
yarn test                    # Run keyring tests
```

### Code Quality
```bash
yarn prettier --write "**/*.{js,ts,vue}"    # Format code
yarn eslint --fix "src/**/*.{js,ts,vue}"    # Fix linting issues
```

## Watch Mode Details

The extension watch mode uses `concurrently` to run multiple build processes:
- `watch-contentscript` - Rollup watch for content script
- `watch-inject` - Rollup watch for inject script
- `watch-vue-chrome` or `watch-vue-firefox` - Vue CLI watch for popup/UI

Build artifacts are output to `packages/extension/dist/`

## Testing

- Tests use `ts-mocha` with chai assertions
- Test files use `.test.ts` or `.mocha.ts` extensions
- Extension tests: `packages/extension/src/providers/*/tests/`
- Test config: `packages/extension/configs/tsconfig.test.json`

## Commit Guidelines

Uses commitlint with conventional commits. Allowed types:
- `feat`, `fix`, `chore`, `docs`, `style`, `refactor`, `perf`, `test`, `ci`, `build`, `devop`, `revert`, `button`

## Key Implementation Details

### Extension Architecture

- **Background Script**: Manages providers, keyring, and message routing
- **Content Script**: Injected into web pages, built with Rollup
- **Inject Script**: Provides `window.ethereum` API to dApps
- **Popup UI**: Vue 3 SPA for wallet interface

### Message Flow

Uses `extension-bridge` for communication:
1. DApp → Inject Script → Content Script → Background → Provider
2. UI Popup → Background → Provider

### State Management

- Uses Pinia for Vue state management
- Persistent state in `packages/extension/src/libs/*-state/`
  - `networks-state` - Network configurations
  - `tokens-state` - Token balances
  - `activity-state` - Transaction history
  - `settings-state` - User settings

### Network Support

Currently focuses on EVM-compatible chains only. The codebase includes Bitcoin, Kadena, and Polkadot provider code but the UI has been configured to show only EVM chains.

### Name Resolution

Custom self-hosted EVM resolution service has been integrated. The name-resolution package supports ENS, SID (Space ID), and Unstoppable Domains.

## Browser Compatibility

- Chrome/Chromium (primary)
- Firefox (requires separate build)
- Opera/Edge (requires separate build)

Build process uses `BROWSER` environment variable to generate browser-specific manifests and polyfills.
