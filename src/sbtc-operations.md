# sBTC Requests and Responses
Requests to the sBTC system happen on the Bitcoin blockchain. In this chapter, we explain the two requests that users can create. We go over all information they must contain, and how the sBTC protocol responds to the requests.

## Deposit Request
When a user wishes to deposit BTC in favor of receiving sBTC, they create a deposit request transaction. This is a bitcoin transaction sending the requested deposit amount of BTC to the address provided by the Stackers. In addition, the transaction must also specify to which Stacks address the sBTC should be minted.

The sBTC deposit request transaction will therefore contain the following data:

* Recipient address: The Stacks address which should receive the sBTC.
* sBTC wallet address: The Bitcoin address maintaining custody of the deposited BTC.
* Amount: The amount to deposit.

The exact format to submit this data is explained in [Wire Formats](./sbtc-operations/wire-formats.md).

TODO: Figure

## Withdrawal Request
An sBTC withdraw request is a bitcoin transaction sending a dust amount to the BTC address.

To withdraw sBTC is slightly more complex than to deposit. The Withdrawal request is also a bitcoin transaction


TODO: [#8](https://github.com/stacks-network/sbtc-docs/issues/8)
