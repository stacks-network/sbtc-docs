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
  "contract": "../asset-contract/contracts/asset.clar",
  "wif": "YOUR_WIF_CREDENTIALS",
  "bitcoin_node_url": "http://devnet:devnet@localhost:8001/api",
  "stacks_node_url": "http://localhost:3999",
  "stacks_transaction_fee": 2000,
  "bitcoin_transaction_fee": 2000,
  "contract_name": "sbtc-alpha-romeo-testing"
}
```
2. Create a folder `state` in `testing` folder
3. Create an empty file `log.ndjson`
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

