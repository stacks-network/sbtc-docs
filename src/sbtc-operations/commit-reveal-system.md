# Commit-Reveal Transactions
In the previous section, we introduced special Bitcoin transactions that participants in the sBTC system must create. 

## Witness data reveal

If you have read the previous chapters you will recognize deposits, withdrawals and the sBTC wallet.
However, the final item in the opcode list "Witness data" has not yet been explained.

The "Witness data" transaction is a special type of meta-transaction in which the data and opcode for the transaction is embedded in the witness data of the first input of the transaction.
This way, a "Witness data" transaction enables anyone to commit to an sBTC transaction by paying to a single p2sh, p2wsh or p2tr address.

The witness script of the first input should have the following form.

```
< opcode | data | fee subsidy (8 bytes)> OP_DROP <lock script... >
```

Once the opcode and data of a "Wintess data" transaction is looked up, the transaction is treated as if it was of the transaction type associated with the given opcode in the witness.
In [The Commit-Reveal System](,/sbtc-operations-commit-reveal-system.md) we do a deep dive in this particular transaction type explaining when to use this format and what the `fee subsidy` is needed for.

