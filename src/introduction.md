# Welcome

Welcome to the sBTC developer documentation.
Here you will find the most up-to-date description of what sBTC is, how it's implemented and how you can get involved.

# Introduction to sBTC

To understand sBTC, we first need to understand the current limitations of Bitcoin (BTC).

Bitcoin is to date the most secure and decentralized blockchain.
While Bitcoin is the largest cryptocurrency by market cap, comparatively few applications exist within the Bitcoin ecosystem.
Developers interested in building applications for the Bitcoin community often find it difficult or impossible to implement their logic directly on the Bitcoin chain.
Although Bitcoin has a simple scripting system built in, it lacks lacks the expressiveness of many other smart contract languages.

sBTC is for:
- Bitcoin holders who want to participate in smart contracts.
- Developers who want to build applications on Bitcoin.

sBTC empowers developers to build applications on Bitcoin by bridging Bitcoin and [Stacks](https://www.stacks.co/).
We achieve this by introducing a fungible token (sBTC) on the Stacks blockchain.
The token has the following properties:

- **Same value as BTC**. sBTC can always be exchanged 1:1 for BTC on the Bitcoin chain, as long as the Stacks blockchain is operational.
- **Open membership**. Anyone can participate in the sBTC protocol. No centralized entity maintains custody over any BTC in the protocol.

Other tokens which try to achieve the same end as sBTC are

- [xBTC](https://www.stacks.co/blog/tokensoft-wrapped-fundamental-bitcoin-defi-building-blocks-xbtc)
- [TODO #2](https://github.com/stacks-network/sbtc-docs/issues/2) Which other wrapped Bitcoin tokens exist on Stacks?

While these tokens all achieve the same value as BTC, they maintain BTC reserves through trusted entities.
sBTC is the only truly decentralized Bitcoin backed asset on Stacks.

# How does sBTC work?

Bitcoin holders can do two things to interact with sBTC, deposit and withdraw.
Both of these operations are controlled through special Bitcoin transactions.

To deposit BTC into sBTC, a Bitcoin holder would create a deposit transaction on the Bitcoin chain.
This deposit transaction informs the protocol how much BTC the holder has deposited, and to which Stacks address the holder wishes to receive the sBTC.
The sBTC system responds to the deposit transaction by minting sBTC to the given Stacks address.

To withdraw BTC, a Bitcoin holder creates a withdrawal transaction on the Bitcoin chain.
This withdrawal transaction informs the protocol how much sBTC the holder wishes to withdraw, from which stacks address th sBTC should be withdrawn and which Bitcoin address should receive the withdrawn BTC.
In response to this transaction, the sBTC system burns the requested amount of sBTC from the given Stacks address and issues a BTC payment to the given BTC address with the same amount.


[TODO #4](https://github.com/stacks-network/sbtc-docs/issues/4): Explain the bitcoin deposit and withdrawal from a high level perspective (no wire formats)

# Where to go next?
If you want to use sBTC, check out [How to Deposit](./how-to-deposit.md) and [How to Withdraw](./how-to-withdraw).

If you are curious which applications are being built on sBTC, read [Applications using sBTC](./applications.md).

If you want to understand how sBTC achieves a secure open-membership wrapping of Bitcoin, look into the design documentation. A good start is the [Architecture Overview](./architecture.md)
