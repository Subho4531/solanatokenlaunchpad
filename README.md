# Solana Token Launchpad

Solana Token Launchpad is an open-source toolkit for creating, managing and launching token sales on Solana. It includes smart contracts (Anchor/Rust) for token sale logic (launchpad, whitelist, vesting), a web frontend for launch creation and participation, and developer scripts to deploy and test on localnet/devnet/mainnet-beta.

This repository aims to provide a simple, auditable foundation to run IDOs/launches on Solana while allowing teams to extend and customize sale mechanics.

> NOTE: This README describes typical repository layout and workflows. Adjust paths and commands to your repo structure if they differ.

Table of contents
- Project overview
- Features
- Architecture
- Prerequisites
- Quick start (local development)
- Deployment (devnet/mainnet)
- Creating a token & launch
- Testing
- Security & best practices
- Contributing
- License & contact

Project overview
This project contains:
- Anchor smart contracts (programs) implementing token launchpad features:
  - Create & configure a token sale
  - Whitelisting participants
  - Vesting schedules for token distribution
  - Purchase & claim logic
- A React-based frontend for launch administrators and participants
- Scripts for deployment, token minting, and common admin tasks
- Unit and integration tests for contracts

Features
- Create configurable IDO parameters: start/end times, soft/hard caps, price, min/max per-wallet
- Whitelist addresses and tiered allocation
- Vesting schedules with Cliff & Linear release options
- Admin controls to finalize, cancel, and withdraw funds
- Support for SOL and SPL token purchases
- Frontend to create launches, buy tokens, and claim vested tokens
- Tests and localnet support for quick iteration

Architecture
- Programs (Anchor/Rust): /programs or /contracts
- Frontend (React/TypeScript): /app or /frontend
- Scripts (Node.js or TS) for automation: /scripts
- Tests: /tests (Anchor/mocha or Jest)
- Configs: Anchor.toml, package.json, .env.example

Prerequisites
- Node.js (>=16)
- npm or yarn
- Rust toolchain (stable) and Solana toolchain
  - solana-cli (recommended version matching the cluster)
  - anchor-cli (for building & deploying Anchor programs)
- Anchor (for Anchor-based programs)
- pnpm (optional) or npm/yarn for package management
- A supported wallet (Phantom/Solflare) for frontend

Recommended versions
- Rust stable (1.70+)
- solana-cli >= 1.15
- anchor-cli >= 0.28
- Node.js 18+

Quick start (local development)
1. Clone
   git clone https://github.com/Subho4531/solanatokenlaunchpad.git
   cd solanatokenlaunchpad

2. Install dependencies
   - Root scripts / frontend:
     cd frontend
     npm install
   - Programs:
     cd ../programs
     npm install
     # or cargo will fetch dependencies when you build

3. Start Solana localnet (optional)
   solana-test-validator --reset

4. Configure Anchor (example)
   - Edit Anchor.toml to point to localnet or devnet
   - Example:
     [provider]
     cluster = "localnet"
     wallet = "~/.config/solana/id.json"

5. Build & deploy programs locally
   anchor build
   anchor deploy

6. Start frontend
   cd frontend
   npm run dev
   # Open http://localhost:3000 (or port set by the app)

Environment variables
Create a .env file in the frontend and scripts directories (or at repo root) by copying .env.example and filling values:

- CLUSTER (localnet | devnet | mainnet-beta)
- RPC_URL (e.g., https://api.devnet.solana.com)
- ANCHOR_WALLET (path to keypair json file)
- PROGRAM_ID (the deployed program ID)
- FRONTEND_URL (optional)

Creating a token (SPL token)
You can create a token with the SPL Token CLI:

1. Create a mint
   solana config set --url https://api.devnet.solana.com
   spl-token create-token
   spl-token create-account <TOKEN_ADDRESS>
   spl-token mint <TOKEN_ADDRESS> 1000000 <RECIPIENT_ADDRESS>

2. Or use a provided script:
   node scripts/create_token.js --name "MyToken" --supply 1000000 --decimals 6

Configuring & creating a launch
The frontend includes a "Create Launch" flow. Typical parameters:
- Token mint address
- Sale token price (in SOL or stable SPL)
- Start / end timestamps
- Hard cap / soft cap
- Min & max per wallet
- Whitelist (file upload or address list)
- Vesting schedule (cliff, duration, percentage)

Admin tasks (examples)
- Finalize sale (transfer unsold tokens back)
- Withdraw raised funds
- Add/remove whitelist addresses
Use the admin UI or scripts:
node scripts/finalize_sale.js --sale <SALE_ACCOUNT> --wallet ./keypair.json

Deploying to devnet / mainnet
1. Update Anchor.toml provider cluster to devnet or mainnet-beta.
2. Ensure ANCHOR_WALLET points to a funded keypair.
3. Build and deploy:
   anchor build
   anchor deploy --provider.cluster devnet
4. Update FRONTEND program ID and RPC_URL in .env
5. Deploy frontend to Vercel/Netlify or serve static build.

Testing
- Contract tests (Anchor)
  cd programs
  anchor test
- Frontend tests
  cd frontend
  npm test

Security & best practices
- Audit smart contracts before using mainnet deployments.
- Use fixed-size on-chain accounts; avoid unbounded loops.
- Keep admin keys offline and use multisig for large withdrawals.
- Rate-limit and validate frontend inputs; sign transactions client-side.
- Consider third-party audits for production IDOs.

Known limitations
- This repository provides a starting point and reference implementation — not a production-grade audited launchpad out of the box.
- Ensure economic and edge-case testing for your sale configuration.

Contributing
We welcome contributions:
- Open issues for bugs & feature requests
- Fork, create feature branches, and submit PRs
- Follow code style and add tests for new features

Recommended workflow
- Fork the repo
- Create a feature branch
- Run tests: anchor test and frontend tests
- Open a PR against main with description and testing steps

License
Specify license here (e.g., MIT). Please update LICENSE file accordingly.

Support / Contact
- Issues: https://github.com/Subho4531/solanatokenlaunchpad/issues
- For urgent help or audits, consider hiring a professional auditor.

Appendix: Useful commands
- solana config set --url https://api.devnet.solana.com
- solana-keygen new -o ~/.config/solana/id.json
- anchor build
- anchor deploy --provider.cluster devnet
- anchor test
- npm install && npm run dev (frontend)

Thanks for checking out Solana Token Launchpad. If you'd like, I can:
- generate a .env.example for the repo,
- create starter scripts for token creation and sale setup,
- or draft a frontend demo flow (React pages + components).
