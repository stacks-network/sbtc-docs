# Signer Configuration

In this guide, we'll explain how to create a TOML configuration file to configure a signer binary. The signer binary is responsible for approving or denying transactions based on certain criteria. We'll cover how to set up a TOML file with all required configuration options as well as some optional validation configuration options.

## Mandatory Options

The following options all govern signer operation in terms of communication and transaction monitoring, construction, and broadcasting. All of these options are mandatory and must be included in the .toml file for a signer binary to operate.  

- 'private_key': the private key of the signer, used for signing sBTC transactions.  
- 'stacks_node_rpc_url': the stacks node running the stackerDB instance which is used for signer communication and transaction monitoring and broadcasting.  
- 'bitcoin_node_rpc_url': the bitcoin node used for transaction monitoring and broadcasting.  
- 'revealer_url': the revealer server used to monitor for incoming deposit transactions.  

## Discretionary Options

A majority of the following options involve validation logic for incoming deposit and withdrawal requests. All of these options are discretionary and may be omitted from the .toml file.

- 'network': one of 'Mainnet', 'Testnet', or 'Devnet'. Defaults to 'Testnet'.  
- 'signer_api_server_url': the url at which to host the signer api server for transaction monitoring. Defaults to "http://localhost:3000".  
- 'auto_deny_block': the number of blocks before signing deadline to auto deny a transaction waiting for manual review. Defaults to 10.  
- 'auto_approve_max_amount': the maximum dollar amount of a transactions that will be auto approved  
- 'auto_deny_addresses_btc': a list of bitcoin addresses that should trigger an auto deny  
- 'auto_deny_addresses_stx': a list of stx addresses that should trigger an auto approve  

## Example TOML File
Below is an example configuration file that might be used by the signer binary.  

```toml
# config.toml

# Mandatory fields

# Note: Replace 'MY_PRIVATE_KEY' with the actual private key value
signer_private_key = "MY_PRIVATE_KEY"
stacks_node_rpc_url = "http://localhost:9776"
bitcoin_node_rpc_url = "http://localhost:9777"
revealer_url = "http://localhost:9778"

# Optional fields

auto_approve_max_amount = 500000
network = "Devnet"
# Note: replace "BTC_ADDRESS_*" with actual BTC addresses you wish to deny
auto_deny_addresses_btc = [
    "BTC_ADDRESS_1",
    "BTC_ADDRESS_2"
]
# Note: replace "STX_ADDRESS" with an actual STX address you wish to deny
auto_deny_addresses_stx = [
    "STX_ADDRESS"
]
```

Remember to replace the placeholder values with your actual configuration values.

Now that you have your TOML configuration file set up, your signer binary can load this configuration at runtime and use the specified settings for its operation and transaction validation logic.