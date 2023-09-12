# Get Started with sBTC 0.1

## Local setup
### Requirements
* install [clarinet](https://github.com/hirosystems/clarinet)
* checkout main branch of repository [github.com/stacks-network/sbtc](https://github.com/stacks-network/sbtc)

### Configuration
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

Open up your terminal and run the following commands. When running `clarinet integrate`, you will be prompted to overwrite the deployment script, press `N` to say no.

```
cd romeo
cd asset-contract
# Press N when asked to overwrite deployment script
clarinet integrate
```

This will open a Clarinet dashboard that allows you to see the status of your local devnet blockchain. If you aren't familiar with Clarinet or its `integrate` feature, check out the [Clarinet docs](https://github.com/hirosystems/clarinet).

You can monitor this dashboard to see when your local devnet is ready.

![clarinet dashboard](https://user-images.githubusercontent.com/1449049/258456703-44d219ae-3516-47a3-aa4b-d3e6dc6a8f6a.png)

### Launch sBTC DR binary
While waiting for `clarinet integrate` to finish setting up your devnet, open up a separate terminal window and start the sBTC binary from the root of the git repo.

```
cargo run -p romeo -- -c config.json
```

Now head back to the first terminal window that has Clarinet's devnet running and press `N` to min a new block and see how the transaction is processed.

See `settings/Devnet.toml` for seed phrases for pre-filled accounts.

