# Bitcoin Transactions
This page outlines all transactions which happen on the Bitcoin blockchain within the sBTC protocol, and how they are represented on the bitcoin chain.

# Data and opcodes
Common to all sBTC transactions on Bitcoin is that they need to embed data on the Blockchain. To enable this, we require all sBTC transactions to have their first output be a script of the form
```
OP_RETURN <opcode | data>
```
where the `opcode` identifies the type of the sBTC transaction, and the `data` field is specific to the transaction type. We use `|` as the concatenation operator. The opcodes for sBTC are

- `<`: Deposit request
- `>`: Withdrawal request
- `!`: Withdrawal fulfillment
- `H`: sBTC wallet handoff
- `w`: Witness data

## The Deposit request

## The commit reveal format

## Witness data

If you have read the previous chapters you will recognize Deposits, Withdrawals and the sBTC wallet.
However, the final item "Witness data" has not yet been explained.

The "Witness data" transaction is a special type of meta-transaction in which the data and opcode for the transaction is embedded in the witness data of the first input of the transaction.
This way, a "Witness data" transaction enables anyone to commit to an sBTC transaction by paying to a single p2sh, p2wsh or p2tr address.

```
<opcode | data | fee subsidy (8 bytes)> OP_DROP <lock script...>
```

Once the opcode and data of a "Wintess data" transaction is looked up, the transaction is treated as if it was of the transaction type associated with the given opcode in the witness.
In [The Commit-Reveal System](,/sbtc-operations-commit-reveal-system.md) we do a deep dive in this particular transaction type explaining when to use this format and what the `fee subsidy` is needed for.
