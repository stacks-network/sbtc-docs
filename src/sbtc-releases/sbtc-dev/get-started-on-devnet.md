# Get Started with sBTC 0.1 on devnet

## Local setup
Devnet is a setup with a local bitcoin node running regtest, a local stacks node running testnet and connecting to the local bitcoin node. In addition, explorers and api servers are setup to provide easy access to information.

Developers can use docker to setup all required services.

The following steps have to be done
1. Launch devnet.
2. Deploy sbtc contract.
3. Launch sBTC binary (Alpha romeo engine).
4. Use sBTC web app (sBTC Bridge) or sbtc cli.

### Requirements
* install [docker](https://docs.docker.com/engine/install/).
* install [clarinet](https://github.com/hirosystems/clarinet).
* checkout main branch of repository [github.com/stacks-network/sbtc](https://github.com/stacks-network/sbtc).

### Configuration
The default values are set as follows using the same seed phase defined in `romeo/asset-contract/settings/Devnet.toml`:

|----|----|---|
| seed phrase | twice kind fence tip hidden tilt action fragile skin nothing glory cousin green tomorrow spring wrist shed math olympic multiply hip blue scout claw | main/deployer wallet |
| peg wallet | bcrt1pte5zmd7qzj4hdu45lh9mmdm0nwq3z35pwnxmzkwld6y0a8g83nnqhj6vc0| taproot address from seed phrase |
| stx deployer | ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM | |


1. Create configuration file `config.json` in a new folder `romeo/testing` with the following content:
```
{
  "stacks_network": "testnet",
  "bitcoin_network": "regtest",
  "state_directory": "./state",
  "mnemonic": "twice kind fence tip hidden tilt action fragile skin nothing glory cousin green tomorrow spring wrist shed math olympic multiply hip blue scout claw",
  "bitcoin_node_url": "http://devnet:devnet@localhost:8001/api",
  "stacks_node_url": "http://localhost:3999",
  "electrum_node_url": "https://localhost:60401",
  "contract_name": "asset"
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

See [sbtc/devenv](https://github.com/stacks-network/sbtc/blob/main/devenv/README.md) for details.

Check status of all services:
* http://localhost:3002 (Bitcoin explorer)
* http://localhost:3000/?chain=testnet&api=http://localhost:3999 (Stacks explorer)
* http://localhost:3999/v2/info (Stacks Api)

See `romeo/asset-contract/settings/Devnet.toml` for seed phrases for pre-filled accounts.

### Deploy Contract
The local environment defines wallets that are already funded. The deployer wallet is used to deploy the contract using clarinet deployments.

Run the following command to deploy the Clarity contracts `asset.clar` and `clarinet-bitcoin-mini.clar`

```
./utils/deploy_contracts.sh
```

### Launch sBTC DR binary
While waiting for the contracts to be deployed, open up a separate terminal window and start the sBTC binary from the root of the git repo.

```
cargo run -p romeo -- -c romeo/testing/config.json
```

### Use the sBTC cli
There are helper scripts to create and broadcast deposit and withdrawal requests. They are contained in the `utils` folder of devenv:

1. mint_btc.sh: funds the btc addresses with BTC. Add the number of blocks to mint for the two addresses as parameter.
2. deposit.sh: deposits a random amount of less than 10,000 sats. sBTC will be received by the deployer stacks address.
3. withdrawal.sh: withdraws a random amount of less than 2,000 sats.
### Use sBTC App
Download the wallet client:

  - [Leather Wallet Browser Extension](https://leather.io/install-extension)