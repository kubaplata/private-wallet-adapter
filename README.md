# Solana Private Wallet Adapter Standard

## Introduction
Currently, `@solana/wallet-adapter` is a standard used across most Solana applications. However, with recent development of Solana Private Programs, there is a need of a new standard supporting wider scope of functionalities required by PSPs.

## Problem
Currently, we see at least one major problem with safety of PSPs. As a user, to interact with PSPs, you are required to trust the UI you are interacting with.

Once you login (by signing the Light Protocol-specific message), the UI gets full authority of spending your funds. You are required to trust the UI that it won't perform any malicious operations.

Imagine that traditional Solana DApps work the same - instead of connecting your wallet with Solana Wallet Adapter, you are copy-pasting your private key, giving the DApp full authority over your funds.

This is not acceptable.

## Solution
Solana Private Wallet Adapter Standard is solution we're proposing. Instead of logging in to every PSP UI, extension wallets should be always securely 'logged in' in the background and act as a communication layer between user and the UI.
This way, UI never gets access to user funds, but rather sends requests to the wallet to fetch data, generate UTXOs, generate proofs and send them to the relayer.

This works very similarly to the existing Solana Wallet Adapter standard. To perform any operation, UI has to ask user for approval (via wallet).

## Technical Overview
This is a Typescript package intended to be used as an add-on to existing Solana Wallets. Next to `window.solana` existing in the browser global object, it will register `window.solana.private`. The `private` object will hold all methods and states related to PSPs.

Currently supported methods are:
- getBalance - returns balance of shielded SOL
- getBalances - returns balances of shielded SPL Tokens
- createUtxo - returns UTXO object created from parameters
- generateProofAndSendPrivateTransaction - generates ZK proof, sends private transaction to the relayer
- etc, more to be added soon

