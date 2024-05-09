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

*See [out_of_scope.txt](https://github.com/code-423n4/2024-04-renzo/blob/main/out_of_scope.txt)*

| File                                              |
|---------------------------------------------------|
| src/bridge/IEthBridge.sol                         |
| src/state/Value.sol                               |
| test/foundry/DelayBuffer.t.sol                    |
| src/osp/OneStepProver0.sol                        |
| test/foundry/Inbox.t.sol                          |
| src/bridge/IERC20Inbox.sol                        |
| src/bridge/ERC20Inbox.sol                         |
| src/mocks/SimpleProxy.sol                         |
| src/mocks/UpgradeExecutorMock.sol                 |
| test/foundry/SequencerInbox.t.sol                 |
| src/state/GlobalState.sol                         |
| src/osp/OneStepProverHostIo.sol                   |
| src/test-helpers/ValueArrayTester.sol             |
| src/precompiles/ArbSys.sol                        |
| test/stakingPool/EdgeStakingPool.t.sol            |
| src/libraries/IReader4844.sol                     |
| src/mocks/MerkleTreeAccess.sol                    |
| src/mocks/TestWETH9.sol                           |
| src/test-helpers/BridgeTester.sol                 |
| src/mocks/SimpleOneStepProofEntry.sol             |
| src/test-helpers/MessageTester.sol                |
| src/rollup/ERC20RollupEventInbox.sol              |
| src/mocks/SequencerInboxBlobMock.sol              |
| src/libraries/CryptographyPrimitives.sol          |
| src/bridge/Outbox.sol                             |
| src/bridge/AbsBridge.sol                          |
| test/challengeV2/EdgeChallengeManager.t.sol       |
| src/state/Machine.sol                             |
| src/bridge/AbsInbox.sol                           |
| src/mocks/ProxyAdminForBinding.sol                |
| src/bridge/IOwnable.sol                           |
| src/state/ValueStack.sol                          |
| src/precompiles/ArbOwner.sol                      |
| src/state/ModuleMemory.sol                        |
| src/precompiles/ArbosActs.sol                     |
| src/precompiles/ArbAddressTable.sol               |
| src/test-helpers/InterfaceCompatibilityTester.sol |
| test/challengeV2/UintUtilsLib.t.sol               |
| src/state/MerkleProof.sol                         |
| src/libraries/MessageTypes.sol                    |
| src/precompiles/ArbOwnerPublic.sol                |
| src/precompiles/ArbBLS.sol                        |
| src/test-helpers/RollupMock.sol                   |
| src/libraries/DelegateCallAware.sol               |
| test/challengeV2/ArrayUtilsLib.t.sol              |
| test/challengeV2/StateTools.sol                   |
| test/foundry/Bridge.t.sol                         |
| src/state/Instructions.sol                        |
| src/bridge/GasRefunder.sol                        |
| src/osp/IOneStepProofEntry.sol                    |
| src/test-helpers/EthVault.sol                     |
| src/test-helpers/CryptographyPrimitivesTester.sol |
| src/precompiles/ArbWasm.sol                       |
| src/osp/IOneStepProver.sol                        |
| src/rollup/ValidatorWalletCreator.sol             |
| test/foundry/ERC20Bridge.t.sol                    |
| src/precompiles/ArbosTest.sol                     |
| test/foundry/ERC20Inbox.t.sol                     |
| src/osp/OneStepProverMath.sol                     |
| src/precompiles/ArbFunctionTable.sol              |
| src/rollup/DeployHelper.sol                       |
| test/challengeV2/ChallengeEdgeLib.t.sol           |
| src/osp/OneStepProofEntry.sol                     |
| src/state/Module.sol                              |
| src/libraries/UUPSNotUpgradeable.sol              |
| test/foundry/AbsOutbox.t.sol                      |
| test/stakingPool/AbsBoldStakingPool.t.sol         |
| src/rollup/ValidatorWallet.sol                    |
| src/bridge/AbsOutbox.sol                          |
| src/libraries/AddressAliasHelper.sol              |
| src/mocks/BridgeStub.sol                          |
| src/bridge/IInboxBase.sol                         |
| src/rollup/IRollupEventInbox.sol                  |
| src/bridge/ERC20Outbox.sol                        |
| src/precompiles/ArbRetryableTx.sol                |
| test/foundry/AbsInbox.t.sol                       |
| test/foundry/ERC20Outbox.t.sol                    |
| test/Rollup.t.sol                                 |
| src/mocks/SequencerInboxStub.sol                  |
| test/challengeV2/Utils.sol                        |
| src/precompiles/ArbInfo.sol                       |
| src/bridge/ERC20Bridge.sol                        |
| src/bridge/IDelayedMessageProvider.sol            |
| src/rollup/ValidatorUtils.sol                     |
| src/state/PcArray.sol                             |
| src/precompiles/ArbDebug.sol                      |
| src/libraries/GasRefundEnabled.sol                |
| src/rollup/RollupEventInbox.sol                   |
| src/rollup/AbsRollupEventInbox.sol                |
| src/node-interface/NodeInterface.sol              |
| test/foundry/BridgeCreator.t.sol                  |
| test/ERC20Mock.sol                                |
| src/state/ModuleMemoryCompact.sol                 |
| src/osp/OneStepProverMemory.sol                   |
| test/MockAssertionChain.sol                       |
| src/libraries/AdminFallbackProxy.sol              |
| src/osp/HashProofHelper.sol                       |
| src/libraries/Constants.sol                       |
| src/libraries/DoubleLogicUUPSUpgradeable.sol      |
| src/precompiles/ArbAggregator.sol                 |
| src/bridge/Inbox.sol                              |
| src/mocks/InboxStub.sol                           |
| src/precompiles/ArbStatistics.sol                 |
| test/stakingPool/AssertionStakingPool.t.sol       |
| src/mocks/MockRollupEventInbox.sol                |
| src/test-helpers/TestToken.sol                    |
| src/state/MultiStack.sol                          |
| src/bridge/IERC20Bridge.sol                       |
| test/challengeV2/EdgeChallengeManagerLib.t.sol    |
| src/bridge/IBridge.sol                            |
| src/libraries/IGasRefunder.sol                    |
| src/mocks/Simple.sol                              |
| src/bridge/IOutbox.sol                            |
| src/state/ValueArray.sol                          |
| test/challengeV2/MerkleTreeLib.t.sol              |
| src/libraries/ArbitrumChecker.sol                 |
| src/state/StackFrame.sol                          |
| src/bridge/IInbox.sol                             |
| test/foundry/Outbox.t.sol                         |
| src/bridge/Bridge.sol                             |
| src/bridge/Messages.sol                           |
| src/precompiles/ArbGasInfo.sol                    |
| src/test-helpers/OutboxWithoutOptTester.sol       |
| src/libraries/MerkleLib.sol                       |
| src/node-interface/NodeInterfaceDebug.sol         |
| test/foundry/AbsBridge.t.sol                      |
| test/foundry/util/TestUtil.sol                    |
| src/state/Deserialize.sol                         |
| src/precompiles/ArbWasmCache.sol                  |
| src/mocks/BridgeUnproxied.sol                     |

## Scoping Q &amp; A

### General questions


| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |       None             |
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
- Issues that the security council could fix during the grace period are still in scope if they are the result of an incorrect resolution of the challenge protocol.


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



