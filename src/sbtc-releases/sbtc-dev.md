# sBTC Developer Release (0.1)

## Introduction

The sBTC Developer Release implements a subset of the sBTC protocol. It aims
to come as close as possible without requiring a hard fork. The Developer
Release is implemented as a decentralized stacking pool on Stacks 2.4 that distributes rewards in sBTC. It means sBTC is secured by the locked STX in the stacking pool.

The Stacking pool can have at most 100 members. It is active only if the stacked amount is 1 million STX or more. The minimum required locking amount for pool members is 10,000 STX. The pool can be joined by anyone as long as there are slots available.

Pool members are responsible managing a Bitcoin wallet that backs sBTC. They are responsible for handling deposits and withdrawals of Bitcoin to and from the wallet with the following exceptions:

- Bitcoin withdrawal requests can only be submitted as a Stacks transaction. The full release of the sBTC Protocol will also allow withdrawal requests to be submitted as a Bitcoin transaction.
- Deposits and withdrawals are frozen while the pool is inactive.

## Caveats

As the name implies, this is a developer release of the sBTC Protocol. Its purpose is to validate certain aspects of the protocol design; namely, the process of jointly managing a Bitcoin wallet by means of a threshold scheme, the ability to verify Bitcoin transactions in Clarity, to mint and burn sBTC based on those transactions, and to manage the hand-off process. Signer incentives are not aligned with those of the full version of the sBTC Protocol. The benevolence of the Signers is assumed and only minor penalisation for bad behaviour exists. **The Developer Release should therefore not be used on mainnet.**

## Components

The sBTC Developer Release is made up of multiple on-chain components. These components can be logically divided into the following groups:

1. Controller
2. Registry
3. Token
4. Deposit
5. Withdraw
6. Pool
7. Hand-off
8. Bitcoin library

### Controller

The controller is a single Clarity smart contract that manages which other contracts are part of the protocol. The other contracts use the controller for access control. The controller also exposes a pathway to upgrade the protocol by changing the access list.

### Registry

The registry contract functions as the main storage space for the protocol. The other components use the registry to store protocol state. It is intended to allow for more seamless upgrades by keeping the registry the same across future soft upgrades.

TODO [#39](https://github.com/stacks-network/sbtc-docs/issues/39): List the things the registry stores and why, also how those things are updated. Pay special attention to the get-and-update function.

### Token

The sBTC token is implemented as a SIP-010 fungible token. It uses the controller for access control so that the sBTC Protocol contracts may mint and burn sBTC tokens.

### Deposit

The deposit contracts verify Bitcoin deposit transactions. They trigger an sBTC mint to the recipient found in the Bitcoin transaction. The transaction format is described in the [section on Bitcoin transactions](../sbtc-operations/bitcoin-transactions.md).

TODO: [#41](https://github.com/stacks-network/sbtc-docs/issues/41): Explain how deposits are processed. Link back to BTC transaction structure and minimum confirmations required.

### Withdraw

The withdrawal contracts process withdrawal requests and verify Bitcoin withdrawal transactions.

Withdrawal requests start on Stacks. However, an sBTC holder that does not own any STX can still sign a Stacks transaction and pay transaction fees in sBTC. The protocol exposes a mechanism to submit the withdrawals by means of a sponsored Stacks transaction. The sponsor will then receive an amount of sBTC in return for sponsoring the transaction. A sponsor can decide to sponsor the transaction if they find the fee acceptable.

TODO [#42](https://github.com/stacks-network/sbtc-docs/issues/42): Explain steps and how redemptions are processed. Link back to BTC transaction structure and minimum confirmations required. Explain more what the process looks like.

### Pool

The sBTC Stacking pool proper that allows Stackers to participate in the sBTC Protocol. Stackers will participate in the distributed key generation scheme and individually vote on the next Bitcoin wallet address to use. They vote by locking their STX for the next cycle.

TODO [#43](https://github.com/stacks-network/sbtc-docs/issues/43): Is it deserving of a sub page? Will be a lot of information here on how to use the Stacking pool and voting.

### Hand-off

The contract that verifies that the hand-off to the next Bitcoin wallet was done properly.

TODO [#44](https://github.com/stacks-network/sbtc-docs/issues/44): Mostly automated, the Signer binary (or who?) will submit hand-off proof Stacks transaction.

### Bitcoin library

The helper library that makes it easier to decode and interpret Bitcoin transaction in Clarity. Any component that receives Bitcoin transactions uses this library. It is an adapted version of an earlier open source library [link and attribution here].

## Contracts

[Does this section go here? It will be a long document.]

The sBTC Developer Release consists of the following contracts:

[list here.]


## API

[List of public functions that downstream apps should use]


## Events

[Auto-generated list of events based on latest tagged release.]

## Errors

[Auto-generated list of errors based on latest tagged release.]

## Running sBTC Developer Release
Detailed instructions can be found on the [bootstrap](./sbtc-dev/bootstrap.md) page.

## Things we need to formulate

- Bootstrapping question, when is the protocol viable?
- What if the signers process a peg-in it anyway while in a bad state? Do we care?
- Testnet controller so we can put the protocol in a state so we can test stuff on testnet.


