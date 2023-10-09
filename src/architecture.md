# Architecture Overview
In [Introduction to sBTC](./introduction.md) we established thath sBTC is a fungible token on the Stacks blockchain, and explained how users interact with the protocol.
This chapter takes a closer look at the major entities in the sBTC protocol and briefly explain how they interact. The following chapters goes into more details on each component.

sBTC builds on the Proof-of-Transfer (PoX) consensus mechanism defined in [SIP-007](https://github.com/stacksgov/sips/blob/main/sips/sip-007/sip-007-stacking-consensus.md).
This SIP introduces the concept of stacking, which is to lock STX for a period of time to earn Bitcoin rewards.
Stacking is performed through a special smart contract, called the PoX contract.
People who stack are called stackers.

In sBTC we introduce the following changes to Stacks consensus:
* TODO: Introduce sBTC Token contract...
* The PoX contract is extended to include sBTC as a [SIP-010 fungible token](https://github.com/stacksgov/sips/blob/main/sips/sip-010/sip-010-fungible-token-standard.md).
* Stacks miners must include sBTC mint and burn transactions in their blocks in response to valid sBTC requests on the Bitcoin chain.
* Stackers must collectively generate a Bitcoin address every reward cycle and publish it in the PoX contract as the sBTC wallet address.
* Stackers are required to respond to sBTC withdrawal requests.

The following chart illustrates the main components of sBTC.

```mermaid
erDiagram
    "PoX Contract" ||--|| "Stacks blockchain" : "Depoloyed on"
    "sBTC Token" }o--|| "PoX Contract": "Defined in"
    "Stackers" }|--|| "PoX Contract": "Participate in"
    "sBTC Requests" }o--|| "Stacks blockchain": "Validated by"
    "Stackers" }|--o{ "sBTC Requests": "Respond to"
    "User" }o--o{ "sBTC Requests": "Create"
```

Now that we have established the main components of sBTC, we're ready to dig deeper in the actual workings of it.
The following three chapters explains different aspects of the sBTC design and can be read in any order.

[^1] PoX is the abbreviation for the Proof-of-Transfer consensus mechanism introduced in [SIP-007](https://github.com/stacksgov/sips/blob/main/sips/sip-007/sip-007-stacking-consensus.md).
