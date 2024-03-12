# Repo setup

## ⭐️ Sponsor: Add code to this repo

- [x] Create a PR to this repo with the below changes:
- [x] Provide a self-contained repository with working commands that will build (at least) all in-scope contracts, and commands that will run tests producing gas reports for the relevant contracts.
- [x] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [x] Please have final versions of contracts and documentation added/updated in this repo **no less than 48 business hours prior to audit start time.**
- [x] Be prepared for a 🚨code freeze🚨 for the duration of the audit — important because it establishes a level playing field. We want to ensure everyone's looking at the same code, no matter when they look during the audit. (Note: this includes your own repo, since a PR can leak alpha to our wardens!)


---

## ⭐️ Sponsor: Edit this `README.md` file

- [ ] Modify the contents of this `README.md` file. Describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. (Here are two well-constructed examples: [Ajna Protocol](https://github.com/code-423n4/2023-05-ajna) and [Maia DAO Ecosystem](https://github.com/code-423n4/2023-05-maia))
- [ ] Review the Gas award pool amount. This can be adjusted up or down, based on your preference - just flag it for Code4rena staff so we can update the pool totals across all comms channels.
- [ ] Optional / nice to have: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] [This checklist in Notion](https://code4rena.notion.site/Key-info-for-Code4rena-sponsors-f60764c4c4574bbf8e7a6dbd72cc49b4#0cafa01e6201462e9f78677a39e09746) provides some best practices for Code4rena audits.

## ⭐️ Sponsor: Final touches
- [ ] Review and confirm the details in the section titled "Scoping details" and alert Code4rena staff of any changes.
- [ ] Review and confirm the list of in-scope files in the `scope.txt` file in this directory.  Any files not listed as "in scope" will be considered out of scope for the purposes of judging, even if the file will be part of the deployed contracts.
- [ ] Check that images and other files used in this README have been uploaded to the repo as a file and then linked in the README using absolute path (e.g. `https://github.com/code-423n4/yourrepo-url/filepath.png`)
- [ ] Ensure that *all* links and image/file paths in this README use absolute paths, not relative paths
- [ ] Check that all README information is in markdown format (HTML does not render on Code4rena.com)
- [ ] Remove any part of this template that's not relevant to the final version of the README (e.g. instructions in brackets and italic)
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# Coinbase audit details
- Total Prize Pool: $49,000 in USDC
  - HM awards: $24,750 in USDC 
  - QA awards: $750 in USDC
  - Bot Race awards: $2,250 in USDC
  - Analysis awards: $1,500 in USDC
  - Gas awards: $750 in USDC
  - Judge awards: $5,600 in USDC
  - Lookout awards: $2,400 in USDC
  - Scout awards: $500 in USDC
  - Mitigation Review: $10,500 in USDC (Opportunity goes to top 3 certified wardens based on placement in this audit.)
 
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2024-03-coinbase/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts March 14, 2024 20:00 UTC
- Ends March 21, 2024 20:00 UTC

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-03-coinbase/blob/main/4naly3er-report.md).

Automated findings output for the audit can be found [here](https://github.com/code-423n4/2024-03-coinbase/blob/main/bot-report.md) within 24 hours of audit opening.

_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

[ ⭐️ SPONSORS: Are there any known issues or risks deemed acceptable that shouldn't lead to a valid finding? If so, list them here. ]


# Overview

This audit covers four separate but related groups of code
- SmartWallet is a smart contract wallet. In addition to Ethereum address owners, it supports passkey owners and validates their signatures via WebAuthnSol. It supports multiple owners and allows for signing account-changing user operations such that they can be replayed across any EVM chain where the account has the same address. It is ERC-4337 compliant and can be used with paymasters such as MagicSpend. 
- WebAuthnSol is a library for verifying WebAuthn Authentication Assertions onchain. 
- FreshCryptoLib is an excerpt from [FreshCryptoLib](https://github.com/rdubois-crypto/FreshCryptoLib/tree/master/solidity), including the function `ecdsa_verify` and all code it depends on. This function is used by WebAuthnSol onchains without the RIP-7212 verifier.
- MagicSpend is a contract that allows for signature-based withdraws. MagicSpend is a EntryPoint v0.6 compliant paymaster and also allows using withdraws to pay transaction gas, in this way. 

## Links

- **Previous audits:** 
TODO
- Coinbase completed an audit of FreshCryptoLib with artifacts [here](https://github.com/base-org/FCL-ecdsa-verify-audit/tree/main).
- **Documentation:** Each folder has a detailed README. Please read those. 



# Scope


*List all files in scope in the table below (along with hyperlinks) -- and feel free to add notes here to emphasize areas of focus.*

| Contract | SLOC | Purpose | External Imports |  
| ----------- | ----------- | ----------- | ----------- |
| [src/SmartWallet/MultiOwnable.sol](https://github.com/code-423n4/2024-03-coinbase/blob/main/src/SmartWallet/MultiOwnable.sol) | 80 | Auth contract, supporting multiple owners and owners identified as bytes to allow for secp256r1 public keys | |
| [src/SmartWallet/ERC1271.sol](https://github.com/code-423n4/2024-03-coinbase/blob/main/src/SmartWallet/ERC1271.sol) | 54 | Abstract contract for ERC-1271 support for CoinbaseSmartWallet | |
| [src/SmartWallet/CoinbaseSmartWalletFactory.sol](https://github.com/code-423n4/2024-03-coinbase/blob/main/src/SmartWallet/CoinbaseSmartWalletFactory.sol) | 35 | ERC-4337 compliant Factory for CoinbaseSmartWallet | [solady/utils/LibClone.sol](https://github.com/Vectorized/solady/blob/main/src/utils/LibClone.sol) |
| [src/SmartWallet/CoinbaseSmartWallet.sol](https://github.com/code-423n4/2024-03-coinbase/blob/main/src/SmartWallet/CoinbaseSmartWallet.sol) | 35 | ERC-4337 compliant smart account | [solady/accounts/Receiver.sol](https://github.com/Vectorized/solady/blob/main/src/accounts/Receiver.sol) [solady/utils/UUPSUpgradeable.sol](https://github.com/Vectorized/solady/blob/main/src/utils/UUPSUpgradeable.sol) [solady/utils/SignatureCheckerLib.sol](https://github.com/Vectorized/solady/blob/main/src/utils/SignatureCheckerLib.sol) [account-abstraction/interfaces/UserOperation.sol](https://github.com/eth-infinitism/account-abstraction/blob/abff2aca61a8f0934e533d0d352978055fddbd96/contracts/interfaces/UserOperation.sol)|
| [src/WebAuthnSol/WebAuthnSol.sol](https://github.com/code-423n4/2024-03-coinbase/blob/main/src/WebAuthnSol/WebAuthn.sol) | 54 | Solidity WebAuthn verifier| [solady/utils/LibClone.sol](https://github.com/Vectorized/solady/blob/main/src/utils/LibClone.sol) |


## Out of scope

*List any files/contracts that are out of scope for this audit.*

# Additional Context

- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Please list specific ERC20 that your protocol is anticipated to interact with. Could be "any" (literally anything, fee on transfer tokens, ERC777 tokens and so forth) or a list of tokens you envision using on launch.
- [ ] Please list specific ERC721 that your protocol is anticipated to interact with.
- [ ] Which blockchains will this code be deployed to, and are considered in scope for this audit?
- [ ] Please list all trusted roles (e.g. operators, slashers, pausers, etc.), the privileges they hold, and any conditions under which privilege escalation is expected/allowable
- [ ] In the event of a DOS, could you outline a minimum duration after which you would consider a finding to be valid? This question is asked in the context of most systems' capacity to handle DoS attacks gracefully for a certain period.
- [ ] Is any part of your implementation intended to conform to any EIP's? If yes, please list the contracts in this format: 
  - `Contract1`: Should comply with `ERC/EIPX`
  - `Contract2`: Should comply with `ERC/EIPY`

## Attack ideas (Where to look for bugs)
- SmartWallet
  - Can an move funds from the account
  - Can an attacker brick (make unusable) the account
- MagicSpend
  - Can attacker withdraw using an invalid WithdrawRequest
  - Can an attacker receive more than WithdrawRequest.amount 


## Main invariants
*Describe the project's main invariants (properties that should NEVER EVER be broken).*

## Scoping Details 
[ ⭐️ SPONSORS: please confirm/edit the information below. ]

```
- If you have a public code repo, please share it here: https://github.com/coinbase/smart-wallet, https://github.com/coinbase/magic-spend, https://github.com/base-org/webauthn-sol, https://github.com/base-org/fresh-crypto-lib-audit  
- How many contracts are in scope?: 7   
- Total SLoC for these contracts?: 785  
- How many external imports are there?: 14  
- How many separate interfaces and struct definitions are there for the contracts within scope?: 0  
- Does most of your code generally use composition or inheritance?: Composition   
- How many external calls?: 0   
- What is the overall line coverage percentage provided by your tests?: 95
- Is this an upgrade of an existing system?: False
- Check all that apply (e.g. timelock, NFT, AMM, ERC20, rollups, etc.): 
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?: False  
- Please describe required context:   
- Does it use an oracle?: No
- Describe any novel or unique curve logic or mathematical models your code uses: 
- Is this either a fork of or an alternate implementation of another project?: True   
- Does it use a side-chain?:
- Describe any specific areas you would like addressed:
```

# Tests


This repository is managed using [Foundry](https://book.getfoundry.sh).

**Install Foundry**

Run the following command and then follow the instructions.
```bash
curl -L https://foundry.paradigm.xyz | bash
```


**Install Modules**
```bash
forge install
```

**Run Tests**
```bash
forge test
```


Solidity `0.8.23` is used to compile and test the smart contracts. 

## Miscellaneous

Employees of Coinbase and employees' family members are ineligible to participate in this audit.