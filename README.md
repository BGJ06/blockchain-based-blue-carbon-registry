# 🌐 Decentralized Blue Carbon Registry System (SIH-25038)

A secure, transparent, and decentralized blockchain-based registry system for managing blue carbon credits (coastal and marine ecosystem offsets). Built as a prototype for Smart India Hackathon (SIH), this platform leverages EVM smart contracts to mint, track, and retire carbon credits on-chain while providing a modern Next.js dashboard for traditional and Web3 operations.

[![Next.js](https://img.shields.io/badge/Next.js-15.5.2-000000?logo=next.js&logoColor=white)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19.1.0-61DAFB?logo=react&logoColor=white)](https://react.dev/)
[![Solidity](https://img.shields.io/badge/Solidity-^0.8.20-363636?logo=solidity&logoColor=white)](https://soliditylang.org/)
[![Web3 Wallet](https://img.shields.io/badge/RainbowKit-Wagmi-3898EC?logo=ethereum&logoColor=white)](https://rainbowkit.com/)
[![TailwindCSS](https://img.shields.io/badge/TailwindCSS-v4.0-38B2AC?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16.x-4169E1?logo=postgresql&logoColor=white)](https://www.postgresql.org/)

---

## 📋 Project Overview

Coastal ecosystems (mangroves, salt marshes, and seagrasses) absorb large amounts of carbon dioxide. This platform ensures that credit generation, verification, and retirement are securely tracked. Traditional registries face issues with double-counting and lack of auditing. This project solves these issues by:
1.  **Minting On-Chain Tokens**: Representing verified blue carbon credits.
2.  **Irreversible Retirement**: Locking credits into a dedicated burn contract (`Retirement.sol`) to prevent double-spending.
3.  **Role-Based Audits**: Requiring Multi-Sig or independent verifier signatures before credit issuance.
4.  **Dual Authentication**: Utilizing traditional database accounts (Next-Auth + PostgreSQL) alongside secure crypto wallets (RainbowKit + MetaMask).

---

## 🏗️ System Architecture & Web3 Lifecycle

The diagram below details the dual-security integration linking Next-Auth with EVM Web3 smart contract calls:

```mermaid
sequenceDiagram
    autonumber
    actor User as Developer / Auditor
    participant UI as Next.js 15 Client
    participant Auth as Next-Auth & Postgres
    participant Wallet as Web3 (MetaMask/Wagmi)
    participant Ledger as Solidity Smart Contracts

    User->>UI: Enter Credentials
    UI->>Auth: Authenticates & Resolves DB Role
    Auth-->>UI: Authorizes Session Cookie
    User->>UI: Click "Connect Wallet"
    UI->>Wallet: Requests Account Connection
    Wallet-->>UI: Returns Ethereum Address
    User->>UI: Registers Project/Retires Credits
    UI->>Wallet: Prompts Transaction Signature
    Wallet->>Ledger: Calls CarbonRegistry.sol / Retirement.sol
    Ledger-->>Ledger: Executes State Update (On-Chain Event Logged)
    Ledger-->>UI: Emits Transaction Receipt
```

---

## 📁 Repository Directory Structure

```text
blockchain-based-blue-carbon-registry/
├── contracts/                  # Solidity Smart Contracts (EVM Compatible)
│   ├── CarbonRegistry.sol     # Core credit token minting, tracking, and owner mappings
│   ├── Reitrement.sol         # Smart contract managing irreversible credit burning
│   └── RolesController.sol    # Decentralized Access Control List for system roles
├── blue-carbon-registry/       # Next.js 15 Web Application (App Router)
│   ├── app/                   # Web pages (auth, dashboard, projects, ledger)
│   ├── components/            # Shadcn/Radix UI widgets & Web3 connection modals
│   ├── lib/                   # Database helpers & Web3 configurations (wagmi/viem)
│   ├── auth.js                # Next-Auth configuration & JWT handlers
│   ├── middleware.js          # Route protection and role restrictions
│   ├── check-admin.js         # Command line admin creation tool
│   ├── tsconfig.json          # TypeScript compilation settings
│   └── package.json           # Web app dependency manifest (Tailwind 4, Wagmi, viem)
├── bccr/                      # Vite + React Client Dashboard
│   ├── src/                   # Secondary client layout & charts
│   ├── vite.config.ts         # Vite build settings
│   └── package.json           # Vite project dependencies
└── README.md                  # Root documentation (this file)
```

---

## ⚙️ Setup & Installation Guide

### Prerequisites
*   [Node.js](https://nodejs.org/) (v18.0.0 or higher)
*   [PostgreSQL Database](https://www.postgresql.org/) (local or hosted instance)
*   [MetaMask Extension](https://metamask.io/) installed in your browser
*   EVM network connection details (Hardhat Node, Sepolia Testnet, or Arbitrum)

### 1. Clone & Set Up Folder
```bash
git clone https://github.com/BGJ06/blockchain-based-blue-carbon-registry.git
cd blockchain-based-blue-carbon-registry
```

### 2. Configure Smart Contracts
In the root directory or your Hardhat/Foundry workspace, compile the Solidity smart contracts located in `/contracts`:
```bash
# Compilation commands vary depending on compiler choice:
# Solidity Compiler Version: 0.8.20
```

### 3. Set Up Next.js Web App
Navigate to `/blue-carbon-registry` and configure the environment:
```bash
cd blue-carbon-registry
npm install
```

Create a `.env.local` file in `/blue-carbon-registry`:
```env
# Database Credentials
DATABASE_URL="postgresql://postgres:password@localhost:5432/blue_carbon_db"

# Next Auth Configurations
AUTH_SECRET="your_long_random_next_auth_secret_key_here"

# Web3 Provider Details
NEXT_PUBLIC_RPC_URL="https://sepolia.infura.io/v3/your_infura_key"
NEXT_PUBLIC_REGISTRY_CONTRACT_ADDRESS="0xYourCompiledContractAddressHere"
NEXT_PUBLIC_RETIREMENT_CONTRACT_ADDRESS="0xYourRetirementContractAddressHere"
```

### 4. Seed Database & Create Admin
Run the CLI utility script to initialize database tables and seed an admin user:
```bash
node check-admin.js
```

### 5. Start Application
Launch the local Next.js server with active hot reload:
```bash
npm run dev
```
Open **`http://localhost:3000`** in your browser.

---

## 🛡️ Decentralized Security Auditing
*   **Double-Spend Protection**: Tokens assigned to projects are locked upon retirement, preventing them from being traded or re-assigned.
*   **Decentralized Access List**: Sensitive minting operations are guarded by `RolesController.sol`, ensuring only verified validators can authorize new offsets.
*   **Traditional Web2 Fallback**: Next-Auth encrypts session credentials to secure dashboard route navigation, complementing the decentralized security layer.

---

## 👨‍💻 Developer Credit
This platform was developed and is maintained by:
*   **Mithun Raj T** ([@BGJ06](https://github.com/BGJ06))

---

## 🙏 Acknowledgments
*   **Smart India Hackathon (SIH)** for the problem statement platform.
*   **Radix UI & Shadcn Teams** for accessible component structures.
*   **Wagmi & RainbowKit Teams** for making EVM-React integrations simple and fast.
