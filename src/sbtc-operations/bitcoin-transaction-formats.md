# Bitcoin Transaction Formats
This page outlines all transactions which happen on the Bitcoin blockchain within the sBTC protocol, and how they are represented on the bitcoin chain.

# Data and opcodes
Common to all sBTC transactions on Bitcoin is that they need to embed data on the Blockchain. To enable this, we require all sBTC transactions to have their first output be a script of the form
```
OP_RETURN <opcode> <data>
```
where the `opcode` identifies the type of the sBTC transaction, and the `data` field is specific to the transaction type. The opcodes for sBTC are

- `<`: Deposit request
- `>`: Withdrawal request
- `!`: Withdrawal fulfillment
- `H`: sBTC wallet handoff
- `w`: Commit-reveal

## The commit reveal format
