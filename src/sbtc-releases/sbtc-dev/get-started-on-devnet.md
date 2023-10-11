# Get Started with sBTC 0.1 on devnet

## Local setup

Devnet is a setup with a local bitcoin node running regtest, a local stacks node running testnet and connecting to the local bitcoin node. In addition, explorers and api servers are setup to provide easy access to information.

Developers should use docker to setup all required services.

Here are the basic steps we'll need to follow to get everything running.

1. Launch devnet.
2. Deploy sbtc contract.
3. Launch sBTC binary (Alpha romeo engine).
4. Use sBTC web app (sBTC Bridge) or sbtc cli.

### Requirements

- Install [Docker](https://docs.docker.com/engine/install/).
- Install [Clarinet](https://github.com/hirosystems/clarinet).

### Configuration

The default values are set as follows using the same seed phase defined in `romeo/asset-contract/settings/Devnet.toml`:

| field        | value                                                                                                                                                | explanation                                             |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| seed phrase  | twice kind fence tip hidden tilt action fragile skin nothing glory cousin green tomorrow spring wrist shed math olympic multiply hip blue scout claw | main/deployer wallet, same as in get_credentials script |
| peg wallet   | bcrt1pte5zmd7qzj4hdu45lh9mmdm0nwq3z35pwnxmzkwld6y0a8g83nnqhj6vc0                                                                                     | taproot address from seed phrase                        |
| stx deployer | ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM                                                                                                            | aka wallet.deployer, uses account index 0               |
| btc deployer | tb1q3tj2fr9scwmcw3rq5m6jslva65f2rqjxt2t0zh                                                                                                           | p2wpkh using account index 0                            |
| stx Alice    | ST2ST2H80NP5C9SPR4ENJ1Z9CDM9PKAJVPYWPQZ50                                                                                                            | uses account index 1                                    |
| btc Alice    | tb1q3zl64vadtuh3vnsuhdgv6pm93n82ye8qc36c07                                                                                                           | p2wkh address using account index 1                     |

Create configuration file `config.json` in a new folder `romeo/testing` with the following content:

```json
{
  "stacks_network": "testnet",
  "bitcoin_network": "regtest",
  "state_directory": "./state",
  "mnemonic": "twice kind fence tip hidden tilt action fragile skin nothing glory cousin green tomorrow spring wrist shed math olympic multiply hip blue scout claw",
  "stacks_node_url": "http://localhost:3999",
  "bitcoin_node_url": "http://devnet:devnet@localhost:18443",
  "electrum_node_url": "tcp://localhost:60401",
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

- Install [Docker](https://docs.docker.com/engine/install/).
- Install [Clarinet](https://github.com/hirosystems/clarinet).
- Checkout main branch of the sBTC repository [github.com/stacks-network/sbtc](https://github.com/stacks-network/sbtc).

### Launch devnet

Now we need to get sBTC up and running locally using the [sBTC devenv](https://github.com/stacks-network/sbtc/blob/main/devenv/README.md). You'll need to make sure you have Docker and Docker Compose installed on your system.

Check status of all services:

- (Mempool web) [http://localhost:8083](http://localhost:8083)
- (Stacks explorer) [http://localhost:3000/?chain=testnet&api=http://localhost:3999](http://localhost:3000/?chain=testnet&api=http://localhost:3999)
- (Stacks Api) [http://localhost:3999/v2/info](http://localhost:3999/v2/info)

This will serve as the contract owner and will call our sBTC operations on the Stacks side.

First, install the sBTC CLI with

```
cargo install --path sbtc-cli
```

Make sure you are in the `devenv` folder. Now we can use that to generate a new private key with:

```bash
./sbtc/bin/sbtc generate-from -s testnet new
```

Change the "mnemonic" field in the `sbtc/devenv/sbtc/docker/config.json` file to the one generated by the CLI. You also need to make sure you change the `contract_name` field to `asset`.

There are some utility functions built into `devenv` that will allow us to mint some sBTC and BTC, but we need the Stacks address to be set to the same one we are using in our Leather wallet (which we'll get set up with below).

Now let's get everything running. Make sure you are in the `devenv` folder and run:

```bash
./build.sh
```

This first build will take a while to complete, but once that's done we just need to run everything with:

```bash
./up.sh
```

Let's check to make sure things are running by visiting the Stacks explorer and the Bridge app. The Stacks explorer will take a bit get up and running.

- http://localhost:3000/?chain=testnet&api=http://localhost:3999 (Stacks explorer)
- http://127.0.0.1:8080/ (sBTC Bridge App)

### Deploy Contract

The local environment defines wallets that are already funded. The deployer wallet is used to deploy the contract using clarinet deployments.

Download the [Leather wallet](https://leather.io) and import the deployer address mnemonic (`twice kind fence tip hidden tilt action fragile skin nothing glory cousin green tomorrow spring wrist shed math olympic multiply hip blue scout claw`) into it to load that account locally.

You can see the default generated mnemonics for the devnet in the [`devenv` repo](https://github.com/stacks-network/sbtc/devenv).

### Fund the sBTC contract owner

Before we can deploy our contracts, we need to fund the newly generated address. You can do that with the Leather wallet, just make sure you are on DevNet.

Once you've funded the sBTC contract owner address, run the following command from within the `devenv` folder to deploy the Clarity contracts `asset.clar` and `clarinet-bitcoin-mini.clar`.

```
./utils/deploy_contracts.sh
```

This will deploy the contracts and set the owner to the address we generated previously.

### Mint some BTC and sBTC

There are helper scripts to create and broadcast deposit and withdrawal requests. They are contained in the `utils` folder of devenv:

1. mint_btc.sh: funds the btc addresses with BTC. Add the number of blocks to mint for the two addresses as parameter.
2. deposit.sh: deposits a random amount of less than 10,000 sats. sBTC will be received by the deployer stacks address.
3. withdrawal.sh: withdraws a random amount of less than 2,000 sats.

Before running these, make sure the Stacks devnet is running by visiting the explorer at http://127.0.0.1:3000/transactions?chain=testnet&api=http://localhost:3999.

If you get an error, wait a few minutes and refresh. Once that page loads correctly you should be good to go.

First, let's mine some BTC with `./utils/mine_btc.sh 200`. This will mine 200 BTC blocks and ensure our address is funded.

Next we can initiate a deposit, which will deposit a random amount of satoshis from our Bitcoin wallet into the sBTC threshold wallet, minting sBTC in the process.

We can do that with `./utils/deposit.sh`.

And finally, we can do the reverse, convert our sBTC back to BTC, with `./utils/withdraw.sh`.

Check the results on Stacks at our address:
http://localhost:3000/address/ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM?chain=testnet&api=http://localhost:3999

Check the results on Bitcoin at the txs printed by the utility functions, eg. https://127.0.0.1:3002/tx/38089273be6ed7521261c3a3a7d1bd36bc67d97123c27263e880350fc519899d, replacing the txid parameter with the given tx id.

## Next Steps

Congratulations! You are among the first people in the world to run sBTC. Now you're ready to actually build something. For that, head over to the [Developer Quickstart](../../quickstart.md) and we'll build a simple full-stack Bitcoin lending application powered by sBTC.
