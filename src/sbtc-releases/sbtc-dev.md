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

Even though the controller is at the heart of the sBTC protocol, it is very simple by design. It only exposes two functions: one to check if a specific contract principal is part of the protocol and another to upgrade the protocol. 

The check function is used by all protocol contracts that need to guard functions that may only be called by the protocol itself. It is necessary to ensure that no third-party contracts can do things like mint and burn sBTC tokens.

The upgrade function allows for the list of protocol contracts to be changed. The function itself is also guarded by the check function. It is designed this way to allow for the protocol upgrade process itself to be defined in a separate contract at a later stage. It also makes it possible for that process itself to be upgraded over the lifetime of the protocol. Adding upgrade logic to the controller would make it opinionated and lock in a specific upgrade process.

### Registry

The registry contract functions as the main storage space for the protocol. The other components use the registry to store protocol state. It is intended to allow for more seamless upgrades by keeping the registry the same across future soft upgrades.

TODO [#39](https://github.com/stacks-network/sbtc-docs/issues/39): List the things the registry stores and why, also how those things are updated. Pay special attention to the get-and-update function.

### Token

The sBTC token is implemented as a SIP-010 fungible token. As such, it is supported by a wide range of wallet providers and on-chain protocols. The token contract itself contains no protocol logic. It only contains guarded functions that allows other protocol contracts to manage the tokens. This design was chosen to allow for maximum flexibility and for the token to survive potential future protocol upgrades without requiring a hard fork. The Developer Release of the sBTC Protocol only allows minting via the deposit component and burning via the withdrawal component.

### Deposit

Deposits follow a two-step process. A Bitcoin holder first sends Bitcoin to a specially generated Taproot escrow address. The output sent to this address can be spent by either the threshold signer group or the original Bitcoin holder. The holder must then wait for the signer group to collect the Bitcoin and send it to the sBTC peg wallet address. Once that Bitcoin transaction is mined, it is submitted to the deposit smart contract in order to prove that the Bitcoin was moved into the protocol. sBTC tokens are minted to the recipient immediately When the proof is verified.

The deposit contracts verify the aforementioned Bitcoin deposit transactions. Bitcoin transactions should be in a specific format as outlined in the [section on Bitcoin transactions](../sbtc-operations/bitcoin-transactions.md). The deposit component only allows Bitcoin transactions that are signed by the threshold signer group. It enables the signer group to decide which transactions to process and when completely off-chain. The deposit component will trigger an sBTC mint to the recipient extracted from the Bitcoin transaction if the transaction submitted to the deposit component passes all the checks.

TODO: [#41](https://github.com/stacks-network/sbtc-docs/issues/41): Explain how deposits are processed. Link back to BTC transaction structure and minimum confirmations required.

### Withdraw

Withdrawal requests are submitted on the Stacks chain for the Developer Release. However, it does not mean that the sBTC holder needs STX tokens in order to successfully withdraw Bitcoin from the sBTC protocol. They can instead post a sponsored withdrawal request that allows them to pay the transaction fee directly in sBTC. A sponsor can decide to sponsor the transaction if they find the fee acceptable.

Withdrawal requests are stored on-chain in the order they are received. The signer group observe these events to learn about all pending withdrawals. They will then coordinate the threshold signing process off-chain in order to sign Bitcoin transactions fulfilling the requests. It is paramount that the protocol learns of all pending withdrawal requests so that the signer group is not able to arbitrarily ignore requests. They must acknowledge and process them lest the protocol will penalise the signer group by locking their posted STX tokens until all the backlog is cleared.

As far as the protocol goes, all that matters is that the destination receives the requested amount of Bitcoin. After the sBTC holder successfully submits a withdrawal request, the right amount of Bitcoin sent to the address can be used to show that the withdrawal request was processed. This flexibility allows for the signers to act out-of-band in order to restore the protocol if need be.

TODO [#42](https://github.com/stacks-network/sbtc-docs/issues/42): Explain steps and how redemptions are processed. Link back to BTC transaction structure and minimum confirmations required. Explain more what the process looks like.

### Pool

The sBTC Stacking pool proper that allows Stackers to participate in the sBTC PRotocol. Stackers will participate in the distributed key generation scheme and individually vote on the next Bitcoin wallet address to use. They vote by locking their STX for the next cycle.

TODO [#43](https://github.com/stacks-network/sbtc-docs/issues/43): Is it deserving of a sub page? Will be a lot of information here on how to use the Stacking pool and voting.

### Hand-off

The contract that verifies that the hand-off to the next Bitcoin wallet was done properly.

TODO [#44](https://github.com/stacks-network/sbtc-docs/issues/44): Mostly automated, the Signer binary (or who?) will submit hand-off proof Stacks transaction.

### Bitcoin library

The helper library that makes it easier to decode and interpret Bitcoin transaction in Clarity. Any component that receives Bitcoin transactions uses this library. It is an adapted version of an earlier open source library [link and attribution here]. This version is optimised for the specific demands of the sBTC protocol. Although on-chain block space demand for the sBTC Developer Release might be limited, care was taken to make sure it is a good citizen and use the lowest amount of runtime budget necessary to function.

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

## Upgrading

As mentioned in the section explaining the sBTC Controller component, only the upgrade function itself is explicitly defined. The upgrade process of the protocol is not explicitly defined. During development, various upgrade pathways may be proposed and implemented. For example:

- Upgrade by means of a vote by a quorum of sBTC holders.
- Upgrade by means of a vote by a quorum of Signers.
- Upgrade by means of a combination of the above.
- Upgrade by means of some other mechanism.

When running a local developer testnet version of sBTC Developer Release, the deploying developer may use the debug controller to initiate an upgrade at will.

## Things we need to formulate

- Bootstrapping question, when is the protocol viable?
- What if the signers process a peg-in it anyway while in a bad state? Do we care?
- Testnet controller so we can put the protocol in a state so we can test stuff on testnet.


