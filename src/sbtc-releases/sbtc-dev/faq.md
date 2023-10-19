# FAQ

## Devenv

### What is the meaning of the warning in stacks logs `Relayer: Failed to submit Bitcoin transaction`?
  It is a warning related to stacks mining and not relevant to sBTC transactions.

### How to check progress when starting devenv? When can I start sending deposits?
Wait until the stacks block height (`stacks_tip_height`) is 2 or more by checking http://localhost:3999/v2/info.
Initially, the server won't response.

For more detailed information about the progress you can look at `./logs.sh stacks-api`.
Wait until you see a message like `Proxy created: /  -> http://stacks:20443`

or

you can look at `./logs.sh stacks`.
There is a time after Clarity state genesis was computed without log messages. Just keep on waiting...
See the bitcoin blocks syncing and wait for messages like `Miner: mined anchored block ...`.