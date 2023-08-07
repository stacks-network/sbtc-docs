# Consensus Level changes

We have already talked in detail about the individual unique components of sBTC that work together to make the protocol work, and it might be worthwhile to take a step back and look at how sBTC is implemented at a consensus level (i.e. from the perspective of the stacks blockchain itself).

sBTC is being released bundled with a number of consensus level changes that add significant quality of life improvements to the Stacks blockchain, and this release is being called the *Nakamoto* release. The *Nakamoto* release reduces transaction confirmation latency from hours to seconds, and significantly improves the security budget of the Stacks blockchain (i.e. the minimum number of nodes that need to be compromised to take over the network).

*Nakamoto* replaces the competitive mining paradigm that produces blocks in Stacks 2.0 with a co-operative model between the miners and the Stackers, where miners propose blocks and stacks validate them. Additionally, there are routine checkpoint writes to the Bitcoin blockchain performed by the stackers that works to finalize these blocks (i.e. the network security budget of the Stacks chain becomes that of the Bitcoin blockchain).

Miners co-ordinate to propose blocks from pending transactions, and if a 2/3 majority of the miners agree on the composition of the block, then they are proposed. These blocks are then validated by the Stackers, and during this validation phase, the Stackers also ensure that ALL legitimate sBTC  transactions in the Bitcoin blockchain are addressed. If they are not, then the block is rejected.

There are built in economic incentives for the Stackers to ensure that ALL valid sBTC transactions are included in the block. If they do not address sBTC deposits and withdraws in a timely fashion, then their PoX rewards are diverted to a burn address and their STX are indefinitely locked until all unfulfilled deposits and withdrawals are handled.

If 2/3rd of the Stackers agree that the block is valid, they sign off on this block, at which point this block is added to the blockchain and becomes the new tip. All blocks have to build on top of it. Thus, unless the network is compromised, Stacks guarantees a single block (soft) finality from the *Nakamoto* release. This makes sBTC fulfillment a much more dependable and reliable process for users of the protocol.

Moreover, because Stacks no longer forks, sBTC can be treated like any other SIP-10 token and mined and used quickly on Stacks without the need to wait for Bitcoin confirmations.

sBTC is also not really affected by short lived Bitcoin forks as the protocol already ensures that there are sufficient number of Bitcoin block confirmations before being fulfilled that it is almost guaranteed that they will never be orphaned by the time they are picked up by a block producer set. As such, sBTC is not speculatively instantiated or destroyed and can only materialize or dematerialize once its deposit and withdraw transactions are sufficiently confirmed, and is completely independent of Stacks recovery mechanism from soft Bitcoin forks.

