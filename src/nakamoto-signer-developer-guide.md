# What is this

Draft documentation for sBTC Signer specifically for the Nakamoto [release](https://stacks-network.github.io/sbtc-docs/sbtc-roadmap.html)

The readers of this are assumed to be anyone who is looking to run a sBTC Signer with minimal custom configuration.

# Disclaimer

Development is on going and changes to the Signer configuration should be expected. This document will be updated every sprint to ensure its accuracy.

## Prerequisites

Rust - [To install](https://www.rust-lang.org/tools/install).

Accessible Stacks node - A Stacks node running the stackerDB instance which is used for signer communication and transaction monitoring and broadcasting.

Accessible Bitcoin node - A Bitcoin node used for transaction monitoring and broadcasting.

A Stacks Address - Identify your address as a Signer

## Installing

### Building from Source

If you wish to compile the default binary from source, follow the steps outlined below. Otherwise, download the binary directly (below)

1. First, clone the Stacks sBTC mono repository:

```console
git clone git@github.com:stacks-network/stacks-blockchain.git
```

2. Next, navigate to the stacks-signer directory:

```console
cd stacks-blockchain/stacks-signer
```

3. Checkout the appropriate release branch you wish to use if you are not using the default main branch

```console
git checkout main
```

4. Compile the signer binary:  
   Note the binary path defaults to `target/release/stacks-signer`.

```console
cargo build --release
```

### Downloading the Binary

1. First, download the precompiled default [TODO:NEED:LINK](LINK).

2. Untar the file

```console
tar -xvf signer_binary.tar
```

3.  Check Extracted Files:
    After running the untar command, the contents of the tar file should be extracted to the current directory. You should see the signer binary (stacks-signer-mini) and the configuration file (signer.toml) listed among the extracted files.

4.  Next, install the signer.

```console
cargo install --path stacks-signer
```

## Configuration

The signer takes a TOML config file with the following expected properties

| Key                         | Required | Description                                                                                                                                                  |
| --------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `signer_private_key`        | `true`   | Stacks private key of the signer, used for signing sBTC transactions.                                                                                        |
| `stacks_node_rpc_url`       | `true`   | Stacks node RPC URL that points to a node running the stackerDB instance which is used for signer communication and transaction monitoring and broadcasting. |
| `bitcoin_node_rpc_url`      | `true`   | Bitcoin node RPC URL used for transaction monitoring and broadcasting.                                                                                       |
| `revealer_rpc_url`          | `true`   | Revealer RRC URL                                                                                                                                             |
| `network`                   | `false`  | One of `['Signet', 'Regtest', 'Testnet', 'Bitcoin']`. Defaults to `Testnet`                                                                                  |
| `signer_api_server_url`     | `false`  | Url at which to host the signer api server for transaction monitoring. Defaults to "http://localhost:3000".                                                  |
| `auto_deny_block`           | `false`  | Number of blocks before signing deadline to auto deny a transaction waiting for manual review. Defaults to 10.                                               |
| `auto_approve_max_amount`   | `false`  | Maximum amount of a transactions that will be auto approved                                                                                                  |
| `auto_deny_addresses_btc`   | `false`  | List of bitcoin addresses that trigger an auto deny                                                                                                          |
| `auto_deny_addresses_stx`   | `false`  | List of stx addresses that trigger an auto deny                                                                                                              |
| `auto_deny_deadline_blocks` | `false`  | The number of blocks before deadline at which point the transaction will be auto denied. Default is 10 blocks.                                               |

### Example TOML file

```toml
# config.toml

# Mandatory fields

# Note: Replace 'MY_PRIVATE_KEY' with the actual private key value
signer_private_key = "MY_PRIVATE_KEY"
stacks_node_rpc_url = "http://localhost:9776"
bitcoin_node_rpc_url = "http://localhost:9777"
revealer_rpc_url = "http://locahost:9778"

# Optional fields
network = "Signet"
auto_approve_max_amount = 500000
auto_deny_addresses_btc = [
	"BTC_ADDRESS_1",
	"BTC_ADDRESS_2"
]
auto_deny_addresses_stx = [
	"STX_ADDRESS"
]
auto_deny_deadline_blocks = 120
```

## Running the binary

After installing and creating a config file to run the binary

```console
stacks-signer-mini --config conf/signer.toml
```

## Monitor Transactions

The signer binary operates a web server/client and it can be navigated to by default at http://localhost:3000/ (unless otherwise specified from config). Here you can see pending transactions and manually review and sign transactions that cannot be automatically signed on your behalf. Note that manual review is triggered based on the options you have set in your configuration file.
