# The PoX Contract
The PoX contract, PoX-4, is the crux of stacking & therefore the general consensus between Bitcoin & Stacks. Miners on Bitcoin mine STX blocks by sending Bitcoin to a weighted lottery every block; on the Stacks side, users can "stack" their STX, every two weeks, for receive a part of the Bitcoin mining rewards.

The fourth version of this, PoX-4, introduces the logic necessary for a decentralized two-way Bitcoin-peg (sBTC). Specifically, it introduces the mechanics necessary for stackers to now evolve into signers: instead of stacking & passively receiving mining rewards, signers are now incentivized to help process (by signing) peg-ins(deposits), peg-outs(withdraws) & peg-transfers (handoffs).

PoX-2 documentation lives [here](https://docs.stacks.co/docs/clarity/noteworthy-contracts/stacking-contract). Below we introduce the likely *new* public functions that'll fold into PoX-4. First, though, we'll review the user types before hopping into the PoX-4 lifecycle which lasts ~2100 blocks & is split over five windows.


# User Types
**Observer**
Any active principal in the Stacks network, regardless of whether they hold STX or sBTC. They're not incentivized to *sign* anything, but, they are incentivized to throw a flag (aka start a penalty) if they see an issue with the protocol. Both Current & Registered Signers (below) are subsets of an Observer.

**Pre-Registered Signer**
For the developer release, a pre-registered stacker is an observer that wants to become a signer for at least one future cycle. 

**Registered Signer**
For the developer release, this is a stacker that is now registered to be a signer (for the next cycle); they're either a pre-registered stacker or a current-signer.

**Voted Signer**
For the developer release, this is a registered signer that has now voted for a threshold wallet candidate (for the next cycle).

**Current Signer**
This is a principal that is a signer for the current PoX cycle. The previous cycle they registered & voted. This current cycle, as a current signer, they can also register & vote for the *next* cycle.


# PoX Lifecycle & Functions
Below we'll review the PoX lifecycle along with what public functions are avaiable to which user type. 

**Disbursement [x]**
The disbursement window is the first window yet also acts as the final window of the **previous** cycle. This is when the PoX rewards from the *previous* cycle are disbursed to the previous signers. Since these rewards are disbursed in Bitcoin, someone [either Observer or any of the Signer user types] needs to verify the disbursements on Stacks.   

Observer -> (penalty-pox-reward-disbursement ...)
Observer -> (prove-rewards-were-disbursed ...)

**Registration [1600 - x blocks]**
Once disbursement has been proven on Stacks, the registration window opens (it's worth noting that this is the only window with a dynamic start height). This is the window when either pre-registered signers or current signers register to be a signer for the next cycle. 

Pre-registered Signer -> (signer-register ...)
Current Signer -> (signer-register ...)

**Vote [300 blocks]**
After, registered signers can vote for the next cycle sBTC threshold/peg-wallet. The goal here is for registered signers to reach a 70% consensus on a candidate. 

Observer -> (penalty-pox-reward-disbursement ...)
Registered Signer -> (vote-for-threshold-wallet-candidate ...)

**Transfer [100 blocks]**
Once the vote windows is over, a new threshold/peg wallet should've been - this is now the window when the current signers can transfer the pegged-sBTC balance to the new threshold wallet (aka passing on the responsibility).

Observer -> (penalty-pox-transfer ...)
Observer -> (balance-was-transferred ...)

**Penalize [100 blocks]**
The final window before the new cycle starts with the disbursement window; the main role here is for any observer to throw a flag (aka start a penalty) if they see an issue with the protocol.

Observer -> (penalty-balance-transfer ...)
Observer -> (penalty-unhandled-request ...)
