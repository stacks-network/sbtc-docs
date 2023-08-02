# sBTC Roadmap

TODO: Break down releases into their main components, outline their dependencies and create a gantt chart of how the work plan forward for sBTC is intended to look.

```mermaid
gantt
    dateFormat  YYYY-MM-DD
    title       sBTC Development roadmap
    excludes    weekends
    %% (`excludes` accepts specific dates in YYYY-MM-DD format, days of the week ("sunday") or "weekends", but not the word "weekdays".)

    section Clarity
    Completed task            :done,    des1, 2023-07-25, 2w
    Active task               :active,  des2, 2023-07-25, 2w
    Future task               :         des3, after des2, 5d
    Future task2              :         des4, after des3, 5d

    section Signer
    Completed task in the critical line :crit, done, 2023-08-06,24h
    Implement parser and jison          :crit, done, after des1, 2d
    Create tests for parser             :crit, active, 3d
    Future task in critical line        :crit, 5d
    Create tests for renderer           :2d
    Add to mermaid                      :1d
    Functionality added                 :milestone, 2023-08-25, 0d

    section SDK
    Describe gantt syntax               :after doc1, 3d
    Add gantt diagram to demo page      :20h
    Add another diagram to demo page    :48h

    section Documentation
    Describe gantt syntax               :active, a1, after des1, 3d
    Add gantt diagram to demo page      :after a1  , 20h
    Add another diagram to demo page    :doc1, after a1  , 48h
  
```

## Clarity
- Barebones contracts (pretty darn close! - end of August)
  - Signer running, can do handoff and do deposits and withdrawals.
  - No incentives, assume good behavior.
  - Only on testnet.
  - Assume signers are trustworthy and play nice. Users play nice.

- Developer release v2 contracts (? - end of September)
  - Assume signers play nice, but users can do anything.

- Nakamoto release contracts ( - end of November)

### Dependencies
- Updated test vectors

### Misc
"Developer preview"
- How much tokenomics should be a concern for the clarity group?
- Withdrawals initiated on Stacks - makes a lot of things easier.

----------------------------

- First draft of contracts
  - Asset contract
  - Deposit processor
  - Withdrawal processor
  - Handoff contract
  - Stacking pool contract
  - Bootstrap contract
- Deploy draft contracts to testnet

- Barebones contracts
- Developer release contracts
- Nakamoto contracts

## Signer
- API mockups
- Signer Network protocol
- PSBT Signing
- Initiating fulfillments
- Initiating handoff
- Auto-deny / configuration
- Reveal deposit
- Reveal withdrawal

- FIRE Ready
  - Documentation / library implementation

- Signer Binary
  - Integrate API
  - Use SDK
  - Implement "coordinator logic"

- Signer networking solution

- Signer integration tests

- Signer dashboard
- Signer web client
- Delegated signing

## Documentation

## SDK
- Stacks core migration
- sBTC transactions and wire formats (parsing and writing)
- Commit-reveal functionality

- Signer SDK
  - Part of sBTC SDK (sync with Stjepan)
  - Initiate signer
  - Set signer configuration
  - Get pending transactions
  - Sign transaction
    - Validate transactions to sign
  - Initiate signing round
