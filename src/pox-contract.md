# The PoX Contract
The PoX contract, in this case PoX-4, is the crux of stacking & therefore the general consensus between Bitcoin & Stacks. Miners on Bitcoin mine STX blocks by sending Bitcoin to a weighted lottery; on the Stacks side, users can "stack" their STX, every two weeks, to receive a part of the Bitcoin mining rewards.

The fourth version of this, PoX-4 introduces the logic necessary for a decentralized Bitcoin-peg (sBTC). Specifically, it introduces the mechanics necessary for stackers to evolve into signers: instead of stacking & passively receiving mining rewards, signers are now incentivized to help process (by signing) peg-in(deposits), peg-out(withdraws) & peg-transfers (handoffs).

PoX-2 documentation lives [here](https://docs.stacks.co/docs/clarity/noteworthy-contracts/stacking-contract). Below we introduce the likely *new* public functions that'll fold into PoX-4. First, though, we'll review the user types before hopping into the PoX-4 lifecycle which lasts ~2100 blocks & is split over five windows.


# User Types
**Observer**
This is basically any active principal in the Stacks network, regardless of whether they hold STX or sBTC. They're not incentivized to *sign* anything, but, they are incentivized to throw a flat (aka start a penalty) if they see an issue with the protocol. Both Current & Registered Signers (below) are subsets of an Observer.

**Registered Signer (N+1)**
This is a principal that is or will register & vote in the current cycle. They are *not,* signers but more so the signers on-deck for the next cycle.

**Current Signer (N)**
This is a principal that is currently a signer for the current PoX cycle.

**Previous Signer (N-1)**
This is a principal that was the signer during the previous PoX cycle.


# PoX Lifecycle & Functions
Below we'll review the PoX lifecycle along with what public functions are avaiable to which user type. 

**Disbursement [x]**
The disbursement window is the very last step of the **previous** cycle. This is when the PoX rewards from the previous cycle are distributed to the previous signers. Since these rewards are disbursed in Bitcoin, someone needs to verify it on Stacks.

Observer -> (penalty-pox-reward-disbursement ...)

Previous Signer -> (prove-rewards-were-disbursed ...)

**Registration [1600 - x blocks]**
Summary of registration window

Registered Signer -> (register ...)

**Vote [300 blocks]**
Summary of vote window

Registered Signer -> (vote-for-threshold-wallet-candidate ...)

**Transfer [100 blocks]**
Summary of transfer window

Observer -> (penalty-pox-transfer ...)

**Penalize [100 blocks]**
Summary of penalty window
