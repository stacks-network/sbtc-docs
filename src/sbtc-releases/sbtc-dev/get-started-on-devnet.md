# Get Started with sBTC 0.1 on devnet

## Local setup
Devnet is a setup with a local bitcoin node running regtest, a local stacks node running testnet and connecting to the local bitcoin node. In addition, explorers and api servers are setup to provide easy access to information.

### Requirements
* install [clarinet](https://github.com/hirosystems/clarinet)
* checkout main branch of repository [github.com/stacks-network/sbtc](https://github.com/stacks-network/sbtc)

### Configuration
1. Create configuration file `config.json` in a new folder `romeo/testing` with the following content:
```
{
  "stacks_network": "testnet",
  "bitcoin_network": "regtest",
  "state_directory": "./state",
  "mnemonic": "YOUR_MNEMONIC_PHRASES",
  "bitcoin_node_url": "http://devnet:devnet@localhost:8001/api",
  "stacks_node_url": "http://localhost:3999",
  "contract_name": "sbtc-alpha-romeo-testing"
}
```

**state_directory**
The folder that should contain the state of the Romeo engine binary relative to the folder `testing`.

When running the binary the first time,
an empty file `log.ndjson` is created in the state directory (e.g. `testing/state`). The file stores the processed transaction events.

**Node configurations**
The url and the fees used for transactions the bitcoin node and the stacks node can be configured with `stacks_network`, `bitcoin_network`, `bitcoin_node_url`, `stacks_node_url`.

**contract_name**
The contract name that is used when the contract is deployed. This allows to test the bootstrapping several times on the same network.

### Launch devnet

Open up your terminal and run the following commands.

```
cd devenv
./up.sh
```

See `romeo/asset-contract/settings/Devnet.toml` for seed phrases for pre-filled accounts.

### Fund your sBTC wallet address
In order to deploy the sBTC contract you must first fund your STX address listed above in the `config.json` file.

Download a wallet client:

  - [Leather Wallet Browser Extension](https://leather.io/install-extension)
  - [Leather Wallet Desktop Client](https://github.com/leather-wallet/desktop/releases)

If you wish to use the desktop client, you *MUST* download the testnet version of the executable.

You can use any one of the wallet mnemonic phrases in the `romeo/asset-contract/settings/Devnet.toml` file for the client.

Now send 100 STX to your STX address you generated with the `sbtc-cli` above.

### Launch sBTC DR binary
While waiting for `clarinet integrate` to finish setting up your devnet, open up a separate terminal window and start the sBTC binary from the root of the git repo.

```
cargo run -p romeo -- -c romeo/testing/config.json
```
