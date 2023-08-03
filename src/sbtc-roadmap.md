# sBTC Roadmap

TODO: Break down releases into their main components, outline their dependencies and create a gantt chart of how the work plan forward for sBTC is intended to look.

```mermaid
gantt
    dateFormat  YYYY-MM-DD
    title       sBTC Development roadmap
    excludes    weekends
    %% (`excludes` accepts specific dates in YYYY-MM-DD format, days of the week ("sunday") or "weekends", but not the word "weekdays".)

    section Clarity
    Draft 0.1 contracts            :  clar1, 2023-07-25, 2w
    Update 0.1 contracts           :  clar2, after clar1, 4w
    Draft 0.2 contracts            :  clar3, after clar1, 4w
    Update 0.2 contracts           :  clar4, after clar3, 4w
    Draft 1.0 contracts            :  clar5, after clar3, 4w
    Update 1.0 contracts           :  clar6, after clar5, 4w

    section Signer
    Draft 0.1 contracts            :  clar1, 2023-07-25, 2w
    Update 0.1 contracts           :  clar2, after clar1, 4w
    Draft 0.2 contracts            :  clar3, after clar1, 4w
    Update 0.2 contracts           :  clar4, after clar3, 4w
    Draft 1.0 contracts            :  clar5, after clar3, 4w
    Update 1.0 contracts           :  clar6, after clar5, 4w

    section SDK
    Break out core Stacks types             :  sdk1, 2023-07-25, 2w
    Support creating sBTC transactions      :  sdk2, after sdk1, 2w
    Signer SDK 0.1                          :  sdk3, after sdk1, 8w

    section Documentation
    sBTC High level :            doc1, 2023-07-25, 2w
    Document sBTC 1.0 :            doc1, 2023-07-25, 2w
    Update 0.1 contracts           :  doc2, after doc1, 4w
  
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
