# Simple Flow

## Local environment
Signer uses wallet_1, wallet_2, wallet_3 from the settings.toml file.

sBTC User uses wallet_9.

1. In cycle N registration window (block #140), signer calls sbtc-stacking-pool.signer-pre-register with wallet_1 btc address as reward address.
2. In cycle N + 1 registration window (block #150), signer calls sbtc-stacking-pool.signer-register with wallet_2 btc address as reward address and wallet_3 public key for sBTC wallet.
3. In cycle N + 1 voting window (block #156), signer calls sbtc-stacking-pool.vote-for-threshold-wallet-candidate for the wallet_3 btc address.
4. User sends deposit tx (using sbtc-cli) from wallet_9 to ???