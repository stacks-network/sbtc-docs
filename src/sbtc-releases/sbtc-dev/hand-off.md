# Hand-off process

The contract call required to confirm the hand-off process is `sbtc-hand-off::relay-hand-off-fulfillment`.

The hand-off process can start during the `start-transfer-window` window of the current cycle which is defined in the sbtc-stacking-pool contract as the `transfer` state. The length of the window is defined in the sbtc-staking-pool contract as `normal-transfer-period-len`.

The hand-off can be triggered when a Bitcoin transaction that sends the peg-balance to the shared threshold address is confirmed. This Bitcoin transaction must have all of its outputs pointing to the next treshold wallet scriptpubkey. It is not a hard requirement that all of the outputs aren't required to point to the shared treshold address, in this implementation of the contract, it is (open to other implementations). The value of all the outputs must be greater than or equal to the peg balance for the hand-off function to pass.

The transaction being confirmed must be submitted in the segwit-encoded format even if its inputs do not contain segwit inputs (open to other implementations).

# Incentives

The signers are incentivized to confirm that the hand-off process has been fulfilled or else penalties will start to be applied. We're assuming happy path in the DR so these penalties are not applied.