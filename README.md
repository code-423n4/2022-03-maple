# ✨ So you want to sponsor a contest

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos:
- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted.

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest is over and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Contest setup

## ⭐️ Sponsor: Provide contest details

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [ ] Name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each
  - [ ] external contracts called in each
  - [ ] libraries used in each
- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Does the token conform to the ERC-20 standard? In what specific ways does it differ?
- [ ] Describe anything else that adds any special logic that makes your approach unique
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Add all of the code to this repo that you want reviewed
- [ ] Create a PR to this repo with the above changes.

---

# Contest prep

## ⭐️ Sponsor: Contest prep
- [ ] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [ ] Modify the bottom of this `README.md` file to describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2021-06-gro/blob/main/README.md))
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 8 hours prior to contest start time.**
- [ ] Ensure that you have access to the _findings_ repo where issues will be submitted.
- [ ] Promote the contest on Twitter (optional: tag in relevant protocols, etc.)
- [ ] Share it with your own communities (blog, Discord, Telegram, email newsletters, etc.)
- [ ] Optional: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# Maple Finance contest details
- $47,500 USDC main award pot
- $2,500 USDC gas optimization award pot
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2022-03-maple-finance-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts March 17, 2022 00:00 UTC
- Ends March 21, 2022 23:59 UTC

This repo will be made public before the start of the contest. (C4 delete this line when made public)


# Audit Scope

This scope of this audit includes the following repos, all with corresponding release tags:

- [maple-labs/loan](https://github.com/maple-labs/loan/releases/tag/v3.0.0-beta.1)
- [maple-labs/erc20](https://github.com/maple-labs/erc20/releases/tag/v1.0.0-beta.2)
- [maple-labs/mpl-migration](https://github.com/maple-labs/mpl-migration/releases/tag/v1.0.0-beta.1)
- [maple-labs/revenue-distribution-token](https://github.com/maple-labs/revenue-distribution-token/releases/tag/v1.0.0-beta.1)
- [maple-labs/xmpl](https://github.com/maple-labs/xmpl/releases/tag/v1.0.0-beta.1)

These contracts include inheritance, so the scope of the audit will be expressed as the contracts at the lowest end of the hierarchy, as these are what will be deployed to mainnet. Since there are no external libraries used, all of the code that these flattened contracts use is in scope for audit (excpet for loan, which will be expressed as a diff between releases).

## `maple-labs/loan`
This release includes an alteration of the original v2.0.0 release that includes the following changes:
1. Updates fee structure to move from an upfront establishment fee to an ongoing fee that is paid on every payment.
2. Updates the refinancing capabilities to include a refinance deadline as well as the ability to reject refinance terms.
3. Adds unpaid interest to the subsequent payment after a refinance

The diff of the code can be found here, which contains all code that is in scope for the audit: https://github.com/maple-labs/loan/compare/v2.0.0...v3.0.0-beta.1

The files to be audited are:
- `MapleLoan.sol`
- `MapleLoanInitializer.sol`
- `MapleLoanInternals.sol`

## `maple-labs/erc20`
The files to be audited are:
- `ERC20.sol`

## `maple-labs/mpl-migration`
The files to be audited are:
- `Migrator.sol`

## `maple-labs/revenue-distribution-token`
The files to be audited are:
- `RevenueDistributionToken.sol`

## `maple-labs/xmpl`
xMPL has dependencies on:
- `erc20`
- `revenue-distribution-token`
- `mpl-migration` (not a direct dependency, but it is supposed to interact with Migrator.sol)

The files to be audited are:
- `xMPL.sol` (which includes all code from dependencies listed above)

If any clarification on scope is needed, or if there are any other questions, please comment below this issue.

## Focus Areas
- **Locked funds**: Ensure that there is no way for funds to get locked in the xMPL, Migrator or Loan smart contracts.
- **Stoten funds**: Ensure that any funds that are held custody by contracts cannot be withdrawn maliciously.
- **Invariants**: Ensure that all invariants outlined in the xMPL and RDT repos are upheld.
- **Accounting Exploitation**: Ensure that no users can perform any actions to exploit/manipulate accounting to their favor.
- **Refinancing**: Ensure that the Refinancer contract cannot be used maliciously to exploit the Loan.

In all repos, all dependencies can be found in the `./modules` directory. All repo READMEs include instructions on how to get the environment up and running for testing. All repos have their own unit testing suite, including verbose unit testing fuzz testing, and invariant testing.

All technical documentation related to this release for Loan will be located in the `maple-labs/loan` [wiki](https://github.com/maple-labs/loan/wiki). We HIGHLY recommend reviewing this wiki before beginning the audit.

There is also a [wiki](https://github.com/maple-labs/maple-core/wiki) for our V1 protocol if any further context is needed on how deployed V1 contracts work (Pools, StakeLocker, etc.)

It is recommended to clone our integration testing repo [contract-test-suite](https://github.com/maple-labs/contract-test-suite) locally in order to provide clearer context with how these contracts interact with the rest of the protocol.

It is also recommended to clone our economic simulation testing repo [loan-simiulations](https://github.com/maple-labs/loan-simiulations) locally in order to provide clearer context with how the refinancing functionality is expected to behave from a business perspective.

## Observations

In the wiki, there's a page called [List of Assumptions](https://github.com/maple-labs/loan/wiki/List-of-Assumptions) which outlines some basic conditions/assumptions that we assume that will always hold true. Therefore any issue that does not abide by these assumptions will likely be considered invalid.
