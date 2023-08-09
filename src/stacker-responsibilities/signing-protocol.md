# Signing protocol
We just established that Stackers must collectively fulfill a set of duties in order to receive their BTC rewards.
However, we have not yet explained how they can achieve this.
This chapter introduces the signing protocol which allows a set of signers (e.g. the Stackers) to generate a single BTC address and sign transactions from this address.

## Schnorr signatures and FROST
Something something schnorr... High level. Details can be in a later chapter.

## Signer messages
To participate in FIRE/ROAST/FROST, the signers exchange the following messages:

... outline the different messages https://github.com/stacks-network/stacks-blockchain/discussions/3841#discussioncomment-6673051

## Signer communication and StackerDB
Explain how signers exchange messages with each other

## Signer state
For a signer to operate it must be able to:

- Determine the complete set of signers and their public keys
- Observe and validate incoming sBTC requests

To be able to do this, the signer must keep track of

- sBTC balances in Stacks
- BTC transactions

The following example illustrates how a signer could implement its internal state.

```rust
struct SignerState {
  ...
}
```

## Helpful Stacks node RPC methods
These RPC methods exposed on Stacks nodes provide primitives which the signers can use to build and maintain its internal state.

* ...
* ...

## Signer leader election

