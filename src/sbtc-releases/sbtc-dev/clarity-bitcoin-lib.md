The Clarity Bitcoin library contract does not hold state. It is useful for parsing transactions in the segwit and non-segwit format. It also has other auxiliary functions to get the transaction ID of a raw transaction in hex format and reversing buffers.

# Helper Functions

The Clarity Bitcoin library has some helper functions to read buffers.

`read-uint8`

`read-uint16`

`read-uint32`

`read-uint64`

`read-varint`

`read-varslice`

`reverse-buff32`

`read-hashslice`

# Bitcoin Transaction Helper Functions

# Functions

## This set of functions are useful for parsing the different sections of a Bitcoin transaction.

read-txins(ctx { txbuff, index }): Reads all transaction inputs from the buffer.

read-txouts(ctx { txbuff, index }): Reads all transaction outputs from the buffer.

read-witnesses(ctx { txbuff, index }, num-txins uint): Reads all witness data for the specified number of transaction inputs.

has-witness-data(tx (buff 4096)): Checks if the transaction has witness data.

parse-wtx(tx (buff 4096)): Parses a transaction containing witness data.

get-bc-h-hash(bh uint): Retrieves the burnchain header hash for a given block height. If in debug mode, it uses the mock map.

# Verification and Utility Functions

## Functions used to verify the bitcoin block header and the inclusion of a Bitcoin transaction in a merkle proof

verify-block-header(headerbuff (buff 80), expected-block-height uint): Verifies a block header by matching the computed hash against the expected hash.

get-reversed-txid(tx (buff 4096)): Gets the reversed transaction ID for a given transaction.

get-txid(tx (buff 4096)): Gets the transaction ID (as seen on block explorers).

is-bit-set(val uint, bit uint): Checks if the ith bit in a uint is set to 1.

verify-merkle-proof(reversed-txid (buff 32), merkle-root (buff 32), proof { tx-index: uint, hashes: (list 14 (buff 32)), tree-depth: uint }): Verifies a Merkle proof against a given Merkle root.

get-commitment-scriptPubKey(outs (list 8 { value: uint, scriptPubKey: (buff 128) })): Filters from a list of outputs that do not have the value of the scriptPubKey parameter.

is-commitment-pattern(scriptPubKey (buff 128)): Checks if the given scriptPubKey has the witness commitment prefix.

was-tx-mined-compact(height uint, tx (buff 4096), header (buff 80), proof { tx-index: uint, hashes: (list 14 (buff 32)), tree-depth: uint }): Verifies if a transaction was mined in a compact representation.

was-tx-mined-internal(height uint, tx (buff 4096), header (buff 80), merkle-root (buff 32), proof { tx-index: uint, hashes: (list 14 (buff 32)), tree-depth: uint }): Internal function to verify if a transaction was mined based on various criteria.

was-segwit-tx-mined-compact(burn-height uint, tx (buff 4096), header (buff 80), tx-index uint, tree-depth uint, wproof (list 14 (buff 32)), witness-merkle-root (buff 32), witness-reserved-value (buff 32), ctx (buff 4096), cproof (list 14 (buff 32))): Verifies if a SegWit transaction was mined by proving that it is included in the merkle proof in a coinbase transaction.
