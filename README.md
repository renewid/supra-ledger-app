# Supra Ledger app

A Ledger device app for the [Supra L1](https://supra.com) blockchain, brought up to Ledger's current requirements for a listing in Ledger Live.

> **Unofficial.** This project is not affiliated with or endorsed by Supra or the Entropy Foundation. The Supra name and logo belong to their owners.
>
> **Not audited.** No security audit has been done yet.

This repository only presents the project. The source code is kept private for now and is available to the Supra team on request.

## Status

| Area | State |
|---|---|
| Devices | Nano S Plus, Nano X, Stax, Flex, Apex P |
| UI | NBGL use-case API on all devices, info and settings pages per Ledger's design guidelines |
| SDK | Ledger SDK v27.1.7, `Makefile.standard_app`, `lib_standard_app` |
| CI | Ledger's mandatory workflows (build, guidelines enforcer), CodeQL, unit tests, lint, spell check, functional tests: all green except the enforcer's `APP_LOAD_PARAMS` check, which can only pass once the app is registered in Ledger's app database |
| Tests | Ragger functional tests with snapshots of every review screen on all five devices |
| Fuzzing | Transaction-parser fuzzer with ASan/UBSan covering every clear-signed function, about 20 million runs per 2 minutes without findings |
| Static analysis | CodeQL `security-and-quality`: 0 results |

## Features

- **Clear signing** of the framework functions wallets use, checked against the Supra framework as deployed on mainnet:
  - transfers: `supra_account::transfer`, `transfer_coins`, `coin::transfer`
  - batch transfers: `supra_account::batch_transfer`, `batch_transfer_coins`, with every receiver and amount and the total
  - staking with delegation pools: `pbo_delegation_pool::add_stake`, `unlock`, `reactivate_stake`, `withdraw`
  - fungible assets: `primary_fungible_store::transfer`
- **Every transaction review** ends with the maximum gas fee, the expiration time in UTC and, unless it is mainnet, the network (testnet or chain ID)
- **Amounts** in `SUPRA` with 8 decimals; other coins in base units next to their full coin type
- **Address verification** on the device, in the long form StarKey and Suprascan use
- **Blind-signing setting**, off by default; with it off, the device explains why and offers to open the settings
- **Message signing**: printable text is shown in full, other data only with blind signing and as a hash
- **Path restriction**: only hardened `m/44'/<coin type>'/...` paths (SLIP-10 ed25519)
- **Security fixes** over the upstream app, including stricter detection of the clear-signed framework functions, a length check so no byte is signed without being decoded, key material removed from debug output and checked crypto calls

## Screenshots

| Device | Home | Transfer review | Staking review | Blind-signing warning |
|---|---|---|---|---|
| Nano S Plus | <img src="screenshots/nanosp/home.png" width="200"> | <img src="screenshots/nanosp/transfer_receiver.png" width="200"> | <img src="screenshots/nanosp/staking_review.png" width="200"> | <img src="screenshots/nanosp/blind_signing_warning.png" width="200"> |
| Nano X | <img src="screenshots/nanox/home.png" width="200"> | <img src="screenshots/nanox/transfer_receiver.png" width="200"> | <img src="screenshots/nanox/staking_review.png" width="200"> | <img src="screenshots/nanox/blind_signing_warning.png" width="200"> |
| Stax | <img src="screenshots/stax/home.png" width="160"> | <img src="screenshots/stax/transfer_receiver.png" width="160"> | <img src="screenshots/stax/staking_review.png" width="160"> | <img src="screenshots/stax/blind_signing_warning.png" width="160"> |
| Flex | <img src="screenshots/flex/home.png" width="160"> | <img src="screenshots/flex/transfer_receiver.png" width="160"> | <img src="screenshots/flex/staking_review.png" width="160"> | <img src="screenshots/flex/blind_signing_warning.png" width="160"> |
| Apex P | <img src="screenshots/apex_p/home.png" width="160"> | <img src="screenshots/apex_p/transfer_receiver.png" width="160"> | <img src="screenshots/apex_p/staking_review.png" width="160"> | <img src="screenshots/apex_p/blind_signing_warning.png" width="160"> |

## Road to a listing

Done: development basis, Ledger toolchain, security and signing basics, NBGL UI on all current devices, test coverage, clear signing of transfers, batches, staking and fungible assets.

Next:
1. Host library (`hw-app-supra`, JS/TS) and an integration guide for wallets
2. Fee payer / multi-agent transactions, decimals of well-known tokens
3. Security audit and submission to Ledger

## Proof of state

The work shown here corresponds to commit `eb0aae85b9dfbf0820d19b44af4cc08a24b7d367` of the private repository.

## Contact

Developer: r3new ([renewid on GitHub](https://github.com/renewid))

© 2026 r3new. Text and screenshots in this repository: all rights reserved.
