# the-illusion-of-decentralization
Technical audit of Phantom &amp; Trust Wallet infrastructure.
# 🔌 The Illusion of Decentralization: Why Your "Non-Custodial" Wallet is Blind Without a Master Switch

![NFT Meme Token: Vision vs Reality](meme.png)

> **"If they turn off the switch, your app shows a big fat ZERO."**
> Welcome to the reality of modern Web3. You were told you are completely decentralized because you own your seed phrase. You were told nobody can stop you. **That is a marketing lie.**

This repository serves as a technical audit of **Phantom** and **Trust Wallet** architecture. We prove that while your *private keys* are local, your *entire visibility, transaction routing, and privacy* are completely centralized and tethered to corporate servers. 

---

## 🛑 The "Light Client" Paradox (Expectation vs. Reality)

* **The Myth:** Your phone connects directly to the blockchain peer-to-peer.
* **The Math:** The Bitcoin blockchain is over 600 GB, Ethereum is over 1 TB, and Solana generates terabytes of data at lightning speed. Your smartphone cannot store or sync this.

Your mobile wallet is technically a **Light Client (SPV)**. It is completely blind. Every time you open the app to check your balance, your phone does not scan the blockchain — **it makes a standard HTTPS client-server request to a single point of failure.**

---

## 🛠️ Technical Proof #1: Phantom Wallet & The Solana RPC Trap

When you open Phantom, it utilizes **JSON-RPC** endpoints to fetch data. 

### The Hidden Code Flow:
By default, Phantom routes all your data through pre-configured endpoints like `api.mainnet-beta.solana.com` or their proprietary balancing proxies (`rpc.phantom.app`).

When you check your SPL tokens, your app secretly sends this exact corporate API request:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getTokenAccountsByOwner",
  "params": [
    "Your_Secret_Wallet_Address_Here",
    {
      "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
    },
    {
      "encoding": "jsonParsed"
    }
  ]
}
