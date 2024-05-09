# Arbitrum BoLD audit details
- Total Prize Pool: Up to $300,000 in USDC
  - HM awards: Up to $235,000 in USDC
    - If one or more valid High severity findings are found, the HM pool is $235,000 in USDC
    - If one or more valid Medium severity findings are found, the HM pool is $85,000 in USDC
    - If no valid Highs or Mediums are found, the HM pool is $0 in USDC
  - QA awards: $40,000 in USDC
  - Judge awards: $15,000 in USDC
  - Validator awards: $10,000 in USDC
  - Scout awards: $500 in USDC
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/audits/2024-05-arbitrum-bold/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts May 10, 2024 20:00 UTC
- Ends May 27, 2024 20:00 UTC

**IMPORTANT NOTE:** Prior to receiving payment from this audit you MUST:
1. Become a [Certified Contributor](https://docs.code4rena.com/roles/certified-contributors) (successfully complete KYC)
2. Have a wallet that supports payout via Arbitrum.

You do not have to become certified before submitting bugs. But you must successfully complete the certification process within 30 days of the audit end date in order to receive awards. This applies to all audit participants, including wardens, teams, judges, validators, and scouts.

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-05-arbitrum-foundation/blob/main/4naly3er-report.md).



_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._
- It is always assumed that at least 1 honest validator would be actively participating in the rollup protocol.
- It is known that gas spending are not reimbursed in protocol at the moment, and an adversary can force the honest validators to collectively spend more than the adversary stake due to elevated gas price.
- It is assumed that off-chain computation costs are negligible, and honest validators can react to malicious action broadcasted on-chain immediately in the same block unless the malicious party spend their censorship budget.
- It is assumed that honest validators can collectively stake as much as the attackers willing to spend.
- It is assumed that the stake token would NOT be fee-on-transfer, rebasing, have a transfer hook or otherwise have non-standard ERC20 logics.
- The resolution of challenges that do not involve honest claims are out of scope unless they lead to incorrect assertions being confirmed
- Issues within One Step Proof contracts are out of scope in this contest, but might qualify for our immunefi bug bounty.
- We assume that an adversary can censor transactions for at most 1 `challengePeriodBlocks` or `confirmPeriodBlock` (whichever is smaller)
- Compromised keys are out of scope
- Centralization risk is out-of-scope
- Example: https://github.com/code-423n4/2023-08-arbitrum?tab=readme-ov-file#known-risks
- Any issues whose root cause exists in nitro-contracts@xxxxxx is considered out of scope but may be eligible for our [bug bounty](https://immunefi.com/bug-bounty/arbitrum/).


✅ SCOUTS: Please format the response above 👆 so its not a wall of text and its readable.

# Overview

[ ⭐️ SPONSORS: add info here ]

## Links

- **Previous audits:**  [We will have the public report ready by May 7th]
  - ✅ SCOUTS: If there are multiple report links, please format them in a list.
- **Documentation:** https://linktr.ee/arbitrumbold
- **Website:** https://arbitrum.foundation/
- **X/Twitter:** https://twitter.com/arbitrum
- **Discord:** https://discord.com/invite/arbitrum

---

# Scope


### Files in scope

*See [scope.txt](https://github.com/code-423n4/2024-05-arbitrum-foundation/blob/main/scope.txt)*

| File                                                                 | Logic Contracts | Interfaces | nSLOC    | Purpose | Libraries used |
|----------------------------------------------------------------------|-----------------|------------|----------|---------|----------------|
| src/libraries/Error.sol                                              |                 |            | 69       |         |                |
| src/challengeV2/IAssertionChain.sol                                  |                 | 1          | 6        |         |                |
| src/challengeV2/EdgeChallengeManager.sol                             | 1               | 1          | 286      |         |                |
| src/bridge/SequencerInbox.sol                                        | 1               |            | 594      |         |                |
| src/bridge/ISequencerInbox.sol                                       |                 | 1          | 32       |         |                |
| src/bridge/DelayBufferTypes.sol                                      |                 |            | 19       |         |                |
| src/bridge/DelayBuffer.sol                                           | 1               |            | 54       |         |                |
| src/assertionStakingPool/StakingPoolCreatorUtils.sol                 | 1               |            | 15       |         |                |
| src/assertionStakingPool/EdgeStakingPoolCreator.sol                  | 1               |            | 19       |         |                |
| src/assertionStakingPool/EdgeStakingPool.sol                         | 1               |            | 26       |         |                |
| src/assertionStakingPool/AssertionStakingPoolCreator.sol             | 1               |            | 19       |         |                |
| src/assertionStakingPool/AssertionStakingPool.sol                    | 1               |            | 32       |         |                |
| src/assertionStakingPool/AbsBoldStakingPool.sol                      | 1               |            | 35       |         |                |
| src/rollup/RollupUserLogic.sol                                       | 1               |            | 154      |         |                |
| src/rollup/RollupProxy.sol                                           | 1               |            | 27       |         |                |
| src/rollup/RollupLib.sol                                             | 1               |            | 55       |         |                |
| src/rollup/RollupCreator.sol                                         | 1               |            | 216      |         |                |
| src/rollup/RollupCore.sol                                            | 1               |            | 282      |         |                |
| src/rollup/RollupAdminLogic.sol                                      | 1               |            | 173      |         |                |
| src/rollup/IRollupLogic.sol                                          |                 | 1          | 7        |         |                |
| src/rollup/IRollupCore.sol                                           |                 | 1          | 35       |         |                |
| src/rollup/IRollupAdmin.sol                                          |                 | 1          | 9        |         |                |
| src/rollup/Config.sol                                                |                 |            | 42       |         |                |
| src/rollup/BridgeCreator.sol                                         | 1               |            | 98       |         |                |
| src/rollup/BOLDUpgradeAction.sol                                     | 4               | 3          | 391      |         |                |
| src/rollup/AssertionState.sol                                        | 1               |            | 17       |         |                |
| src/rollup/Assertion.sol                                             | 1               |            | 52       |         |                |
| src/challengeV2/libraries/UintUtilsLib.sol                           | 1               |            | 40       |         |                |
| src/challengeV2/libraries/MerkleTreeLib.sol                          | 1               |            | 118      |         |                |
| src/challengeV2/libraries/Enums.sol                                  |                 |            | 10       |         |                |
| src/challengeV2/libraries/EdgeChallengeManagerLib.sol                | 1               |            | 396      |         |                |
| src/challengeV2/libraries/ChallengeErrors.sol                        |                 |            | 54       |         |                |
| src/challengeV2/libraries/ChallengeEdgeLib.sol                       | 1               |            | 158      |         |                |
| src/challengeV2/libraries/ArrayUtilsLib.sol                          | 1               |            | 30       |         |                |
| src/assertionStakingPool/interfaces/IEdgeStakingPoolCreator.sol      |                 | 1          | 5        |         |                |
| src/assertionStakingPool/interfaces/IEdgeStakingPool.sol             |                 | 1          | 6        |         |                |
| src/assertionStakingPool/interfaces/IAssertionStakingPoolCreator.sol |                 | 1          | 10       |         |                |
| src/assertionStakingPool/interfaces/IAssertionStakingPool.sol        |                 | 1          | 5        |         |                |
| src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol          |                 | 1          | 7        |         |                |
| **Total:**                                                           |          **27** |     **14** | **3603** |         |                |

### Files out of scope

*See [out_of_scope.txt](https://github.com/code-423n4/2024-04-renzo/blob/main/scope.txt)*


## Scoping Q &amp; A

### General questions


| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |       None             |
| Test coverage                           | ✅ SCOUTS: Please populate this after running the test coverage command                          |
| ERC721 used  by the protocol            |            None              |
| ERC777 used by the protocol             |           None                |
| ERC1155 used by the protocol            |              None            |
| Chains the protocol will be deployed on | Ethereum,Arbitrum |


### External integrations (e.g., Uniswap) behavior in scope:


| Question                                                  | Answer |
| --------------------------------------------------------- | ------ |
| Enabling/disabling fees (e.g. Blur disables/enables fees) | No   |
| Pausability (e.g. Uniswap pool gets paused)               |  No   |
| Upgradeability (e.g. Uniswap gets upgraded)               |   No  |


### EIP compliance checklist
N/A


# Additional context

## Main invariants

- Challenges cannot bypass the grace period, challenges must take at least one `challengePeriodBlocks + challengeGracePeriodBlocks`
- Assertions cannot be confirmed before `confirmPeriodBlocks` after creation
- Honest assertions / layer zero edges are always confirmed under the BoLD paper’s assumptions within 2 challenge periods.
    - plus the security council grace period for assertions
- Edge ID’s and Assertion ID’s must be unique

## Attack ideas (where to focus for bugs)
- Assuming there is at least one honest participant, no incorrect assertions or edges can be confirmed.
- Honest parties can always retrieve their assertion stakes and challenge bonds
- History commitments and their proofs can’t be manipulated somehow.
- Issues that the security council could fix during the grace period are still in scope if they are the result of an incorrect resolution of the challenge protocol.


## All trusted roles in the protocol

| Role                                | Description                       |
| --------------------------------------- | ---------------------------- |
| `excessStakeReceiver`                          | will be set to a DAO controlled address. Used to reimburse honest validators |

 
Note: there is a “grace period” after assertions are confirmed via challenge. This is to ensure the result of the challenge is widely observable before it causes an assertion to be confirmed. The security council can act within this grace period in the event a bad assertion is confirmed.


## Describe any novel or unique curve logic or mathematical models implemented in the contracts:

N/A


## Running tests

The README has instructions for running go tests. 
To run foundry tests:

cd contracts
forge test # use --gas-report (some irrelevant tests will fail though)

✅ SCOUTS: Please format the response above 👆 using the template below👇

```bash
git clone https://github.com/code-423n4/2023-08-arbitrum
git submodule update --init --recursive
cd governance
foundryup
make install
make build
make sc-election-test
```
To run code coverage
```bash
make coverage
```

✅ SCOUTS: Add a screenshot of your terminal showing the test coverage

## Miscellaneous
Employees of Arbitrum and employees' family members are ineligible to participate in this audit.



