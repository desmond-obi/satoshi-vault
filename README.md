# 🏦 SatoshiVault Protocol

**Unlock Bitcoin’s dormant value through decentralized collateralized lending.**
Borrow against your BTC without selling — maintain sovereignty, upside potential, and liquidity.

---

## 📘 Overview

**SatoshiVault Protocol** is a decentralized, Bitcoin-native lending system built on **Stacks Layer 2**, enabling users to **collateralize BTC** and borrow stablecoins in a **trustless**, **non-custodial** manner.

Through smart contracts, users deposit BTC, mint stable assets, and manage loan positions directly from their wallets.
The protocol enforces **on-chain collateralization**, **dynamic liquidation**, and **oracle-driven valuation**, ensuring both solvency and autonomy.

---

## ⚙️ System Overview

### Core Principles

* **Bitcoin-Backed Liquidity:** BTC serves as the sole collateral for borrowing stable assets.
* **Non-Custodial Architecture:** User retains full control of assets until loan liquidation.
* **Automated Liquidation Engine:** Protects system solvency through price-triggered liquidation.
* **Oracle-Verified Pricing:** Ensures accurate BTC valuation and fair loan calculations.
* **Stack-Based Execution:** Fully written in Clarity and secured by Bitcoin finality.

### Actors

| Actor              | Description                                                              |
| ------------------ | ------------------------------------------------------------------------ |
| **Borrower**       | Deposits BTC as collateral and borrows against it.                       |
| **Protocol Owner** | Admin entity authorized to initialize parameters and update price feeds. |
| **Oracle**         | Feeds BTC/USD price data for collateral evaluation.                      |

---

## 🧱 Contract Architecture

The protocol is implemented as a single **Clarity smart contract** with distinct functional modules:

### 1. **Initialization Layer**

Manages one-time setup and configuration:

* `initialize-platform`: Enables protocol after deployment.
* `update-collateral-ratio`, `update-liquidation-threshold`: Adjust risk parameters.
* `update-price-feed`: Updates BTC oracle price.

### 2. **Collateral & Loan Management**

Handles deposit, loan issuance, and repayment:

* `deposit-collateral`: Tracks BTC deposits into the protocol.
* `request-loan`: Creates new loan position, assigning loan ID and metadata.
* `repay-loan`: Completes repayment and releases collateral.

### 3. **Risk & Liquidation Module**

Enforces system safety:

* `calculate-collateral-ratio`: Computes collateral-to-loan ratio.
* `check-liquidation`: Triggers liquidation if ratio falls below threshold.
* `liquidate-position`: Terminates unsafe positions and clears records.

### 4. **Oracle Integration**

Maintains verified BTC price data for accurate loan computation:

* `collateral-prices` mapping stores the latest BTC price.
* Only contract owner (admin/oracle) may update the feed.

### 5. **Data Structures**

| Variable             | Purpose                                 |
| -------------------- | --------------------------------------- |
| `loans`              | Master loan registry keyed by loan ID.  |
| `user-loans`         | Tracks active loans per user principal. |
| `collateral-prices`  | Stores BTC price from oracle feed.      |
| `total-btc-locked`   | System-wide collateralized BTC.         |
| `total-loans-issued` | Counter for all loans created.          |

---

## 🔁 Data Flow

### 1. Loan Creation

1. User deposits BTC via `deposit-collateral`.
2. Oracle updates BTC price.
3. User calls `request-loan(collateral, amount)`.
4. Contract validates collateral ratio and mints loan record.
5. User receives stable asset equivalent to requested loan amount.

### 2. Loan Repayment

1. User calls `repay-loan(loan-id, amount)`.
2. Contract computes owed interest and verifies repayment.
3. Upon full repayment, collateral is unlocked.

### 3. Liquidation

1. Oracle price update triggers collateral ratio check.
2. If ratio < `liquidation-threshold`, loan is auto-liquidated.
3. Collateral is seized, and user’s loan record marked as “liquidated”.

---

## 📊 Platform Parameters

| Parameter                    | Default | Description                                   |
| ---------------------------- | ------- | --------------------------------------------- |
| **Minimum Collateral Ratio** | `150%`  | Minimum BTC value required per unit borrowed. |
| **Liquidation Threshold**    | `120%`  | Ratio at which position is liquidated.        |
| **Interest Rate**            | `5%`    | Static rate per loan cycle (block-based).     |
| **Fee Rate**                 | `1%`    | Protocol fee (currently inactive).            |

---

## 🔐 Security & Trust Model

* **Trustless Custody:** All operations occur on-chain. No intermediaries.
* **Immutable Logic:** No external dependencies beyond oracle.
* **Fail-Safe Liquidation:** Protocol automatically protects solvency.
* **Bitcoin Settlement:** Finality is inherited from Bitcoin through Stacks consensus.

---

## 🧩 Future Extensions

* Integration with decentralized stablecoin minting (e.g., USDA, aBTC).
* Dynamic interest rate models based on utilization.
* DAO-based governance for parameter adjustments.
* Multi-asset collateral (STX, wrapped BTC, etc.).

---

## 🧠 Summary

| Functionality       | Status        |
| ------------------- | ------------- |
| Collateral Deposit  | ✅ Implemented |
| Loan Issuance       | ✅ Implemented |
| Interest Accrual    | ✅ Implemented |
| Liquidation Engine  | ✅ Implemented |
| Oracle Feed         | ✅ Implemented |
| Governance Controls | ✅ Implemented |
