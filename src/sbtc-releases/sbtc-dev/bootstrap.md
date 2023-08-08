# Bootstrapping sBTC 0.1

## Local setup
### Requirements
* install [clarinet](https://github.com/hirosystems/clarinet)
* checkout sbtc-mini/631-deployments branch of repository [github.com/Trust-Machines/stacks-sbtc](https://github.com/Trust-Machines/stacks-sbtc)

### Launch devnet

In a first console
```
cd sbtc-mini
# Press N when asked to overwrite deployment script
./scripts/integration-test.sh
```
See clarinet dashboard what until local devnet is ready (around block 137)

![clarinet dashboard](https://user-images.githubusercontent.com/1449049/258456703-44d219ae-3516-47a3-aa4b-d3e6dc6a8f6a.png)

In a second console run the bootstrap call

```
# Press Enter when asked to overwrite
clarinet deployments apply deployments/bootstrap.devnet-plan.yaml
```

In first console, press N to produce a block and see how the tx is processed.

See `settings/Devnet.toml` for seed phrases for pre-filled accounts.

