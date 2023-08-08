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
- Amount: The amount to withdraw, as an 8-byte big-endian unsigned integer.
- Signature: A 65 byte recoverable ECDSA signature authenticating the request.

The withdrawal request is required to have two additional outputs beyond the data output. The second output is a dust amount to the recipient address. The third output is a fee subsidy to the sBTC wallet to fund the fulfillment of the withdrawal.

### Signature

The recoverable ECDSA signature in the withdrawal request ensures the authenticity and integrity of the transaction. Given that we use the secp256k1 elliptic curve (same as Bitcoin), this signature is composed of 65 bytes.

#### Message Format:

Before diving into the signature's byte-level structure, let's clarify the format of the message being signed.

The signature attests to a withdrawal request from a specific stacks principal. The signed message encapsulates:

1. **Amount (8 bytes)**: The total amount to withdraw, encoded as an 8-byte big-endian unsigned integer.
2. **Recipient Address (`n` bytes)**: This represents the intended beneficiary of the withdrawal. It's encoded as a Bitcoin script. The length of this script, denoted by `n`, can vary depending on the address format and type.

These components are concatenated in the given order, resulting in a byte array with a total length of `n+8`:

```
0      8                     n+8
|------|----------------------|
 amount        recipient
```

The actual message that gets signed is not this byte array directly but its SHA-256 hash, producing a consistent 32-byte digest.

#### Signature Structure:

Now, with the hashed message ready, the structure of the recoverable ECDSA signature is as follows:

```
0   1      33      65
|---|------|-------|
  v    r       s
```

- **v (1 byte)**: The recovery ID, a value between 0 and 3. It's a critical component that facilitates the public key's recovery during verification, eliminating the need for its explicit provision.

- **r (32 bytes)**: A value linked to the x-coordinate of a point on the elliptic curve. It's a pivotal element in ECDSA's verification mechanism, effectively serving as proof of the signer's private key ownership.

- **s (32 bytes)**: This, combined with `r`, constitutes the essence of the ECDSA signature. Together, they confirm the validity and authenticity of the message.

For the withdrawal request to be deemed valid, the signature must match the expected data.

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
