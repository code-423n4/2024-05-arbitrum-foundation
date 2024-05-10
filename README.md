# Arbitrum BoLD audit details
- Total Prize Pool: Up to $300,500 in USDC
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
2. Have an address that supports payout via Arbitrum.

You do not have to become certified before submitting bugs. But you must a) successfully complete the certification process and b) supply a wallet to the C4 team that supports Arbitrum payouts within 30 days of the award announcement in order to receive awards. This applies to all audit participants, including wardens, teams, judges, validators, and scouts.

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
- Any issues whose root cause exists in [nitro-contracts@77ee9de](https://github.com/OffchainLabs/nitro-contracts/commit/77ee9de042de225fab560096f7624f3d13bd12cb) is considered out of scope but may be eligible for our [bug bounty](https://immunefi.com/bug-bounty/arbitrum/).



# Overview

Enabling permissionless validation has long been a goal of Arbitrum on the [progressive journey towards decentralization](https://docs.arbitrum.foundation/state-of-progressive-decentralization). Today, Arbitrum One and Arbitrum Nova rely on a permissioned set of valdiators to prevent *[delay attacks](https://medium.com/offchainlabs/solutions-to-delay-attacks-on-rollups-434f9d05a07a)* against the current rollup protocol - a class of attacks where actors can open disputes and delay on-chain confirmations (as long as actors are willing to sacrifice capital to do so).
 
Arbitrum BoLD is a new dispute protocol that enables permissionless validation by mitigating the risks of delay attacks with a fixed upper time bound for challenge resolution in an all-vs-all format. Specifically, BoLD is an upgrade to the interactive & fully operational fraud proof system that is currently used to secure all Arbitrum chains in production today, including Arbitrum One.

## Links

- **Previous audits:**  [ToB audit](https://github.com/trailofbits/publications/blob/master/reviews/2024-04-offchainbold-securityreview.pdf)
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
| src/libraries/Error.sol                                              |                 |            | 69       | Errors        |                |
| src/challengeV2/IAssertionChain.sol                                  |                 | 1          | 6        | Interface that the Challenge Manager uses to interact with the assertion chain / rollup        |                |
| src/challengeV2/EdgeChallengeManager.sol                             | 1               | 1          | 286      | The main Challenge Manager contract. Validators create edges, bisect edges, update timers, prove edges, confirm edges, etc | `EdgeChallengeManagerLib`, `ChallengeEdgeLib`, OZ `SafeERC20` |
| src/bridge/SequencerInbox.sol                                        | 1               |            | 594      | Accepts batches of transactions from the Sequencer | `ArbitrumChecker`, `Messages`, `DelayBuffer` |
| src/bridge/ISequencerInbox.sol                                       |                 | 1          | 32       | Interface for the Sequencer Inbox        |                |
| src/bridge/DelayBufferTypes.sol                                      |                 |            | 19       | Types for the delay buffer feature of the Sequencer Inbox | `Messages` |
| src/bridge/DelayBuffer.sol                                           | 1               |            | 54       | Functions used by the Sequencer Inbox to handle the delay buffer | |
| src/assertionStakingPool/StakingPoolCreatorUtils.sol                 | 1               |            | 15       | Contains a helper function for staking pool creators to predict CREATE2 addresses | OZ `Create2`, OZ `Address` |
| src/assertionStakingPool/EdgeStakingPoolCreator.sol                  | 1               |            | 19       | Factory contract for creating new `EdgeStakingPool` contracts | `StakingPoolCreatorUtils` |
| src/assertionStakingPool/EdgeStakingPool.sol                         | 1               |            | 26       | Trustless staking pool for creating layer zero edges. Each pool commits to creating an edge with specified ID once it has enough deposits | OZ `SafeERC20` |
| src/assertionStakingPool/AssertionStakingPoolCreator.sol             | 1               |            | 19       | Factory contract for creating new `AssertionStakingPool` contracts | `StakingPoolCreatorUtils` |
| src/assertionStakingPool/AssertionStakingPool.sol                    | 1               |            | 32       | Trustless staking pool for creating top level assertions. Each pool commits to creating an assertion with specified ID once it has enough deposits | OZ `SafeERC20` |
| src/assertionStakingPool/AbsBoldStakingPool.sol                      | 1               |            | 35       | Abstract contract handling both staking pools' deposit and withdrawal logic | OZ `SafeERC20` |
| src/rollup/RollupUserLogic.sol                                       | 1               |            | 154      | Rollup functions that are meant to be called by validators. Inherits `RollupCore` | `AssertionNodeLib`, `GlobalStateLib`, OZ `SafeERC20`, `RollupLib` |
| src/rollup/RollupProxy.sol                                           | 1               |            | 27       | Proxy with two logic contracts: `RollupUserLogic` and `RollupAdminLogic`||
| src/rollup/RollupLib.sol                                             | 1               |            | 55       | Hashing utilities for the rollup contracts | `AssertionNodeLib`, `GlobalStateLib` |
| src/rollup/RollupCreator.sol                                         | 1               |            | 216      | Factory for deploying and initiatializing the full suite of contracts | OZ `SafeERC20` |
| src/rollup/RollupCore.sol                                            | 1               |            | 282      | Common rollup view functions and internal mutative functions used by `RollupUserLogic` and `RollupAdminLogic` | `AssertionNodeLib`, `GlobalStateLib`, `RollupLib`, `ArbitrumChecker`|
| src/rollup/RollupAdminLogic.sol                                      | 1               |            | 173      | Rollup functions meant to be called by an admin (such as a security council)| `AssertionStateLib`, `RollupLib` |
| src/rollup/IRollupLogic.sol                                          |                 | 1          | 7        | Interface for `RollupUserLogic` |                |
| src/rollup/IRollupCore.sol                                           |                 | 1          | 35       | Interface for `RollupCore`        |                |
| src/rollup/IRollupAdmin.sol                                          |                 | 1          | 9        | Interface for `RollupAdminLogic`        |                |
| src/rollup/Config.sol                                                |                 |            | 42       | Structs for rollup configuration        |                |
| src/rollup/BridgeCreator.sol                                         | 1               |            | 98       | Used by the `RollupCreator` to deploy and initialize the full suite of bridge contracts        |                |
| src/rollup/BOLDUpgradeAction.sol                                     | 4               | 3          | 391      | Upgrades Arbitrum chains from the old challenge protocol to BOLD. See [Governance Action Contracts](https://github.com/ArbitrumFoundation/governance/blob/main/src/gov-action-contracts/README.md)        | `AssertionNodeLib`, `GlobalStateLib`, `RollupLib` |
| src/rollup/AssertionState.sol                                        | 1               |            | 17       | Struct and utility functions for Assertion states        |                |
| src/rollup/Assertion.sol                                             | 1               |            | 52       | Structs and utility functions for Assertions        |                |
| src/challengeV2/libraries/UintUtilsLib.sol                           | 1               |            | 40       | Functions for getting the most and least significant bits of a `uint256` |                |
| src/challengeV2/libraries/MerkleTreeLib.sol                          | 1               |            | 118      | Functions for handling binary merkle trees | `MerkleLib`, `ArrayUtilsLib`, `UintUtilsLib` |
| src/challengeV2/libraries/Enums.sol                                  |                 |            | 10       | `EdgeStatus` and `EdgeType` enums |                |
| src/challengeV2/libraries/EdgeChallengeManagerLib.sol                | 1               |            | 396      | Main library for Challenge Manager logic | `UintUtilsLib`, `MerkleTreeLib`, `ChallengeEdgeLib`, `GlobalStateLib`, `AssertionStateLib` |
| src/challengeV2/libraries/ChallengeErrors.sol                        |                 |            | 54       | Challenge Manager errors |                |
| src/challengeV2/libraries/ChallengeEdgeLib.sol                       | 1               |            | 158      | Struct and utility functions for edges |                |
| src/challengeV2/libraries/ArrayUtilsLib.sol                          | 1               |            | 30       | Array utilities        |                |
| src/assertionStakingPool/interfaces/IEdgeStakingPoolCreator.sol      |                 | 1          | 5        | Interface for `EdgeStakingPoolCreator`        |                |
| src/assertionStakingPool/interfaces/IEdgeStakingPool.sol             |                 | 1          | 6        | Interface for `EdgeStakingPool`        |                |
| src/assertionStakingPool/interfaces/IAssertionStakingPoolCreator.sol |                 | 1          | 10       | Interface for `AssertionStakingPoolCreator`        |                |
| src/assertionStakingPool/interfaces/IAssertionStakingPool.sol        |                 | 1          | 5        | Interface for `AssertionStakingPool`        |                |
| src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol          |                 | 1          | 7        | Interface for `AbsBoldStakingPool`        |                |
| **Total:**                                                           |          **27** |     **14** | **3603** |         |                |

### Files out of scope

*See [out_of_scope.txt](https://github.com/code-423n4/2024-05-arbitrum-foundation/blob/main/out_of_scope.txt)*


## Scoping Q &amp; A

### General questions


| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |       Stake Token (Assumed standard behaviors)             |
| Test coverage                           |   74.43% (946/1271 lines, see more details below)      |
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
- Issues that the security council could fix are still in scope.


## All trusted roles in the protocol

| Role                                | Description                       |
| --------------------------------------- | ---------------------------- |
| `excessStakeReceiver`                          | will be set to a DAO controlled address. Used to reimburse honest validators |

 
Note: there is a “grace period” after assertions are confirmed via challenge. This is to ensure the result of the challenge is widely observable before it causes an assertion to be confirmed. The security council can act within this grace period in the event a bad assertion is confirmed.


## Describe any novel or unique curve logic or mathematical models implemented in the contracts:

N/A


## Running tests

```bash
git clone https://github.com/code-423n4/2024-05-arbitrum-foundation
cd 2024-05-arbitrum-foundation
git submodule update --init --recursive
yarn

forge test 
```
To run code coverage
```bash
# same as above, just for the last command:
forge coverage
```

## Coverage

*note that interfaces and contracts without logic (e.g. Error.sol, Config.sol) are excluded*

| File                                                     | % Lines            | % Statements       | % Branches        | % Funcs          |
|----------------------------------------------------------|--------------------|--------------------|-------------------|------------------|
| src/assertionStakingPool/AbsBoldStakingPool.sol          | 86.67% (13/15)     | 86.67% (13/15)     | 66.67% (4/6)      | 100.00% (4/4)    |
| src/assertionStakingPool/AssertionStakingPool.sol        | 77.78% (7/9)       | 77.78% (7/9)       | 100.00% (0/0)     | 80.00% (4/5)     |
| src/assertionStakingPool/AssertionStakingPoolCreator.sol | 100.00% (4/4)      | 100.00% (6/6)      | 100.00% (0/0)     | 100.00% (2/2)    |
| src/assertionStakingPool/EdgeStakingPool.sol             | 100.00% (7/7)      | 100.00% (9/9)      | 100.00% (2/2)     | 100.00% (2/2)    |
| src/assertionStakingPool/EdgeStakingPoolCreator.sol      | 100.00% (4/4)      | 100.00% (6/6)      | 100.00% (0/0)     | 100.00% (2/2)    |
| src/assertionStakingPool/StakingPoolCreatorUtils.sol     | 80.00% (4/5)       | 85.71% (6/7)       | 50.00% (1/2)      | 100.00% (1/1)    |
| src/bridge/DelayBuffer.sol                               | 95.65% (22/23)     | 88.57% (31/35)     | 100.00% (6/6)     | 83.33% (5/6)     |
| src/bridge/SequencerInbox.sol                            | 49.54% (108/218)   | 46.39% (148/319)   | 36.76% (50/136)   | 48.89% (22/45)   |
| src/challengeV2/EdgeChallengeManager.sol                 | 93.75% (105/112)   | 93.51% (144/154)   | 88.00% (44/50)    | 82.61% (19/23)   |
| src/challengeV2/libraries/ArrayUtilsLib.sol              | 100.00% (17/17)    | 100.00% (28/28)    | 100.00% (4/4)     | 100.00% (3/3)    |
| src/challengeV2/libraries/ChallengeEdgeLib.sol           | 98.00% (49/50)     | 98.53% (67/68)     | 96.67% (29/30)    | 100.00% (16/16)  |
| src/challengeV2/libraries/EdgeChallengeManagerLib.sol    | 97.92% (188/192)   | 98.31% (232/236)   | 94.32% (83/88)    | 100.00% (23/23)  |
| src/challengeV2/libraries/MerkleTreeLib.sol              | 98.68% (75/76)     | 98.99% (98/99)     | 87.10% (54/62)    | 100.00% (7/7)    |
| src/challengeV2/libraries/UintUtilsLib.sol               | 100.00% (26/26)    | 100.00% (29/29)    | 100.00% (20/20)   | 100.00% (2/2)    |
| src/rollup/Assertion.sol                                 | 100.00% (11/11)    | 100.00% (11/11)    | 100.00% (6/6)     | 100.00% (3/3)    |
| src/rollup/AssertionState.sol                            | 100.00% (2/2)      | 100.00% (4/4)      | 100.00% (0/0)     | 100.00% (2/2)    |
| src/rollup/BOLDUpgradeAction.sol                         | 0.00% (0/123)      | 0.00% (0/151)      | 0.00% (0/24)      | 0.00% (0/20)     |
| src/rollup/BridgeCreator.sol                             | 100.00% (23/23)    | 100.00% (26/26)    | 50.00% (1/2)      | 100.00% (5/5)    |
| src/rollup/RollupAdminLogic.sol                          | 58.43% (52/89)     | 60.20% (59/98)     | 27.27% (6/22)     | 39.13% (9/23)    |
| src/rollup/RollupCore.sol                                | 86.99% (107/123)   | 86.39% (127/147)   | 52.08% (25/48)    | 80.00% (24/30)   |
| src/rollup/RollupCreator.sol                             | 73.68% (42/57)     | 69.74% (53/76)     | 31.82% (7/22)     | 66.67% (4/6)     |
| src/rollup/RollupLib.sol                                 | 100.00% (6/6)      | 100.00% (7/7)      | 50.00% (1/2)      | 100.00% (4/4)    |
| src/rollup/RollupProxy.sol                               | 80.00% (4/5)       | 92.31% (12/13)     | 50.00% (1/2)      | 100.00% (1/1)    |
| src/rollup/RollupUserLogic.sol                           | 94.59% (70/74)     | 91.30% (84/92)     | 71.67% (43/60)    | 90.48% (19/21)   |
| Total                                       | 74.43% (946/1271) | 73.37% (1207/1645) | 65.15% (387/594) | 71.48% (183/256) |

## Miscellaneous
Employees of Arbitrum and employees' family members are ineligible to participate in this audit.



