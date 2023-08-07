# sBTC Technical Roadmap
Welcome to the technical roadmap for sBTC, a comprehensive guide that will help you understand our goals, timelines, and the journey we're embarking on to bring our product to life. This roadmap is a dynamic document and we will continually update it as we achieve key milestones and refine our plans.


```mermaid
gantt
    dateFormat  YYYY-MM-DD
    title       sBTC Development roadmap
    excludes    weekends
    %% (`excludes` accepts specific dates in YYYY-MM-DD format, days of the week ("sunday") or "weekends", but not the word "weekdays".)

    section Milestones
    sBTC 0.1 target date      : milestone, 2023-09-30, 0d
    sBTC 0.2 target date      : milestone, 2023-10-31, 0d

    section Clarity contracts
    Clarity 0.1 contracts        : clar1, 2023-07-25, 2w
    Clarity 0.2 contracts        : clar2, after clar1, 4w
    Clarity 1.0 contracts        : clar3, after clar2, 4w

    section sBTC SDK core
    Core Stacks types              : sdk1, 2023-07-25, 2w
    sBTC transactions              : sdk2, after sdk1, 2w

    section Signer SDK
    Signer p2p protocol     : sign1, 2023-07-25, 4w
    ROAST signing rounds    : sign2, 2023-07-25, 4w
    Signer transactions     : sign3, after sdk2, 2w
    Signer state            : sign4, after sign1, 2w
    Signer leader election  : sign5, after sign4, 2w

    section Signer UI
    Signer binary            : sign5, after sign3, 2w
    Signer dashboard         : sign6, after clar1, 6w
    Signer UI                : sign7, after sign3, 4w

    section sBTC Docs
    sBTC High level            : doc1, 2023-07-25, 1w
    sBTC Protocol              : doc2, after doc1, 4w
    sBTC Releases              : doc3, after doc1, 2w
    sBTC Developer guides      : doc3, after doc2, 4w
  
```

Each effort in the roadmap represents a key aspect of our development process. The following sections provide more detail about what we aim to achieve with each effort.

## Milestones
These target dates represent the aimed releases as defined in [sBTC Releases](sbtc-releases.md).

## Clarity Contracts

### Clarity 0.1 contracts
The primary aim of this phase is to establish a robust foundation for sBTC by deploying the inaugural set of contracts for the 0.1 release. These contracts are designed to support the entire user flow and signer flow, as delineated in [sBTC 0.1](sbtc-dev.md). An important aspect to note about this set of contracts is the inherent assumption of good faith - we assume that both users and signers will engage honestly and ethically. Consequently, the system's liveliness could be compromised by user actions, and there are no economic incentives in place to ensure signers' appropriate maintenance of the sBTC wallet.

### Clarity 0.2 contracts
Expanding upon the groundwork laid in the 0.1 phase, this subsequent set of contracts enhances the system's security by eliminating potential threats from regular users to the system's liveliness. Although this release provides significant security improvements, it still operates under the assumption that signers act benevolently, as there remains no economic incentive for them to ensure proper behavior.

### Clarity 1.0 contracts
Our 1.0 contracts aim to offer the complete functionality necessary for the consensus-breaking 1.0 release. In this significant stage, the Stacks consensus rules will introduce economic incentives for signers to properly maintain the sBTC wallet. This development reduces the need for trust in the signers, adding an extra layer of security and reliability to the system.

## sBTC SDK Core

### Core Stacks types
This phase of development is dedicated to implementing the fundamental types that sBTC developers require to interact effectively with the Stacks blockchain. With these key primitives in place, developers should be fully equipped to construct arbitrary Stacks transactions, establish connections with Stacks nodes, broadcast their transactions, and monitor the status of these transactions on the blockchain.

### sBTC Transactions
In this next phase, our goal is to furnish a comprehensive toolkit for creating and decoding all sBTC-specific transactions on both the Bitcoin and Stacks blockchains. This toolkit is designed to streamline the process of constructing sBTC operations in either OP_RETURN or commit-reveal format to Bitcoin, as well as initiating contract calls to sBTC contract functions on the Stacks blockchain. This development will allow developers to engage with sBTC operations with ease and efficiency.

