

The contract implements the sip-010-trait-ft-standard.sip-010-trait trait, ensuring that it complies with the standard interface for fungible tokens defined by SIP-010.

# Constants and variables

The Fungible tokens `sbtc-token` and `sbtc-token-locked` are defined.

Data variables for `token-name`, `token-symbol`, and `token-uri` are defined with initial values `"sBTC Mini,"` `"sBTC,"` and `none`, respectively.

The constant for `token-decimals` is defined as 8.

# SIP-010 compliant Functions

Compliant with the SIP-010 standard, the contract provides public functions for typical token interactions.

`transfer`
`get-name`
`get-symbol`
`get-decimals`
`get-balance`
`get-balance-locked`
`get-total-supply`
`get-token-uri`

These read-only functions provide information about the token, including name, symbol, decimals, balance, locked balance, total supply, and token URI.

# Protocol-level Functions

These functions are designed to be called by the sbtc-protocol contracts:

`protocol-transfer`
`protocol-lock`
`protocol-unlock`
`protocol-mint`
`protocol-burn`
`protocol-burn-locked`
`protocol-set-name`
`protocol-set-symbol`
`protocol-set-token-uri`
`protocol-mint-many`

The functions include various operations related to the management of the tokens, such as transfer, lock, unlock, mint, and burn.

# Locking token functions

The contract provides mechanisms to lock and unlock tokens via

`protocol-lock` and `protocol-unlock`.

The locked tokens are managed using a separate fungible token definition, `sbtc-token-locked`.
