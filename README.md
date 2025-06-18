# HVAC Rewards Admin Portal

A frontend management interface for an on-chain customer loyalty and rewards program built on Ethereum.

---

## 🧭 Overview

The **HVAC Rewards Admin Portal** is a web-based interface designed to manage a sophisticated, on-chain customer loyalty program. It provides a user-friendly layer that abstracts away the complexities of blockchain interactions, allowing an administrator to manage the entire customer rewards lifecycle through simple forms and button clicks.

At the heart of the system:

- Customers earn **non-transferable points (SPN)** for services.
- These are converted into **SRV tokens**, which represent real value.
- SRV is auto-staked to generate yield.
- Yield is paid out to customers in **real-world rewards** (e.g. gift cards).

---

## 🏗️ System Architecture & Core Contracts

The system is powered by four key smart contracts, integrated into the Admin Portal:

| Contract            | Role                             | Address (example)         |
|---------------------|----------------------------------|---------------------------|
| **SPN Token**       | Loyalty point tracker (ERC20)    | `0x29...1818`             |
| **Converter**       | SPN → SRV redemption              | `0x52...FA41`             |
| **Staking Vault**   | Stake SRV → Receive SREV         | `0x69...ddB7`             |
| **Yield Engine**    | Distributes USDC yield           | `0x71...D9DE`             |

### Token Flow

1. Admin mints SPN to customer wallets.
2. SPN is burned to mint SRV (via Converter).
3. SRV is staked in the Vault to receive SREV.
4. SREV earns USDC yield via the Yield Engine.

---

## 🛠 How to Use the Admin Portal

### Step 1: Initial Setup

- **Connect Wallet:** Use the `Connect Wallet` button. Must be an admin (DAO operator).
- **Configure Addresses:** Portal will auto-fetch contract addresses. If it fails, input them manually.

### Step 2: Customer Management

- **Add Customer:**  
  - Enter a unique identifier (e.g., `CUST-1234`).  
  - Click `Add New Customer` to generate a new wallet.
- **Issue SPN Points:**  
  - Select a customer.  
  - Enter point amount.  
  - Click `Issue SPN Points`.

### Step 3: Automated Rewards Flow

- **Monitor Balances:**  
  - Use `Refresh All Balances` to update SPN/SRV/SREV info.
- **Convert & Stake Flow:**  
  - Click `Convert & Stake` for a customer.  
  - This triggers:
    1. SPN approval to Converter.
    2. `burnAndConvert()` call → SPN → SRV.
    3. SRV approval to Staking Vault.
    4. SRV is staked and SREV is received.

### Step 4: System & Payout Management

- **Fund Yield Pool:**  
  - Enter USDC amount and fund Yield Contract.
- **Set APR:**  
  - Enter APR in **basis points** (e.g., `500 = 5%`).
- **Process Payouts:**  
  - Off-Chain: Deliver gift cards or real-world rewards.  
  - On-Chain: Click `Claim Yield` to finalize on blockchain.

---

## 🔐 Critical Security Note

> **WARNING:** The current customer wallet implementation is for **demo/testing only**.

- When a customer is added, a private key is generated and stored in browser memory — **NOT SAFE for production**.
- All transactions are auto-signed using this temporary key.

### ✅ Production Architecture Requirements

| Component              | Recommendation                                               |
|------------------------|--------------------------------------------------------------|
| **Wallet Management**  | Secure backend using AWS KMS / HashiCorp Vault / GCP KMS     |
| **Transaction Signing**| Backend signs transactions, frontend only requests actions   |
| **Frontend Role**      | Display data + interact with backend API only                |

Do **NOT** store or access private keys in-browser in a production system.

---

## 📂 Project Status

This repository serves as a demonstration and early-stage admin tool. Production deployment requires:

- Backend API for wallet/account control
- Hardened authentication
- Role-based access control
- Enhanced key management

---

## 🔗 Related Links

- [Service Coin DAO](https://service.money)
- [SREV Staking Page](https://servicerevenue.net/)
- [SPN Loyalty Overview](https://github.com/servicecoinrwb)

---

## 📜 License

MIT © Service Coin DAO