## Signer SDK

### Signer p2p protocol
The Signer peer-to-peer (p2p) protocol lays out the rules governing how signers discover and interact with each other. This initiative aims to precisely define this protocol, enabling any developer to implement their own signers, seamlessly connect with others, and engage in fluid communication. Furthermore, we aim to integrate essential types and methods into the sBTC SDK, simplifying the development of signers capable of communicating over this protocol.

### ROAST signing rounds
ROAST, a wrapper around FROST, is our chosen method for the signature rounds. This task involves constructing primitives that allow developers to effortlessly initiate signing rounds with ROAST and aggregate signatures for arbitrary messages.

### Signer transactions
With the basic signer primitives for ROAST and p2p communication established, this phase involves building a toolkit to construct and sign the specific transactions required by signers within the [sBTC 0.1](sbtc-dev.md) protocol. This toolkit should facilitate developers in creating and initiating a signing round for an sBTC fulfillment transaction, as well as the signer handover transaction.

### Signer state
Signers need to maintain an overview of pending sBTC operations, be able to validate incoming signature requests and determine if they need to initiate a signing round. This phase involves constructing an initial database schema for signers to manage, and providing functions to connect to Bitcoin and Stacks nodes, ensuring they can maintain this local state view.

### Signer leader election
Since all signers collectively observe incoming sBTC requests and need to respond, they also need to agree on who should initiate and aggregate signatures for a particular request. This task involves defining this aspect of the signer protocol and expanding the signer SDK with functions to execute leader election for specific requests.

## Signer UI

### Signer binary
The signer binary is a standalone executable program enabling signers to participate in the protocol. It builds on the SDK components, adding configuration options to allow signers to tailor the behavior of their specific signer to their requirements. While developers have the option to implement their own signer from the SDK and protocol specifications, we anticipate that most signers will opt for this reference implementation.

### Signer dashboard
Our dashboard aims to provide an insightful overview of signers' activities. As a web UI interacting directly with the Bitcoin and Stacks blockchains, it furnishes statistics on all signing rounds, detailing who is signing, which transactions have been signed, any pending transactions, and more.

### Signer UI
The goal with the Signer UI is to offer a streamlined interface for configuring and managing a local signer process. The signer process may require user input for certain decisions; this UI is designed to provide a clean, user-friendly interface for such interactions.
## sBTC Docs

### sBTC High Level
The high-level sBTC documentation serves to align understanding around the purpose, structure, and components of sBTC. It will offer a bird's eye view of the project, setting the stage for more detailed exploration of specific elements in subsequent sections. This should be the starting point for anyone seeking to gain a basic understanding of what sBTC is and its role within the broader ecosystem.

### sBTC Protocol
The sBTC protocol documentation is designed to act as the definitive guide to the sBTC protocol. It will encompass all the requisite details for a comprehensive understanding of its operation, the economic trade-offs involved, and the measures taken to secure the protocol. This document should be thorough enough to serve as a reference for alternative implementations of the protocol, ensuring accurate replication of its design and function.

### sBTC Releases
The sBTC release documentation is a pivotal resource, fostering an understanding of the developmental strides leading up to the full implementation of the sBTC protocol. This section should detail the various releases we've made, drawing attention to how they deviate from the final protocol. It's essential to highlight the simplifications or assumptions each release is predicated on and elucidate why we've chosen to adopt these steps in our development process.

### sBTC Developer Guides
Our developer guides aim to provide a developer-friendly overview of the tools, SDKs, and functionalities offered in the sBTC reference implementation and various releases. These guides are designed to help developers understand and build applications using sBTC, fostering creativity and innovation within the ecosystem. The ultimate goal is to ensure developers can effectively contribute to the growth and diversification of the ecosystem using the sBTC platform.
