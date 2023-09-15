# Get Started with sBTC 0.1

## Testnet
Currently, testnet setup is still under development.

## Local setup
Developers can use docker to setup all required services.

The following steps have to be done
1. Launch devnet.
2. Deploy sbtc contract.
3. Launch sBTC binary (Alpha romeo engine).
4. Use sBTC web app (sBTC Bridge).

### Requirements
* install [docker](https://docs.docker.com/engine/install/).
* install [clarinet](https://github.com/hirosystems/clarinet).
* checkout main branch of repository [github.com/stacks-network/sbtc](https://github.com/stacks-network/sbtc)

### Configuration
The default values are set as follows using the same seed phase defined in `romeo/asset-contract/settings/Devnet.toml`:
|----|----|---|
| seed phrase | twice kind fence tip hidden tilt action fragile skin nothing glory cousin green tomorrow spring wrist shed math olympic multiply hip blue scout claw | main/deployer wallet |
| peg wallet | bcrt1pte5zmd7qzj4hdu45lh9mmdm0nwq3z35pwnxmzkwld6y0a8g83nnqhj6vc0| taproot address from seed phrase |
| stx deployer | ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM | |

1. Create configuration file `config.json` in a new folder `romeo/testing` with the following content:
```
{
  "network": "testnet",
  "state_directory": "./state",
  "mnemonic": "YOUR_MNEMONIC_PHRASES",
  "wif": "YOUR_WIF_CREDENTIALS",
  "bitcoin_node_url": "http://devnet:devnet@localhost:8001/api",
  "stacks_node_url": "http://localhost:3999",
  "stacks_transaction_fee": 2000,
  "bitcoin_transaction_fee": 2000,
  "contract": "../asset-contract/contracts/asset.clar",
  "contract_name": "sbtc-alpha-romeo-testing"
}
```

**state_directory**
The folder that should contain the state of the Romeo engine binary relative to the folder `testing`.

When running the binary the first time,
an empty file `log.ndjson` is created in the state directory (e.g. `testing/state`). The file stores the processed transaction events.

**wif**
The private key in [wallet import format](https://en.bitcoin.it/wiki/Wallet_import_format) used by the Romeo engine binary. It is used for bitcoin and stacks transaction submitted by the binary.

There is a tool specifically useful for sBTC testing. It's the `sbtc-cli` package in the sbtc repo, sibling to romeo. You can generate a new private key by using the following command:

```
sbtc generate-from new
```

The output looks like below and contains the wif.
```
{
  "credentials": {
    "0": {
      "bitcoin": {
        "p2pkh": {
          "address": "n44tpstz2NPRAz2MYhdCbwRHvj87jDW6nu",
          "private_key": "5a323c81001951770e728372484b90af6ab467ac4a7a1f2361f7c48b4e353cfc",
          "public_key": "0233ce5eccd746c90df6d54d33e4a5741f7703ae085df340e9da8ea1648bba066a",
          "wif": "cQc2mfkN2jnRReLj1oAXrk4YZ1GN7i5Ne5g2tbTdH4sp7XEgpgSd"
        },
        "p2tr": {
          "address": "tb1p83weqj54pkwxedwc2jctfhf2eyszuvsavad7qvk6ucffsc8u7vjsld0gxv",
          "private_key": "f5eedc5f82f44551ece4a80e9930037485cfb8bed05ceac8faefb4dbfa4941ae",
          "public_key": "024205c0c1cbcfe2c866d06a17735ac4bc893b02f6fb5284acccb9c55a5e6237ea",
          "wif": "cVpmEAwBvCWhhRxsaXwwfsnyCkWZYkQA4n74oSFXq3MRvxmvcBRt"
        },
        "p2wpkh": {
          "address": "tb1qrkt8ula8cr5w5rsa83cf63nvl2dhzu3wdjpxnj",
          "private_key": "6f964b2fe3bd066e0a0b51ca72cb57405a2f87734b21fcdbb8ee90cd0681e5e1",
          "public_key": "035b8967d295f1f6220e5dfc203fdfc78c796e1945b10f8f4ea9996c75c053873e",
          "wif": "cRKcUCoqUWbTs4iQeHUxvuSJNtBLhVL6MHWkavM44V84un2jxqz7"
        }
      },
      "stacks": {
        "address": "ST2EAW85EB3WZQCGT1PGKG2EQDXHYHH15SJWJECEG",
        "private_key": "dc4438704f7bc7bd6f08906ecf6e7e0ea3cac9a7fa019e2bb557ed3675863412",
        "public_key": "02d38e1c2cd1404ee1e1b15ae833f887f969e397cebdba55ea03ba74b12c501d54",
        "wif": "cUxsTywm1foSgpkAESPHLpdMLEmGKUFtgeA8NgBb3KymydhK5nV2"
      }
    }
  },
  "mnemonic": "tobacco box reject obtain gloom cream bean trouble clap seek remove giggle half zone neck region canoe say heart narrow segment garage wild until",
  "private_key": "6ef44d1e9db101b2fd02a8ad7aad15eb3b4515111f8cd75efcf177501630da72",
  "wif": "cRJP8KnNWHogXDTtCDyMSpZ9RG7xFQuHEXe9ZpM93DQizQf8PLxG"
}
```

**Node configurations**
The url and the fees used for transactions the bitcoin node and the stacks node can be configured with `bitcoin_node_url`, `stacks_node_url`, `bitcoin_transaction_fee` (in sats), `stacks_transaction_fee` (in micro stacks).

**contract**
The path to the source file of the clarity contract relative to the location of the Romeo engine binary.

**contract_name**
The contract name that is used when the contract is deployed. This allows to test the bootstrapping several times on the same network.


### Launch devnet

See [sbtc/devenv](https://github.com/stacks-network/sbtc/blob/41bb3a253f658f89e69cd3cb4f61893a23d62067/devenv/README.md) for details.

Check status of all services:

* http://localhost:3002 (Bitcoin explorer)
* http://localhost:3000/?chain=testnet&api=http://localhost:3999 (Stacks explorer)
* http://localhost:3999/v2/info (Stacks Api)

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
cargo run -p romeo -- -c config.json
```

Now head back to the first terminal window that has Clarinet's devnet running and press `N` to min a new block and see how the transaction is processed.

See `settings/Devnet.toml` for seed phrases for pre-filled accounts.

### Use sBTC App
Download the wallet client:

  - [Leather Wallet Browser Extension](https://leather.io/install-extension)
