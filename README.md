# Simple Blockchain in Node.js/TypeScript

This project implements a minimal, educational blockchain using Node.js and TypeScript. It demonstrates fundamental blockchain concepts such as transactions, blocks, hashing, proof-of-work, digital signatures, and a basic peerless ledger.

---

## Features

* **Transactions**: Define transfers between wallets (amount, payer, payee).
* **Blocks**: Container for transactions, linked by SHA-256 hashes.
* **Proof-of-Work**: Simple MD5-based mining puzzle requiring four leading zeros.
* **Digital Signatures**: RSA key pairs used to sign and verify transactions.
* **Singleton Chain**: A single global blockchain instance with a genesis block.

---

## Prerequisites

* [Node.js](https://nodejs.org/) (v14 or later)
* npm (comes with Node.js)

---

## Getting Started

1. **Clone the Repository**

   ```bash
   git clone <repo-url>
   cd blockchain-ts
   ```

2. **Install Dependencies**

   ```bash
   npm install
   ```

3. **Compile and Run**

   * For continuous TypeScript compilation:

     ```bash
     npm run dev
     ```
   * To build then execute the code once:

     ```bash
     npm start
     ```

---

## Project Structure

```plaintext
blockchain-ts/
├── index.ts       # Main source: Transaction, Block, Chain, Wallet
├── package.json   # npm scripts and dependencies
├── package-lock.json
├── tsconfig.json  # TypeScript compiler options
└── README.md      # Project documentation
```

---

## Core Classes

### `Transaction`

* **Fields**: `amount`, `payer` (public key), `payee` (public key).
* **Methods**: `toString()` — serializes to JSON for hashing and signing.

### `Block`

* **Fields**: `prevHash` (hash of previous block), `transaction`, `ts` (timestamp), `nonce` (random number).
* **Getters**: `hash` — SHA-256 of the stringified block.

### `Chain`

* **Pattern**: Singleton (`Chain.instance`).
* **State**: `chain` — array of `Block` objects, starting with a genesis block.
* **Methods**:

  * `lastBlock` — retrieves the most recent block.
  * `mine(nonce)` — brute-forces an MD5-based proof-of-work puzzle.
  * `addBlock(transaction, senderPublicKey, signature)` — verifies signature, mines the block, and appends it.

### `Wallet`

* **Fields**: `publicKey`, `privateKey` (RSA key pair in PEM format).
* **Methods**: `sendMoney(amount, payeeKey)` — creates and signs a `Transaction`, then calls `Chain.instance.addBlock`.

---

## Example Usage

In `index.ts`, three wallets are created and transactions are executed:

```ts
const satoshi = new Wallet();
const bob     = new Wallet();
const alice   = new Wallet();

satoshi.sendMoney(50, bob.publicKey);
bob.sendMoney(23, alice.publicKey);
alice.sendMoney(5, bob.publicKey);

console.log(Chain.instance);
```

After running `npm start`, the console will display the blockchain with each mined block and its verified transaction.

---

## What I Learned

This project reinforced:

* Data serialization for cryptography.
* Block linking via SHA-256 hashes.
* Proof-of-work mechanics.
* RSA key generation, signing, and verification.
* TypeScript project setup and compilation.

---

## License

This project is licensed under the MIT License.
