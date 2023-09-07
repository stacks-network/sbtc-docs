# Get Started with sBTC 0.1

## Local setup
### Requirements
* install [clarinet](https://github.com/hirosystems/clarinet)
* checkout main branch of repository [github.com/stacks-network/sbtc](https://github.com/stacks-network/sbtc)

### Configuration
1. Create configuration file `config.json` in a new folder `romeo/testing` with the following content:
```
{
  "state_directory": "./state",
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
  "mnemonic": "negative aspect salad kitchen banner enlist cram deputy describe exit jar anger market flip orient age maze ocean ranch hungry flavor disease high nothing",
  "wif": "cNidUqiKvU3K6sypaRPNhPsXL1dcW8sdzP2JJJaCjUdK39t6F5rS",
  "private_key": "0x21ea7a9532dd379130430220baba1686dbae7f5364157f2e2827a96aa1ae3f2f",
  "public_key": "0x029a19ac8fb93a7b9416b5be603fa19d53eca8f6e33ac89667abc7e777afd3b816",
  "stacks_address": "ST17Z635X9KRCAVHNRX3HX4XAWBJ0KM4VEHZZ2KBW",
  "bitcoin_taproot_address_tweaked": "tb1p90dyzzyuqdzc9lmfss8qmds3m8cur2jyf4et99pu87t583s35xzqdltv60",
  "bitcoin_taproot_address_untweaked": "tb1pngv6erae8faeg944hesrlgva20k23ahr8tyfveatclnh0t7nhqtqufj023",
  "bitcoin_p2pkh_address": "mnogqbtDSktLEfz3G5wpZUPq61AVZTjPYC",
  "bitcoin_p2wpkh_address": "tb1qflese02v7rzkudw8gu0f82hzusyapxm567p469"
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
In a second console, start the sBTC DR binary from the root of the git repo

```
cargo run -p romeo -- -c config.json
```

In first console, press N to produce a block and see how the tx is processed.

See `settings/Devnet.toml` for seed phrases for pre-filled accounts.

