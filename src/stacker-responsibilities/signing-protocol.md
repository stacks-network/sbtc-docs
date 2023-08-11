# Signing protocol
We just established that Stackers must collectively fulfill a set of duties in order to receive their BTC rewards.
However, we have not yet explained how they can achieve this.
This chapter introduces the signing protocol which allows a set of signers (e.g. the Stackers) to generate a single BTC address and sign transactions from this address.

## Signature aggregation and FROST
Let's begin by understanding the cryptographic algorithm that enables trustless collaboration between the signers.

Flexible Round Optimized Schnorr Threshold (FROST) is a cryptographic algorithm designed to enhance security and efficiency in digital signature schemes. It combines the benefits of the Schnorr signature algorithm with the concept of threshold cryptography. It ensures N parties, each with their own private key, can only generate a valid Schnorr signature when a certain number (threshold) of participants collaborate honestly.  

The sBTC protocol requires signers to use FROST to generate signatures for the deposit and withdrawal transactions. This approach offers several advantages:

**Security**: As a deposit or withdrawal transaction must be validated and signed by multiple independent signers, the risk of a single malicious signer causing harm to the system is reduced.  
**Trust Distribution**: The trust required to process transactions is distributed among multiple signers, making the system more resilient to potential attacks of malicious behaviour.  
**Efficiency**: FROST signatures can optimize communication rounds, making the signing process efficient even with multiple signers.

## Signer setup
To run a signer, it 

## Signer messages
To participate in FIRE/ROAST/FROST, the signers exchange the following messages:

... outline the different messages https://github.com/stacks-network/stacks-blockchain/discussions/3841#discussioncomment-6673051

## Signer communication and StackerDB
Explain how signers exchange messages with each other and use StackerDB.

## Signer state
For a signer to operate it must be able to:

- Determine the complete set of signers and their public keys
- Observe and validate incoming sBTC requests

To be able to do this, the signer must keep track of

- sBTC balances in Stacks
- BTC transactions

The following pseudocode illustrates the information a Signer should keep track of.

```rust
struct State {
  rounds: Vec<SigningRound>
}

struct SigningRound{
  round_id: RoundID,
  signers: Map<ID, Signer>,
  sbtc_operations: Vec<SbtcOperation>
}

struct Signer {
  id: ID,
  public_key: PublicKey,
}


struct SbtcOperation {
  request: SbtcRequest,
  responses: Vec<SbtcResponse>,
}

enum SbtcRequest{
  Fulfillment(...),
  Reveal(...),
  Handoff(...),
}

enum SbtcResponse{
  Fulfillment(...),
  Reveal(...),
  Handoff(...),
}

struct FulfillmentRequest {
  source: StacksTransaction,
  sender: StacksPrincipal,
  receiver: BitcoinAddress,
  amount: u64,
  created_at: BlockHeight
}

struct FulfillmentResponse {
  source: StacksTransaction,
  fulfillment: BitcoinTransaction,
  status: FulfillmentStatus,
}

enum FulfillmentStatus(
  Pending,  
  Mined,
  Cancelled,
)

struct RevealRequest {
  commit_output: BitcoinTxOut,
  control_block: ControlBlock,
  internal_key: TaprootInternalKey,
  script: TapScript,
}

struct RevealResponse {
  tx: BitcoinTransaction
}

struct HandoffRequest {
  destination: BitcoinAddress,
  amount: u64,
}

struct HandoffResponse {
  tx: BitcoinTransaction
}

```

## Helpful Stacks node RPC methods
These RPC methods exposed on Stacks nodes provide primitives which the signers can use to build and maintain its internal state.

* ...
* ...

## Signer leader election

