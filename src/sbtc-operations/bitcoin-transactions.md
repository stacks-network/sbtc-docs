# Bitcoin Transactions
This page outlines all transactions which happen on the Bitcoin blockchain within the sBTC protocol, and how they are represented on the bitcoin chain.

# Data and opcodes
Common to all sBTC transactions on Bitcoin is that they need to embed data on the Blockchain. To enable this, we require all sBTC transactions to have their first output be a script of the form
```
OP_RETURN < magic bytes | opcode | data >
```
where `magic bytes` are special bytes required by the Stacks blockchain, the `opcode` identifies the type of the sBTC transaction, and the `data` field is specific to the transaction type. We use `|` as the concatenation operator to denote that all information is pushed as a single byte slice to the bitcoin script.

The magic bytes are

- `X2` for Stacks mainnet
- `T2` for Stacks testnet

The opcodes for sBTC are

- `<`: Deposit request
- `>`: Withdrawal request
- `!`: Withdrawal fulfillment
- `H`: sBTC wallet handoff

The following sections will go throuch each of these transactions and outline what data and outputs they require.

## Deposit request
The deposit request contains the following data (incl. opcode and magic byte) in its first output
```
0       2  3                   66
|-------|--|-------------------|
| magic |op| Recipient address |
```

- Magic: `X2` or `T2`
- Opcode: `<`
- Recipient address: Either a standard or a contract principal encoded as a Clarity value as defined in [SIP-005](https://github.com/stacksgov/sips/blob/main/sips/sip-005/sip-005-blocks-and-transactions.md#clarity-value-representation).

The deposit transaction also has a second output which sends the requested amount to the sBTC wallet address.

TODO: Figure

## Withdrawal request
The withdrawal request contains the following data (incl. opcode and magic byte) in its first output
```
0      2  3          11                76
|------|--|----------|-----------------|
 magic  op   amount       signature
```

- Magic: `X2` or `T2`
- Opcode: `>`
- Amount: The amount to withdraw.
- Signature: A 65 byte recoverable ECDSA signature authenticating the request.

The withdrawal request is required to have two additional outputs beyond the data output. The second output is a dust amount to the recipient address. The third output is a fee subsidy to the sBTC wallet to fund the fulfillment of the withdrawal.

TODO: Figure

### Signature format
TODO: [#18](https://github.com/stacks-network/sbtc-docs/issues/18)

## Withdrawal fulfillment
The withdrawal request contains the following data (incl. opcode and magic byte) in its first output
```
0      2  3                     35
|------|--|---------------------|
 magic  op       Chain tip
```

- Magic: `X2` or `T2`
- Opcode: `!`
- Chain tip: The stacks chain tip used to vaildate the withdrawal.

The withdrawal fulfillment has a second output which sends the requested amount to the recipient address.
Finally, the withdrawal fulfillment links back to the withdrawal request transaction by consuming the fee subsidy output of the withdrawal request as its first input.

TODO: Figure

## sBTC wallet handoff
The sBTC wallet handoff contains the following data (incl. opcode and magic byte) in its first output
```
0      2  3                     11
|------|--|---------------------|
 magic  op     Reward cycle
```

- Magic: `X2` or `T2`
- Opcode: `H`
- Reward cycle: The reward cycle number of the Stacker set handing over the sBTC wallet.

The second output sends an amount greater than or equal to the total amount of sBTC in circulation to the sBTC wallet of the next reward cycle.

TODO: Figure

