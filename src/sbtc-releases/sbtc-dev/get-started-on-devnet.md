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

See [sbtc/devenv](https://github.com/stacks-network/sbtc/blob/main/devenv/README.md) for details.

Check status of all services:
* http://localhost:3002 (Bitcoin explorer)
* http://localhost:3000/?chain=testnet&api=http://localhost:3999 (Stacks explorer)
* http://localhost:3999/v2/info (Stacks Api)

See `romeo/asset-contract/settings/Devnet.toml` for seed phrases for pre-filled accounts.

### Deploy Contract
The local environment defines wallets that are already funded. The deployer wallet is used to deploy the contract.

Change the folder to `romeo/asset-contract` and run the following command

```
cd romeo/asset-contract
clarinet deployments apply -p deployments/default.devnet-plan.yaml
```

### Launch sBTC DR binary
While waiting for `clarinet integrate` to finish setting up your devnet, open up a separate terminal window and start the sBTC binary from the root of the git repo.

```
cargo run -p romeo -- -c romeo/testing/config.json
```

### Use sBTC App
Download the wallet client:

  - [Leather Wallet Browser Extension](https://leather.io/install-extension)