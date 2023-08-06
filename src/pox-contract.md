# The PoX Contract
The PoX contract, in this case PoX-4, is the crux of stacking & therefore the general consensus between Bitcoin & Stacks. Miners on Bitcoin mine STX blocks by sending Bitcoin to a weighted lottery; on the Stacks side, users can "stack" their STX, every two weeks, to receive a part of the Bitcoin mining rewards.

The fourth version of this, PoX-4 introduces the logic necessary for a decentralized Bitcoin-peg (sBTC). Specifically, it introduces the mechanics necessary for stackers to evolve into signers: instead of stacking & passively receiving mining rewards, signers are now incentivized to help process (by signing) peg-in(deposits), peg-out(withdraws) & peg-transfers (handoffs).

PoX-2 documentation lives [here](https://docs.stacks.co/docs/clarity/noteworthy-contracts/stacking-contract). Below we introduce the likely *new* public functions that'll fold into PoX-4. First, though, we'll review the user types before hopping into the PoX-4 lifecycle which lasts ~2100 blocks & is split over five windows.

# User Types
**Observer**
This is basically any active principal in the Stacks network, regardless of whether they hold STX or sBTC. They're not incentivized to *sign* anything, but, they are incentivized to throw a flat (aka start a penalty) if they see an issue with the protocol.

**Registered Signer**
This is a principal that is or will register & vote in the current cycle. They are *not,* signers but more so the signers on-deck for the next cycle.

**Current Signer**
This is a principal that is currently a signer.

# PoX Lifecycle
**Disbursement [x]**

**Registration [1600 - x blocks]**

**Vote [300 blocks]**

**Transfer [100 blocks]**

**Penalize [100 blocks]**

