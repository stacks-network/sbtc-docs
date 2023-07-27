# Stacker responsibilities
The stackers are gaining a lot of responsibilities in sBTC. This chapter outlines the new role of the stackers, and how they interact with each other to fulfill their duties.

- sBTC Wallet address generation
- sBTC Wallet handoff
- sBTC Withdrawal fulfillment

## sBTC wallet address generation
Stackers in sBTC operate in Reward cycles, the same way they have been in previous versions of [PoX](https://github.com/stacksgov/sips/blob/main/sips/sip-007/sip-007-stacking-consensus.md). A reward cycle is approximately two weeks and during this time, the set of stackers is fixed.

At the start of every reward cycle, the stackers must provide a single Bitcoin address in the PoX contract. This is the sBTC wallet address for that cycle. Once this address is provided, the previous set of Stackers **must** execute a wallet handoff to this newly generated sBTC wallet address.

| ❓Open question |
|----|
|How do we ensure stackers are motivated to complete the wallet handoff?|

## sBTC Wallet handoff
TODO...

## sBTC Withdrawal fulfillment
TODO...
