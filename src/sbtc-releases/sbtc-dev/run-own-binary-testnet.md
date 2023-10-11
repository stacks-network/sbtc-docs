# Run Own Version sBTC DR 0.1 on Testnet

The sBTC DR 0.1 contains all the tools to deploy your own token and run the binary that manages the supply of the token through bitcoin transactions. To be your own centralized authority (CA) for the token, follow the steps below.

## Requirements
* cargo installed
* clarinet installed
* sbtc installed
* some testnet STX and testnet BTC

## CA Accounts
Generated a new seed phrase using

```
sbtc generate-from -s testnet -b testnet new
```

The stx address is used to deploy the contracts. The p2tr address is used as sBTC wallet.

Fund the stx address with testnet tokens.


## Deploy Contracts

In folder `romeo/asset-contract/settings` create (or update) `Testnet.toml` with the seed phrase:

```
[network]
name = "testnet"
node_rpc_address = "http://api.testnet.hiro.so"

[accounts.deployer]
mnemonic = "<your seed phrase>"
```

Generate deployment script for the seed phrase using clarinet in folder `romeo/asset-contract`:
```
clarinet deployments generate --testnet --medium-cost
```

Edit the generated file `deployments/default.testnet-plan.yaml` to remove `_test` contracts and to use the correct deployable contract with name `clarity-bitcoin-mini` using path `contracts/clarity-bitcoin-mini-deploy.clar`. The file should look like this with your stx address as expected-sender:

```
---
id: 0
name: Testnet deployment
network: testnet
stacks-node: "https://api.testnet.hiro.so"
bitcoin-node: "http://blockstack:blockstacksystem@bitcoind.testnet.stacks.co:18332"
plan:
  batches:
    - id: 0
      transactions:
        - contract-publish:
            contract-name: clarity-bitcoin-mini
            expected-sender: ST1ZA396YZ6T06JRCVKGQ115QFHC9TQNR1C32201C
            cost: 52950
            path: contracts/clarity-bitcoin-mini-deploy.clar
            anchor-block-only: true
            clarity-version: 2
        - contract-publish:
            contract-name: asset
            expected-sender: ST1ZA396YZ6T06JRCVKGQ115QFHC9TQNR1C32201C
            cost: 43410
            path: contracts/asset.clar
            anchor-block-only: true
            clarity-version: 2

```

## Start sBTC Binary
The sBTC Binary (Alpha Romeo engine) takes your seed phrase as configuration parameter.
Create a config file `config.json`

Create configuration file `config.json` in a new folder `romeo/testing` with the following content:

```json
{
  "stacks_network": "testnet",
  "bitcoin_network": "testnet",
  "state_directory": "./state",
  "mnemonic": "<your seed phrase>",
  "stacks_node_url": "https://api.testnet.hiro.so",
  "bitcoin_node_url": "http://blockstack:blockstacksystem@bitcoind.testnet.stacks.co:18332",
  "electrum_node_url": "ssl://blockstream.info:993",
  "contract_name": "asset"
}
```

Then start the binary using the following command:

```
cargo run -p romeo -- -c config.json
```

It will start outputing all events. The events are storted in `state/log.ndjson` and are used for replay when re-starting the engine.

## Launch Bridge Web App

Your token can be used by the Bridge Web App. It allows to deposit and withdraw bitcoin from your sBTC wallet.

For details how to configure the web app and its backend, please see READMEs in (github.com/stacks-network/sbtc-bridge-web)[https://github.com/stacks-network/sbtc-bridge-web] and (github.com/stacks-network/sbtc-bridge-api)[https://github.com/stacks-network/sbtc-bridge-api].


