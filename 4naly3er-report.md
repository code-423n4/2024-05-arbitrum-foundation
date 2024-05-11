# Report


## Gas Optimizations


| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings) | 19 |
| [GAS-2](#GAS-2) | Use assembly to check for `address(0)` | 25 |
| [GAS-3](#GAS-3) | Comparing to a Boolean constant | 1 |
| [GAS-4](#GAS-4) | Using bools for storage incurs overhead | 11 |
| [GAS-5](#GAS-5) | Cache array length outside of loop | 12 |
| [GAS-6](#GAS-6) | State variables should be cached in stack variables rather than re-reading them from storage | 17 |
| [GAS-7](#GAS-7) | Use calldata instead of memory for function arguments that do not get mutated | 12 |
| [GAS-8](#GAS-8) | For Operations that will not overflow, you could use unchecked | 274 |
| [GAS-9](#GAS-9) | Use Custom Errors instead of Revert Strings to save Gas | 72 |
| [GAS-10](#GAS-10) | State variables only set in the constructor should be declared `immutable` | 54 |
| [GAS-11](#GAS-11) | Functions guaranteed to revert when called by normal users can be marked `payable` | 12 |
| [GAS-12](#GAS-12) | `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`) | 15 |
| [GAS-13](#GAS-13) | Using `private` rather than `public` for constants, saves gas | 11 |
| [GAS-14](#GAS-14) | Use shift right/left instead of division/multiplication if possible | 1 |
| [GAS-15](#GAS-15) | Increments/decrements can be unchecked in for-loops | 14 |
| [GAS-16](#GAS-16) | Use != 0 instead of > 0 for unsigned integer comparison | 20 |
| [GAS-17](#GAS-17) | `internal` functions not called by the contract should be removed | 36 |
### <a name="GAS-1"></a>[GAS-1] `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings)
This saves **16 gas per instance.**

*Instances (19)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

34:         depositBalance[msg.sender] += amount;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/bridge/DelayBuffer.sol

46:         buffer += (elapsed * replenishRateInBasis) / BASIS;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/SequencerInbox.sol

771:             extraGas += l1Fees / block.basefee;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

493:             totalTimeUnrivaled += lowerTimer < upperTimer ? lowerTimer : upperTimer;

529:         totalTimeUnrivaled += store.edges[claimingEdgeId].totalTimeUnrivaledCache;

739:         totalTimeUnrivaled += claimedAssertionUnrivaledBlocks;

805:                 machineStep += stepSize * store.edges[cursor].startHeight;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

296:                 sum += 2 ** i;

339:             size += numLeaves;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

35:             msb += 128;

40:             msb += 64;

45:             msb += 32;

50:             msb += 16;

55:             msb += 8;

60:             msb += 4;

65:             msb += 2;

68:         if (x >= 0x2) msb += 1;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

74:             currentInboxCount += 1;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

347:         totalWithdrawableFunds += amount;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

### <a name="GAS-2"></a>[GAS-2] Use assembly to check for `address(0)`
*Saves 6 gas per instance*

*Instances (25)*:
```solidity
File: src/bridge/SequencerInbox.sol

189:             if (feeToken != address(0)) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

317:         if (address(_assertionChain) == address(0)) {

321:         if (address(_oneStepProofEntry) == address(0)) {

331:         if (_excessStakeReceiver == address(0)) {

429:         if (address(st) != address(0) && sa != 0) {

580:         if (address(st) != address(0) && sa != 0) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

103:         if (staker == address(0)) {

259:         return edge.claimId != 0 && edge.staker != address(0);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

490:         if (store.edges[edgeId].lowerChildId != bytes32(0)) {

665:         if (confirmedRivalId != bytes32(0)) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/rollup/BridgeCreator.sol

112:             nativeToken == address(0) ? ethBasedTemplates : erc20BasedTemplates,

117:         if (nativeToken == address(0)) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

57:         require(config.loserStakeEscrow != address(0), "INVALID_ESCROW_0");

272:         require(newLoserStakerEscrow != address(0), "INVALID_ESCROW_0");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

127:         require(assertionHash != bytes32(0), "ASSERTION_ID_CANNOT_BE_ZERO");

478:             newAssertionHash == expectedAssertionHash || expectedAssertionHash == bytes32(0),

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

228:         if (deployParams.batchPosterManager != address(0)) {

292:         if (_nativeToken == address(0)) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupProxy.sol

16:             _getAdmin() == address(0) &&

17:             _getImplementation() == address(0) &&

18:             _getSecondaryImplementation() == address(0)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

28:         require(_stakeToken != address(0), "NEED_STAKE_TOKEN");

170:             expectedAssertionHash == bytes32(0)

278:         require(expectedAssertionHash != bytes32(0), "EXPECTED_ASSERTION_HASH");

337:         require(withdrawalAddress != address(0), "EMPTY_WITHDRAWAL_ADDRESS");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="GAS-3"></a>[GAS-3] Comparing to a Boolean constant
Comparing to a constant (`true` or `false`) is a bit more expensive than directly checking the returned boolean value.

Consider using `if(directValue)` instead of `if(directValue == true)` and `if(!directValue)` instead of `if(directValue == false)`

*Instances (1)*:
```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

271:         if (edge.refunded == true) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

### <a name="GAS-4"></a>[GAS-4] Using bools for storage incurs overhead
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

*Instances (11)*:
```solidity
File: src/bridge/SequencerInbox.sol

95:     mapping(address => bool) public isBatchPoster;

115:     mapping(address => bool) public isSequencer;

131:     bool internal immutable hostChainIsArbitrum = ArbitrumChecker.runningOnArbitrum();

133:     bool public immutable isUsingFeeToken;

135:     bool public immutable isDelayBufferable;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

86:     mapping(address => mapping(bytes32 => bool)) hasMadeLayerZeroRival;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

204:     bool public immutable DISABLE_VALIDATOR_WHITELIST;

207:     bool public immutable IS_DELAY_BUFFERABLE;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupCore.sol

96:     mapping(address => bool) public isValidator;

108:     bool public validatorWhitelistDisabled;

112:     bool internal immutable _hostChainIsArbitrum = ArbitrumChecker.runningOnArbitrum();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

### <a name="GAS-5"></a>[GAS-5] Cache array length outside of loop
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (12)*:
```solidity
File: src/challengeV2/EdgeChallengeManager.sol

492:         for (uint256 i = 0; i < edgeIds.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

15:         for (uint256 i = 0; i < arr.length; i++) {

47:         for (uint256 i = 0; i < arr1.length; i++) {

50:         for (uint256 i = 0; i < arr2.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

115:         for (uint256 i = 0; i < me.length; i++) {

188:         for (uint256 i = 0; i < me.length; i++) {

294:         for (uint256 i = 0; i < me.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

528:             for (uint256 i = 0; i < validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

184:         for (uint256 i = 0; i < _validator.length; i++) {

230:         for (uint256 i = 0; i < staker.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

225:         for (uint256 i = 0; i < deployParams.batchPosters.length; i++) {

235:             for (uint256 i = 0; i < deployParams.validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="GAS-6"></a>[GAS-6] State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*Saves 100 gas per instance*

*Instances (17)*:
```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

45:         IRollupUser(rollup).newStakeOnNewAssertion(requiredStake, assertionInputs, assertionHash, address(this));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

46:         IERC20(stakeToken).safeIncreaseAllowance(address(challengeManager), requiredStake);

47:         bytes32 newEdgeId = EdgeChallengeManager(challengeManager).createLayerZeroEdge(args);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

```solidity
File: src/bridge/SequencerInbox.sol

210:             revert NotOwner(msg.sender, IOwnable(rollup).owner());

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

418:             args, ard, oneStepProofEntry, expectedEndHeight, NUM_BIGSTEP_LEVEL, whitelistEnabled

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

357:                 IOldRollupAdmin(address(OLD_ROLLUP)).forceRefundStaker(stakersToRefund);

362:         DoubleLogicUUPSUpgradeable(address(OLD_ROLLUP)).upgradeSecondaryTo(IMPL_PATCHED_OLD_ROLLUP_USER);

417:         IBridge(BRIDGE).updateRollupAddress(IOwnable(newRollupAddress));

426:         IRollupEventInbox(REI).updateRollupAddress();

430:         IOutbox(OUTBOX).updateRollupAddress();

449:             PROXY_ADMIN_SEQUENCER_INBOX.upgrade(sequencerInbox, IMPL_SEQUENCER_INBOX);

458:             ISequencerInbox(SEQ_INBOX).isDelayBufferable() == IS_DELAY_BUFFERABLE,

461:         ISequencerInbox(SEQ_INBOX).updateRollupAddress();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupCore.sol

510:             wasmModuleRoot,

511:             baseStake,

512:             address(challengeManager),

513:             confirmPeriodBlocks

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

### <a name="GAS-7"></a>[GAS-7] Use calldata instead of memory for function arguments that do not get mutated
When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly bypasses this loop. 

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it's still more gas-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one. 

 *Saves 60 gas per instance*

*Instances (12)*:
```solidity
File: src/bridge/ISequencerInbox.sol

247:     function setMaxTimeVariation(MaxTimeVariation memory maxTimeVariation_) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

```solidity
File: src/bridge/SequencerInbox.sol

161:     function postUpgradeInit(BufferConfig memory bufferConfig_)

180:         BufferConfig memory bufferConfig_

885:     function setMaxTimeVariation(ISequencerInbox.MaxTimeVariation memory maxTimeVariation_)

947:     function setBufferConfig(BufferConfig memory bufferConfig_) external onlyRollupOwner {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

78:     function forceRefundStaker(address[] memory stacker) external;

84:     function postUpgradeInit(BufferConfig memory bufferConfig_) external;

464:     function perform(address[] memory validators) external {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/IRollupAdmin.sol

54:     function setValidator(address[] memory _validator, bool[] memory _val) external;

80:     function forceRefundStaker(address[] memory stacker) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupAdmin.sol)

```solidity
File: src/rollup/RollupCreator.sol

137:     function createRollup(RollupDeploymentParams memory deployParams)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupProxy.sol

12:     function initializeProxy(Config memory config, ContractDependencies memory connectedContracts)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

### <a name="GAS-8"></a>[GAS-8] For Operations that will not overflow, you could use unchecked

*Instances (274)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

7: import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

8: import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

9: import {IAbsBoldStakingPool} from "./interfaces/IAbsBoldStakingPool.sol";

34:         depositBalance[msg.sender] += amount;

50:         depositBalance[msg.sender] = balance - amount;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

8: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

9: import "../rollup/IRollupLogic.sol";

10: import "./AbsBoldStakingPool.sol";

11: import "./interfaces/IAssertionStakingPool.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPoolCreator.sol

8: import "./AssertionStakingPool.sol";

9: import "./StakingPoolCreatorUtils.sol";

10: import "./interfaces/IAssertionStakingPoolCreator.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

7: import "./AbsBoldStakingPool.sol";

8: import "./interfaces/IEdgeStakingPool.sol";

9: import "../challengeV2/EdgeChallengeManager.sol";

11: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

12: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPoolCreator.sol

8: import "./EdgeStakingPool.sol";

9: import "./StakingPoolCreatorUtils.sol";

10: import "./interfaces/IEdgeStakingPoolCreator.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

8: import "@openzeppelin/contracts/utils/Create2.sol";

9: import "@openzeppelin/contracts/utils/Address.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/assertionStakingPool/interfaces/IAssertionStakingPool.sol

7: import "../../rollup/IRollupLogic.sol";

8: import "./IAbsBoldStakingPool.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/interfaces/IAssertionStakingPoolCreator.sol

7: import "../../rollup/IRollupLogic.sol";

8: import "./IAssertionStakingPool.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAssertionStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/interfaces/IEdgeStakingPool.sol

7: import "../../challengeV2/EdgeChallengeManager.sol";

8: import "./IAbsBoldStakingPool.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IEdgeStakingPool.sol)

```solidity
File: src/assertionStakingPool/interfaces/IEdgeStakingPoolCreator.sol

7: import "./IEdgeStakingPool.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IEdgeStakingPoolCreator.sol)

```solidity
File: src/bridge/DelayBuffer.sol

7: import "./Messages.sol";

8: import "./DelayBufferTypes.sol";

42:         uint256 elapsed = end > start ? end - start : 0;

43:         uint256 delay = sequenced > start ? sequenced - start : 0;

46:         buffer += (elapsed * replenishRateInBasis) / BASIS;

48:         uint256 unexpectedDelay = delay > threshold ? delay - threshold : 0;

55:             buffer -= unexpectedDelay;

105:         return block.number - self.prevBlockNumber <= self.threshold;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/DelayBufferTypes.sol

5: import "./Messages.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBufferTypes.sol)

```solidity
File: src/bridge/ISequencerInbox.sol

9: import "../libraries/IGasRefunder.sol";

10: import "./IDelayedMessageProvider.sol";

11: import "./IBridge.sol";

12: import "./Messages.sol";

13: import "./DelayBufferTypes.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

```solidity
File: src/bridge/SequencerInbox.sol

39: } from "../libraries/Error.sol";

40: import "./IBridge.sol";

41: import "./IInboxBase.sol";

42: import "./ISequencerInbox.sol";

43: import "../rollup/IRollupLogic.sol";

44: import "./Messages.sol";

45: import "../precompiles/ArbGasInfo.sol";

46: import "../precompiles/ArbSys.sol";

47: import "../libraries/IReader4844.sol";

49: import {L1MessageType_batchPostingReport} from "../libraries/MessageTypes.sol";

50: import "../libraries/DelegateCallAware.sol";

51: import {IGasRefunder} from "../libraries/IGasRefunder.sol";

52: import {GasRefundEnabled} from "../libraries/GasRefundEnabled.sol";

53: import "../libraries/ArbitrumChecker.sol";

54: import {IERC20Bridge} from "./IERC20Bridge.sol";

55: import "./DelayBuffer.sol";

225:             bounds.minTimestamp = uint64(block.timestamp) - delaySeconds_;

227:         bounds.maxTimestamp = uint64(block.timestamp) + futureSeconds_;

229:             bounds.minBlockNumber = uint64(block.number) - delayBlocks_;

231:         bounds.maxBlockNumber = uint64(block.number) + futureBlocks_;

301:             _totalDelayedMessagesRead - 1,

314:         if (l1BlockAndTime[0] + delayBlocks_ >= block.number) revert ForceIncludeBlockTooSoon();

319:             prevDelayedAcc = bridge.delayedInboxAccs(_totalDelayedMessagesRead - 2);

322:             bridge.delayedInboxAccs(_totalDelayedMessagesRead - 1) !=

331:         uint256 newSeqMsgCount = prevSeqMsgCount; // force inclusion should not modify sequencer message count

693:         uint256 fullDataLen = HEADER_LENGTH + data.length;

742:         uint256 blobCost = reader4844.getBlobBaseFee() * GAS_PER_BLOB * dataHashes.length;

746:             block.basefee > 0 ? blobCost / block.basefee : 0

771:             extraGas += l1Fees / block.basefee;

839:         return blockNumber + _delayBlocks;

906:         if (keysetBytes.length >= 64 * 1024) revert KeysetTooLarge();

938:         emit OwnerFunctionCalled(4); // Owner in this context can also be batch poster manager

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

7: import "../rollup/Assertion.sol";

8: import "./libraries/UintUtilsLib.sol";

9: import "./IAssertionChain.sol";

10: import "./libraries/EdgeChallengeManagerLib.sol";

11: import "../libraries/Constants.sol";

12: import "../state/Machine.sol";

14: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

15: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

360:         if (_numBigStepLevel + 2 != _stakeAmounts.length) {

361:             revert StakeAmountsMismatch(_stakeAmounts.length, _numBigStepLevel + 2);

492:         for (uint256 i = 0; i < edgeIds.length; i++) {

530:                 - assertionChain.getFirstChildCreationBlock(claimStateData.prevAssertionHash);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/IAssertionChain.sol

7: import "../bridge/IBridge.sol";

8: import "../osp/IOneStepProofEntry.sol";

9: import "../rollup/Assertion.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/IAssertionChain.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

14:         bytes32[] memory clone = new bytes32[](arr.length + 1);

15:         for (uint256 i = 0; i < arr.length; i++) {

18:         clone[clone.length - 1] = newItem;

35:         bytes32[] memory newArr = new bytes32[](endIndex - startIndex);

36:         for (uint256 i = startIndex; i < endIndex; i++) {

37:             newArr[i - startIndex] = arr[i];

46:         bytes32[] memory full = new bytes32[](arr1.length + arr2.length);

47:         for (uint256 i = 0; i < arr1.length; i++) {

50:         for (uint256 i = 0; i < arr2.length; i++) {

51:             full[arr1.length + i] = arr2[i];

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

7: import "./Enums.sol";

8: import "./ChallengeErrors.sol";

229:         uint256 len = edge.endHeight - edge.startHeight;

284:         } else if (level == numBigStepLevels + 1) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/ChallengeErrors.sol

7: import "./Enums.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeErrors.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

7: import "./UintUtilsLib.sol";

8: import "./MerkleTreeLib.sol";

9: import "./ChallengeEdgeLib.sol";

10: import "../../osp/IOneStepProofEntry.sol";

11: import "../../rollup/AssertionState.sol";

12: import "../../libraries/Constants.sol";

13: import "./ChallengeErrors.sol";

334:         uint256 y = x - 1;

384:             startHistoryRoot, 1, args.endHistoryRoot, args.endHeight + 1, preExpansion, preProof

493:             totalTimeUnrivaled += lowerTimer < upperTimer ? lowerTimer : upperTimer;

529:         totalTimeUnrivaled += store.edges[claimingEdgeId].totalTimeUnrivaledCache;

552:             return block.number - store.edges[edgeId].createdAtBlock;

565:                 return firstRivalCreatedAtBlock - edgeCreatedAtBlock;

577:         if (end - start < 2) {

580:         if (end - start == 2) {

581:             return start + 1;

584:         uint256 diff = (end - 1) ^ start;

587:         return ((end - 1) & mask);

625:                 bisectionHistoryRoot, middleHeight + 1, ce.endHistoryRoot, ce.endHeight + 1, preExpansion, proof

675:         uint8 nextLevel = level + 1;

739:         totalTimeUnrivaled += claimedAssertionUnrivaledBlocks;

805:                 machineStep += stepSize * store.edges[cursor].startHeight;

807:                 stepSize *= bigStepHeight;

822:             store.edges[edgeId].endHistoryRoot, afterHash, machineStep + 1, afterHistoryInclusionProof

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

7: import "../../libraries/MerkleLib.sol";

8: import "./ArrayUtilsLib.sol";

9: import "./UintUtilsLib.sol";

115:         for (uint256 i = 0; i < me.length; i++) {

125:                     if (i != me.length - 1) {

163:             bytes32[] memory empty = new bytes32[](level + 1);

174:         uint256 postSize = meSize + 2 ** level;

179:             ? new bytes32[](me.length + 1)

188:         for (uint256 i = 0; i < me.length; i++) {

223:             next[next.length - 1] = accumHash;

228:         require(next[next.length - 1] != 0, "Last entry zero");

269:         uint256 mask = (1 << (msb) + 1) - 1;

294:         for (uint256 i = 0; i < me.length; i++) {

296:                 sum += 2 ** i;

320:         require(preSize > 0, "Pre-size cannot be 0");

339:             size += numLeaves;

341:             proofIndex++;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

18:         uint256 isolated = ((x - 1) & x) ^ x;

35:             msb += 128;

40:             msb += 64;

45:             msb += 32;

50:             msb += 16;

55:             msb += 8;

60:             msb += 4;

65:             msb += 2;

68:         if (x >= 0x2) msb += 1;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/Assertion.sol

7: import "./AssertionState.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Assertion.sol)

```solidity
File: src/rollup/AssertionState.sol

7: import "../state/GlobalState.sol";

8: import "../state/Machine.sol";

9: import "../osp/IOneStepProofEntry.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/AssertionState.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

7: import "@openzeppelin/contracts-upgradeable/utils/Create2Upgradeable.sol";

8: import "@openzeppelin/contracts/proxy/transparent/ProxyAdmin.sol";

9: import "./RollupProxy.sol";

10: import "./RollupLib.sol";

11: import "./RollupAdminLogic.sol";

45:     uint64 currentChallenge; // 1. cannot have current challenge

46:     bool isStaked; // 2. must be staked

350:         for (uint64 i = 0; i < stakerCount; i++) {

393:             owner: address(this), // upgrade executor is the owner

394:             loserStakeEscrow: L1_TIMELOCK, // additional funds get sent to the l1 timelock

396:             chainConfig: "", // we can use an empty chain config it wont be used in the rollup initialization because we check if the rei is already connected there

476:                     address(PROXY_ADMIN_BRIDGE), // use the same proxy admin as the bridge

528:             for (uint256 i = 0; i < validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

7: import "../bridge/Bridge.sol";

8: import "../bridge/SequencerInbox.sol";

9: import "../bridge/Inbox.sol";

10: import "../bridge/Outbox.sol";

11: import "./RollupEventInbox.sol";

12: import "../bridge/ERC20Bridge.sol";

13: import "../bridge/ERC20Inbox.sol";

14: import "../rollup/ERC20RollupEventInbox.sol";

15: import "../bridge/ERC20Outbox.sol";

17: import "../bridge/IBridge.sol";

18: import "@openzeppelin/contracts/access/Ownable.sol";

19: import "@openzeppelin/contracts/proxy/transparent/ProxyAdmin.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/Config.sol

7: import "../state/GlobalState.sol";

8: import "../state/Machine.sol";

9: import "../bridge/ISequencerInbox.sol";

10: import "../bridge/IBridge.sol";

11: import "../bridge/IOutbox.sol";

12: import "../bridge/IInboxBase.sol";

13: import "./IRollupEventInbox.sol";

14: import "./IRollupLogic.sol";

15: import "../challengeV2/EdgeChallengeManager.sol";

48:     address rollupAdminLogic; // this cannot be IRollupAdmin because of circular dependencies

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Config.sol)

```solidity
File: src/rollup/IRollupAdmin.sol

7: import "./IRollupCore.sol";

8: import "../bridge/ISequencerInbox.sol";

9: import "../bridge/IOutbox.sol";

10: import "../bridge/IOwnable.sol";

11: import "./Config.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupAdmin.sol)

```solidity
File: src/rollup/IRollupCore.sol

7: import "./Assertion.sol";

8: import "../bridge/IBridge.sol";

9: import "../bridge/IOutbox.sol";

10: import "../bridge/IInboxBase.sol";

11: import "./IRollupEventInbox.sol";

12: import "../challengeV2/EdgeChallengeManager.sol";

13: import "../challengeV2/IAssertionChain.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupCore.sol)

```solidity
File: src/rollup/IRollupLogic.sol

7: import "./IRollupCore.sol";

8: import "../bridge/ISequencerInbox.sol";

9: import "../bridge/IOutbox.sol";

10: import "../bridge/IOwnable.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

7: import "./IRollupAdmin.sol";

8: import "./IRollupLogic.sol";

9: import "./RollupCore.sol";

10: import "../bridge/IOutbox.sol";

11: import "../bridge/ISequencerInbox.sol";

12: import "../libraries/DoubleLogicUUPSUpgradeable.sol";

13: import "@openzeppelin/contracts/proxy/beacon/UpgradeableBeacon.sol";

74:             currentInboxCount += 1;

184:         for (uint256 i = 0; i < _validator.length; i++) {

230:         for (uint256 i = 0; i < staker.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

7: import "@openzeppelin/contracts-upgradeable/security/PausableUpgradeable.sol";

9: import "./Assertion.sol";

10: import "./RollupLib.sol";

11: import "./IRollupEventInbox.sol";

12: import "./IRollupCore.sol";

14: import "../state/Machine.sol";

16: import "../bridge/ISequencerInbox.sol";

17: import "../bridge/IBridge.sol";

18: import "../bridge/IOutbox.sol";

19: import "../challengeV2/EdgeChallengeManager.sol";

20: import "../libraries/ArbitrumChecker.sol";

289:         uint256 finalStaked = initialStaked + amountAdded;

305:         uint256 amountWithdrawn = current - target;

334:         totalWithdrawableFunds -= amount;

345:         uint256 finalWithdrawable = initialWithdrawable + amount;

347:         totalWithdrawableFunds += amount;

359:         _stakerList[stakerIndex] = _stakerList[_stakerList.length - 1];

458:                 nextInboxPosition = currentInboxPosition + 1;

471:             sequencerBatchAcc = bridge.sequencerInboxAccs(afterInboxPosition - 1);

489:             prevAssertion.firstChildBlock == 0, // assumes block 0 is impossible

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

7: import "./RollupProxy.sol";

8: import "./IRollupAdmin.sol";

9: import "./BridgeCreator.sol";

10: import "@offchainlabs/upgrade-executor/src/IUpgradeExecutor.sol";

11: import "@openzeppelin/contracts/proxy/transparent/ProxyAdmin.sol";

12: import "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol";

13: import "@openzeppelin/contracts/access/Ownable.sol";

14: import {DeployHelper} from "./DeployHelper.sol";

15: import {SafeERC20, IERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

225:         for (uint256 i = 0; i < deployParams.batchPosters.length; i++) {

235:             for (uint256 i = 0; i < deployParams.validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupLib.sol

7: import "../state/GlobalState.sol";

8: import "../bridge/ISequencerInbox.sol";

10: import "../bridge/IBridge.sol";

11: import "../bridge/IOutbox.sol";

12: import "../bridge/IInboxBase.sol";

13: import "./Assertion.sol";

14: import "./IRollupEventInbox.sol";

15: import "../challengeV2/EdgeChallengeManager.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupLib.sol)

```solidity
File: src/rollup/RollupProxy.sol

7: import "../libraries/AdminFallbackProxy.sol";

8: import "./IRollupAdmin.sol";

9: import "./Config.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

7: import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

9: import {IRollupUser} from "./IRollupLogic.sol";

10: import "../libraries/UUPSNotUpgradeable.sol";

11: import "./RollupCore.sol";

12: import "./IRollupLogic.sol";

13: import {ETH_POS_BLOCK_TIME} from "../libraries/Constants.sol";

57:             return latestConfirmedAssertion.firstChildBlock + VALIDATOR_AFK_BLOCKS < block.number;

59:         return latestConfirmedAssertion.createdAtBlock + VALIDATOR_AFK_BLOCKS < block.number;

110:         require(block.number >= assertion.createdAtBlock + prevConfig.confirmPeriodBlocks, "BEFORE_DEADLINE");

125:                 block.number >= winningEdge.confirmedAtBlock + challengeGracePeriodBlocks,

205:             uint256 timeSincePrev = block.number - getAssertionStorage(prevAssertion).createdAtBlock;

304:             bridge.sequencerInboxAccs(assertion.afterState.globalState.getInboxPosition() - 1)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="GAS-9"></a>[GAS-9] Use Custom Errors instead of Revert Strings to save Gas
Custom errors are available from solidity version 0.8.4. Custom errors save [**~50 gas**](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

Additionally, custom errors can be used inside and outside of contracts (including interfaces and libraries).

Source: <https://blog.soliditylang.org/2021/04/21/custom-errors/>:

> Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4), there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Consider replacing **all revert strings** with custom errors in the solution, and particularly those that have multiple occurrences:

*Instances (72)*:
```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

32:         require(startIndex < endIndex, "Start not less than end");

33:         require(endIndex <= arr.length, "End not less or equal than length");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

111:         require(me.length > 0, "Empty merkle expansion");

112:         require(me.length <= MAX_LEVEL, "Merkle expansion too large");

158:         require(level < MAX_LEVEL, "Level too high");

159:         require(subtreeRoot != 0, "Cannot append empty subtree");

160:         require(me.length <= MAX_LEVEL, "Merkle expansion too large");

170:         require(level < me.length, "Level greater than highest level of current expansion");

183:         require(next.length <= MAX_LEVEL, "Append creates oversize tree");

195:                 require(me[i] == 0, "Append above least significant bit");

228:         require(next[next.length - 1] != 0, "Last entry zero");

265:         require(startSize < endSize, "Start not less than end");

287:         revert("Both y and z cannot be zero");

320:         require(preSize > 0, "Pre-size cannot be 0");

321:         require(root(preExpansion) == preRoot, "Pre expansion root mismatch");

322:         require(treeSize(preExpansion) == preSize, "Pre size does not match expansion");

323:         require(preSize < postSize, "Pre size not less than post size");

335:             require(proofIndex < proof.length, "Index out of range");

345:         require(root(exp) == postRoot, "Post expansion root not equal post");

349:         require(proofIndex == proof.length, "Incomplete proof usage");

364:         require(rootHash == calculatedRoot, "Invalid inclusion proof");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

15:         require(x > 0, "Zero has no significant bits");

30:         require(x != 0, "Zero has no significant bits");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/Assertion.sol

94:         require(self.status != AssertionStatus.NoAssertion, "ASSERTION_NOT_EXIST");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Assertion.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

107:         require(h == stateHash(executionState, inboxMaxCount), "Invalid hash");

114:         require(inboxMaxCount != 0, "Hash not yet set");

517:         require(address(rollup) == expectedRollupAddress, "UNEXPCTED_ROLLUP_ADDR");

529:                 require(ROLLUP_READER.isValidator(validators[i]), "UNEXPECTED_NEW_VALIDATOR");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

57:         require(config.loserStakeEscrow != address(0), "INVALID_ESCROW_0");

128:         require(_outbox != address(outbox), "CUR_OUTBOX");

181:         require(_validator.length > 0, "EMPTY_ARRAY");

182:         require(_validator.length == _val.length, "WRONG_LENGTH");

214:         require(newConfirmPeriod > 0, "INVALID_CONFIRM_PERIOD");

229:         require(staker.length > 0, "EMPTY_ARRAY");

272:         require(newLoserStakerEscrow != address(0), "INVALID_ESCROW_0");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

127:         require(assertionHash != bytes32(0), "ASSERTION_ID_CANNOT_BE_ZERO");

148:             require(blockNum > 0, "NO_ASSERTION");

244:         require(assertion.status == AssertionStatus.Pending, "NOT_PENDING");

304:         require(target <= current, "TOO_LITTLE_STAKE");

357:         require(staker.isStaked, "NOT_STAKED");

399:         require(assertion.beforeState.machineStatus == MachineStatus.FINISHED, "BAD_PREV_STATUS");

424:             require(afterGS.comparePositions(beforeGS) >= 0, "INBOX_BACKWARDS");

427:             require(afterStateCmpMaxInbox <= 0, "INBOX_TOO_FAR");

433:                 require(afterGS.comparePositions(beforeGS) > 0, "OVERFLOW_STANDSTILL");

438:             require(afterGS.comparePositionsAgainstStartOfBatch(currentInboxPosition) <= 0, "INBOX_PAST_END");

466:             require(afterInboxPosition != 0, "EMPTY_INBOX_COUNT");

485:         require(getAssertionStorage(newAssertionHash).status == AssertionStatus.NoAssertion, "ASSERTION_SEEN");

546:         require(assertionHash == RollupLib.assertionHash(prevAssertionHash, state, inboxAcc), "INVALID_ASSERTION_HASH");

566:         require(isStaked(stakerAddress), "NOT_STAKED");

573:         require(isLatestConfirmed || haveChild, "STAKE_ACTIVE");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

160:             require(deployParams.maxDataSize == ethInbox.maxDataSize(), "I_MAX_DATA_SIZE_MISMATCH");

305:             require(sent, "Refund failed");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

21:         require(isValidator[msg.sender] || validatorWhitelistDisabled, "NOT_VALIDATOR");

28:         require(_stakeToken != address(0), "NEED_STAKE_TOKEN");

63:         require(!validatorWhitelistDisabled, "WHITELIST_DISABLED");

64:         require(_chainIdChanged(), "CHAIN_ID_NOT_CHANGED");

72:         require(!validatorWhitelistDisabled, "WHITELIST_DISABLED");

73:         require(_validatorIsAfk(), "VALIDATOR_NOT_AFK");

110:         require(block.number >= assertion.createdAtBlock + prevConfig.confirmPeriodBlocks, "BEFORE_DEADLINE");

113:         require(prevAssertionHash == latestConfirmed(), "PREV_NOT_LATEST_CONFIRMED");

118:             require(winningEdge.claimId == assertionHash, "NOT_WINNER");

119:             require(winningEdge.status == EdgeStatus.Confirmed, "EDGE_NOT_CONFIRMED");

120:             require(winningEdge.confirmedAtBlock != 0, "ZERO_CONFIRMED_AT_BLOCK");

139:         require(!isStaked(msg.sender), "ALREADY_STAKED");

175:         require(isStaked(msg.sender), "NOT_STAKED");

182:         require(amountStaked(msg.sender) >= assertion.beforeStateData.configData.requiredStake, "INSUFFICIENT_STAKE");

207:             require(timeSincePrev >= minimumAssertionPeriod, "TIME_DELTA");

233:         require(isStaked(stakerAddress), "NOT_STAKED");

258:         require(msg.sender == anyTrustFastConfirmer, "NOT_FAST_CONFIRMER");

278:         require(expectedAssertionHash != bytes32(0), "EXPECTED_ASSERTION_HASH");

337:         require(withdrawalAddress != address(0), "EMPTY_WITHDRAWAL_ADDRESS");

360:         require(amount > 0, "NO_FUNDS_TO_WITHDRAW");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="GAS-10"></a>[GAS-10] State variables only set in the constructor should be declared `immutable`
Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around **20 000 gas** per variable) and replace the expensive storage-reading operations (around **2100 gas** per reading) to a less expensive value reading (**3 gas**)

*Instances (54)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

25:         stakeToken = _stakeToken;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

35:         rollup = _rollup;

36:         assertionHash = _assertionHash;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

39:         challengeManager = _challengeManager;

40:         edgeId = _edgeId;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

```solidity
File: src/bridge/SequencerInbox.sol

146:         maxDataSize = _maxDataSize;

152:         reader4844 = reader4844_;

153:         isUsingFeeToken = _isUsingFeeToken;

154:         isDelayBufferable = _isDelayBufferable;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

127:         rollup = _rollup;

127:         rollup = _rollup;

170:         _array = __array;

170:         _array = __array;

293:         L1_TIMELOCK = contracts.l1Timelock;

294:         OLD_ROLLUP = contracts.rollup;

295:         BRIDGE = contracts.bridge;

296:         SEQ_INBOX = contracts.sequencerInbox;

297:         REI = contracts.rollupEventInbox;

298:         OUTBOX = contracts.outbox;

299:         INBOX = contracts.inbox;

300:         OSP = contracts.osp;

302:         PROXY_ADMIN_OUTBOX = ProxyAdmin(proxyAdmins.outbox);

303:         PROXY_ADMIN_BRIDGE = ProxyAdmin(proxyAdmins.bridge);

304:         PROXY_ADMIN_REI = ProxyAdmin(proxyAdmins.rei);

305:         PROXY_ADMIN_SEQUENCER_INBOX = ProxyAdmin(proxyAdmins.seqInbox);

306:         PROXY_ADMIN_INBOX = ProxyAdmin(proxyAdmins.inbox);

307:         PREIMAGE_LOOKUP = new StateHashPreImageLookup();

308:         ROLLUP_READER = new RollupReader(contracts.rollup);

310:         IMPL_BRIDGE = implementations.bridge;

311:         IMPL_SEQUENCER_INBOX = implementations.seqInbox;

312:         IMPL_INBOX = implementations.inbox;

313:         IMPL_REI = implementations.rei;

314:         IMPL_OUTBOX = implementations.outbox;

315:         IMPL_PATCHED_OLD_ROLLUP_USER = implementations.oldRollupUser;

316:         IMPL_NEW_ROLLUP_USER = implementations.newRollupUser;

317:         IMPL_NEW_ROLLUP_ADMIN = implementations.newRollupAdmin;

318:         IMPL_CHALLENGE_MANAGER = implementations.challengeManager;

320:         CHAIN_ID = settings.chainId;

321:         CONFIRM_PERIOD_BLOCKS = settings.confirmPeriodBlocks;

322:         CHALLENGE_PERIOD_BLOCKS = settings.challengePeriodBlocks;

323:         STAKE_TOKEN = settings.stakeToken;

324:         STAKE_AMOUNT = settings.stakeAmt;

325:         MINI_STAKE_AMOUNTS_STORAGE = address(new ConstantArrayStorage(settings.miniStakeAmounts));

326:         ANY_TRUST_FAST_CONFIRMER = settings.anyTrustFastConfirmer;

327:         DISABLE_VALIDATOR_WHITELIST = settings.disableValidatorWhitelist;

328:         BLOCK_LEAF_SIZE = settings.blockLeafSize;

329:         BIGSTEP_LEAF_SIZE = settings.bigStepLeafSize;

330:         SMALLSTEP_LEAF_SIZE = settings.smallStepLeafSize;

331:         NUM_BIGSTEP_LEVEL = settings.numBigStepLevel;

332:         CHALLENGE_GRACE_PERIOD_BLOCKS = settings.challengeGracePeriodBlocks;

333:         IS_DELAY_BUFFERABLE = settings.isDelayBufferable;

334:         MAX = settings.bufferConfig.max;

335:         THRESHOLD = settings.bufferConfig.threshold;

336:         REPLENISH_RATE_IN_BASIS = settings.bufferConfig.replenishRateInBasis;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

### <a name="GAS-11"></a>[GAS-11] Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (12)*:
```solidity
File: src/bridge/SequencerInbox.sol

903:     function setValidKeyset(bytes calldata keysetBytes) external onlyRollupOwner {

922:     function invalidateKeysetHash(bytes32 ksHash) external onlyRollupOwner {

942:     function setBatchPosterManager(address newBatchPosterManager) external onlyRollupOwner {

947:     function setBufferConfig(BufferConfig memory bufferConfig_) external onlyRollupOwner {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/rollup/BridgeCreator.sol

53:     function updateTemplates(BridgeTemplates calldata _newTemplates) external onlyOwner {

58:     function updateERC20Templates(BridgeTemplates calldata _newTemplates) external onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

27:     function initialize(address _stakeToken) external view override onlyProxy {

137:     function _newStake(uint256 depositAmount, address withdrawalAddress) internal onlyValidator whenNotPaused {

222:     function returnOldDeposit() external override onlyValidator whenNotPaused {

232:     function _addToDeposit(address stakerAddress, uint256 depositAmount) internal onlyValidator whenNotPaused {

241:     function reduceDeposit(uint256 target) external onlyValidator whenNotPaused {

349:     function addToDeposit(address stakerAddress, uint256 tokenAmount) external onlyValidator whenNotPaused {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="GAS-12"></a>[GAS-12] `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`)
Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name *post-increment*:

```solidity
uint i = 1;  
uint j = 2;
require(j == i++, "This will be false as i is incremented after the comparison");
```
  
However, pre-increments (or pre-decrements) return the new value:
  
```solidity
uint i = 1;  
uint j = 2;
require(j == ++i, "This will be true as i is incremented before the comparison");
```

In the pre-increment case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`.

Consider using pre-increments and pre-decrements where they are relevant (meaning: not where post-increments/decrements logic are relevant).

*Saves 5 gas per instance*

*Instances (15)*:
```solidity
File: src/challengeV2/EdgeChallengeManager.sol

492:         for (uint256 i = 0; i < edgeIds.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

15:         for (uint256 i = 0; i < arr.length; i++) {

36:         for (uint256 i = startIndex; i < endIndex; i++) {

47:         for (uint256 i = 0; i < arr1.length; i++) {

50:         for (uint256 i = 0; i < arr2.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

115:         for (uint256 i = 0; i < me.length; i++) {

188:         for (uint256 i = 0; i < me.length; i++) {

294:         for (uint256 i = 0; i < me.length; i++) {

341:             proofIndex++;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

350:         for (uint64 i = 0; i < stakerCount; i++) {

528:             for (uint256 i = 0; i < validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

184:         for (uint256 i = 0; i < _validator.length; i++) {

230:         for (uint256 i = 0; i < staker.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

225:         for (uint256 i = 0; i < deployParams.batchPosters.length; i++) {

235:             for (uint256 i = 0; i < deployParams.validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="GAS-13"></a>[GAS-13] Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*Instances (11)*:
```solidity
File: src/bridge/DelayBuffer.sol

17:     uint256 public constant BASIS = 10000;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/SequencerInbox.sol

70:     uint256 public constant HEADER_LENGTH = 40;

73:     bytes1 public constant DATA_AUTHENTICATED_FLAG = 0x40;

76:     bytes1 public constant DATA_BLOB_HEADER_FLAG = DATA_AUTHENTICATED_FLAG | 0x10;

79:     bytes1 public constant DAS_MESSAGE_HEADER_FLAG = 0x80;

82:     bytes1 public constant TREE_DAS_MESSAGE_HEADER_FLAG = 0x08;

85:     bytes1 public constant BROTLI_MESSAGE_HEADER_FLAG = 0x00;

88:     bytes1 public constant ZERO_HEAVY_MESSAGE_HEADER_FLAG = 0x20;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

135:     bytes32 public constant UNRIVALED = keccak256(abi.encodePacked("UNRIVALED"));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

104:     uint256 public constant MAX_LEVEL = 64;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

49:     uint256 public constant VALIDATOR_AFK_BLOCKS = 201600;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="GAS-14"></a>[GAS-14] Use shift right/left instead of division/multiplication if possible
While the `DIV` / `MUL` opcode uses 5 gas, the `SHR` / `SHL` opcode only uses 3 gas. Furthermore, beware that Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting. Eventually, overflow checks are never performed for shift operations as they are done for arithmetic operations. Instead, the result is always truncated, so the calculation can be unchecked in Solidity version `0.8+`
- Use `>> 1` instead of `/ 2`
- Use `>> 2` instead of `/ 4`
- Use `<< 3` instead of `* 8`
- ...
- Use `>> 5` instead of `/ 2^5 == / 32`
- Use `<< 6` instead of `* 2^6 == * 64`

TL;DR:
- Shifting left by N is like multiplying by 2^N (Each bits to the left is an increased power of 2)
- Shifting right by N is like dividing by 2^N (Each bits to the right is a decreased power of 2)

*Saves around 2 gas + 20 for unchecked per instance*

*Instances (1)*:
```solidity
File: src/bridge/SequencerInbox.sol

906:         if (keysetBytes.length >= 64 * 1024) revert KeysetTooLarge();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

### <a name="GAS-15"></a>[GAS-15] Increments/decrements can be unchecked in for-loops
In Solidity 0.8+, there's a default overflow check on unsigned integers. It's possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

[ethereum/solidity#10695](https://github.com/ethereum/solidity/issues/10695)

The change would be:

```diff
- for (uint256 i; i < numIterations; i++) {
+ for (uint256 i; i < numIterations;) {
 // ...  
+   unchecked { ++i; }
}  
```

These save around **25 gas saved** per instance.

The same can be applied with decrements (which should use `break` when `i == 0`).

The risk of overflow is non-existent for `uint256`.

*Instances (14)*:
```solidity
File: src/challengeV2/EdgeChallengeManager.sol

492:         for (uint256 i = 0; i < edgeIds.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

15:         for (uint256 i = 0; i < arr.length; i++) {

36:         for (uint256 i = startIndex; i < endIndex; i++) {

47:         for (uint256 i = 0; i < arr1.length; i++) {

50:         for (uint256 i = 0; i < arr2.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

115:         for (uint256 i = 0; i < me.length; i++) {

188:         for (uint256 i = 0; i < me.length; i++) {

294:         for (uint256 i = 0; i < me.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

350:         for (uint64 i = 0; i < stakerCount; i++) {

528:             for (uint256 i = 0; i < validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

184:         for (uint256 i = 0; i < _validator.length; i++) {

230:         for (uint256 i = 0; i < staker.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

225:         for (uint256 i = 0; i < deployParams.batchPosters.length; i++) {

235:             for (uint256 i = 0; i < deployParams.validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="GAS-16"></a>[GAS-16] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (20)*:
```solidity
File: src/bridge/DelayBufferTypes.sol

7: pragma solidity >=0.6.9 <0.9.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBufferTypes.sol)

```solidity
File: src/bridge/ISequencerInbox.sol

6: pragma solidity >=0.6.9 <0.9.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

```solidity
File: src/bridge/SequencerInbox.sol

702:         if (data.length > 0) {

746:             block.basefee > 0 ? blobCost / block.basefee : 0

818:         if (calldataLengthPosted > 0 && !isUsingFeeToken) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

412:                 assertionChain.getSecondChildCreationBlock(claimStateData.prevAssertionHash) > 0,

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

445:         while (edge.level > 0) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

111:         require(me.length > 0, "Empty merkle expansion");

320:         require(preSize > 0, "Pre-size cannot be 0");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

15:         require(x > 0, "Zero has no significant bits");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

181:         require(_validator.length > 0, "EMPTY_ARRAY");

214:         require(newConfirmPeriod > 0, "INVALID_CONFIRM_PERIOD");

229:         require(staker.length > 0, "EMPTY_ARRAY");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

148:             require(blockNum > 0, "NO_ASSERTION");

433:                 require(afterGS.comparePositions(beforeGS) > 0, "OVERFLOW_STANDSTILL");

572:         bool haveChild = getAssertionStorage(lastestAssertion).firstChildBlock > 0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

56:         if (latestConfirmedAssertion.firstChildBlock > 0) {

115:         if (prevAssertion.secondChildBlock > 0) {

196:             lastAssertion == prevAssertion || getAssertionStorage(lastAssertion).firstChildBlock > 0,

360:         require(amount > 0, "NO_FUNDS_TO_WITHDRAW");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="GAS-17"></a>[GAS-17] `internal` functions not called by the contract should be removed
If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

*Instances (36)*:
```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

13:     function getPool(bytes memory creationCode, bytes memory args) internal view returns (address) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/bridge/DelayBuffer.sol

68:     function update(BufferData storage self, uint64 blockNumber) internal {

108:     function isUpdatable(BufferData storage self) internal view returns (bool) {

115:     function isValidBufferConfig(BufferConfig memory config) internal pure returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

13:     function append(bytes32[] memory arr, bytes32 newItem) internal pure returns (bytes32[] memory) {

27:     function slice(bytes32[] memory arr, uint256 startIndex, uint256 endIndex)

45:     function concat(bytes32[] memory arr1, bytes32[] memory arr2) internal pure returns (bytes32[] memory) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

93:     function newLayerZeroEdge(

133:     function newChildEdge(

180:     function mutualId(ChallengeEdge storage ce) internal view returns (bytes32) {

184:     function mutualIdMem(ChallengeEdge memory ce) internal pure returns (bytes32) {

208:     function idMem(ChallengeEdge memory edge) internal pure returns (bytes32) {

215:     function id(ChallengeEdge storage edge) internal view returns (bytes32) {

222:     function exists(ChallengeEdge storage edge) internal view returns (bool) {

228:     function length(ChallengeEdge storage edge) internal view returns (uint256) {

239:     function setChildren(ChallengeEdge storage edge, bytes32 lowerChildId, bytes32 upperChildId) internal {

249:     function setConfirmed(ChallengeEdge storage edge) internal {

264:     function setRefunded(ChallengeEdge storage edge) internal {

279:     function levelToType(uint8 level, uint8 numBigStepLevels) internal pure returns (EdgeType eType) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

411:     function createLayerZeroEdge(

443:     function getPrevAssertionHash(EdgeStore storage store, bytes32 edgeId) internal view returns (bytes32) {

516:     function updateTimerCacheByChildren(EdgeStore storage store, bytes32 edgeId) internal returns (bool, uint256) {

520:     function updateTimerCacheByClaim(

604:     function bisectEdge(EdgeStore storage store, bytes32 edgeId, bytes32 bisectionHistoryRoot, bytes memory prefixProof)

723:     function confirmEdgeByTime(

766:     function confirmEdgeByOneStepProof(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

238:     function appendLeaf(bytes32[] memory me, bytes32 leaf) internal pure returns (bytes32[] memory) {

312:     function verifyPrefixProof(

359:     function verifyInclusionProof(bytes32 rootHash, bytes32 leaf, uint256 index, bytes32[] memory proof)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

14:     function leastSignificantBit(uint256 x) internal pure returns (uint256 msb) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/Assertion.sol

70:     function createAssertion(

85:     function childCreated(AssertionNode storage self) internal {

93:     function requireExists(AssertionNode memory self) internal pure {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Assertion.sol)

```solidity
File: src/rollup/AssertionState.sol

18:     function toExecutionState(AssertionState memory state) internal pure returns (ExecutionState memory) {

22:     function hash(AssertionState memory state) internal pure returns (bytes32) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/AssertionState.sol)

```solidity
File: src/rollup/RollupLib.sol

73:     function validateConfigHash(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupLib.sol)


## Non Critical Issues


| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Missing checks for `address(0)` when assigning values to address state variables | 4 |
| [NC-2](#NC-2) | Array indices should be referenced via `enum`s rather than via numeric literals | 8 |
| [NC-3](#NC-3) | `require()` should be used instead of `assert()` | 2 |
| [NC-4](#NC-4) | Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked` | 16 |
| [NC-5](#NC-5) | Constants should be in CONSTANT_CASE | 4 |
| [NC-6](#NC-6) | `constant`s should be defined rather than using magic numbers | 28 |
| [NC-7](#NC-7) | Control structures do not follow the Solidity Style Guide | 78 |
| [NC-8](#NC-8) | Critical Changes Should Use Two-step Procedure | 2 |
| [NC-9](#NC-9) | Consider disabling `renounceOwnership()` | 3 |
| [NC-10](#NC-10) | Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function | 20 |
| [NC-11](#NC-11) | Events should use parameters to convey information | 3 |
| [NC-12](#NC-12) | Event missing indexed field | 3 |
| [NC-13](#NC-13) | Events that mark critical parameter changes should contain both the old and the new value | 27 |
| [NC-14](#NC-14) | Function ordering does not follow the Solidity style guide | 11 |
| [NC-15](#NC-15) | Functions should not be longer than 50 lines | 295 |
| [NC-16](#NC-16) | Interfaces should be defined in separate files from their usage | 5 |
| [NC-17](#NC-17) | Lack of checks in setters | 24 |
| [NC-18](#NC-18) | Lines are too long | 1 |
| [NC-19](#NC-19) | Missing Event for critical parameters change | 3 |
| [NC-20](#NC-20) | NatSpec is completely non-existent on functions that should have them | 38 |
| [NC-21](#NC-21) | Incomplete NatSpec: `@param` is missing on actually documented functions | 6 |
| [NC-22](#NC-22) | Incomplete NatSpec: `@return` is missing on actually documented functions | 2 |
| [NC-23](#NC-23) | File's first line is not an SPDX Identifier | 39 |
| [NC-24](#NC-24) | Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor | 23 |
| [NC-25](#NC-25) | Consider using named mappings | 14 |
| [NC-26](#NC-26) | Adding a `return` statement when the function defines a named return variable, is redundant | 4 |
| [NC-27](#NC-27) | Take advantage of Custom Error's return value property | 66 |
| [NC-28](#NC-28) | Avoid the use of sensitive terms | 24 |
| [NC-29](#NC-29) | Contract does not follow the Solidity style guide's suggested layout ordering | 8 |
| [NC-30](#NC-30) | Use Underscores for Number Literals (add an underscore every 3 digits) | 3 |
| [NC-31](#NC-31) | Internal and private variables and functions names should begin with an underscore | 105 |
| [NC-32](#NC-32) | Event is missing `indexed` fields | 17 |
| [NC-33](#NC-33) | Constants should be defined rather than using magic numbers | 10 |
| [NC-34](#NC-34) | `public` functions not called by the contract should be declared `external` instead | 24 |
| [NC-35](#NC-35) | Variables need not be initialized to zero | 18 |
### <a name="NC-1"></a>[NC-1] Missing checks for `address(0)` when assigning values to address state variables

*Instances (4)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

25:         stakeToken = _stakeToken;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

35:         rollup = _rollup;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

39:         challengeManager = _challengeManager;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

```solidity
File: src/bridge/SequencerInbox.sol

943:         batchPosterManager = newBatchPosterManager;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

### <a name="NC-2"></a>[NC-2] Array indices should be referenced via `enum`s rather than via numeric literals

*Instances (8)*:
```solidity
File: src/bridge/SequencerInbox.sol

299:             l1BlockAndTime[0],

300:             l1BlockAndTime[1],

310:             buffer.update(l1BlockAndTime[0]);

314:         if (l1BlockAndTime[0] + delayBlocks_ >= block.number) revert ForceIncludeBlockTooSoon();

705:             if (!isValidCallDataFlag(data[0])) revert InvalidHeaderFlag(data[0]);

711:             if (data[0] & DAS_MESSAGE_HEADER_FLAG != 0 && data.length >= 33) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

355:                 stakersToRefund[0] = stakerAddr;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupCreator.sol

281:         executors[0] = rollupOwner;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="NC-3"></a>[NC-3] `require()` should be used instead of `assert()`
Prior to solidity version 0.8.0, hitting an assert consumes the **remainder of the transaction's available gas** rather than returning it, as `require()`/`revert()` do. `assert()` should be avoided even past solidity version 0.8.0 as its [documentation](https://docs.soliditylang.org/en/v0.8.14/control-structures.html#panic-via-assert-and-error-via-require) states that "The assert function creates an error of type Panic(uint256). ... Properly functioning code should never create a Panic, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix. Additionally, a require statement (or a custom error) are more friendly in terms of understanding what happened."

*Instances (2)*:
```solidity
File: src/bridge/SequencerInbox.sol

651:         assert(header.length == HEADER_LENGTH);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

340:             assert(size <= postSize);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

### <a name="NC-4"></a>[NC-4] Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked`
Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

Solidity version 0.8.12 introduces `string.concat()` (vs `abi.encodePacked(<str>,<str>), which catches concatenation errors (in the event of a `bytes` data mixed in the concatenation)`)

*Instances (16)*:
```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

14:         bytes32 bytecodeHash = keccak256(abi.encodePacked(creationCode, args));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/bridge/SequencerInbox.sol

643:         bytes memory header = abi.encodePacked(

744:             keccak256(bytes.concat(header, DATA_BLOB_HEADER_FLAG, abi.encodePacked(dataHashes))),

774:         bytes memory spendingReportMsg = abi.encodePacked(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

173:         return keccak256(abi.encodePacked(level, originId, startHeight, startHistoryRoot, endHeight));

198:             abi.encodePacked(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

135:     bytes32 public constant UNRIVALED = keccak256(abi.encodePacked("UNRIVALED"));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

126:                         accum = keccak256(abi.encodePacked(accum, bytes32(0)));

132:                 accum = keccak256(abi.encodePacked(val, accum));

135:                 accum = keccak256(abi.encodePacked(accum, bytes32(0)));

213:                         accumHash = keccak256(abi.encodePacked(me[i], accumHash));

241:         return appendCompleteSubTree(me, 0, keccak256(abi.encodePacked(leaf)));

363:         bytes32 calculatedRoot = MerkleLib.calculateRoot(proof, index, keccak256(abi.encodePacked(leaf)));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

103:             keccak256(abi.encodePacked(executionState.globalState.hash(), inboxMaxCount, executionState.machineStatus));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupLib.sol

45:                 abi.encodePacked(

63:                 abi.encodePacked(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupLib.sol)

### <a name="NC-5"></a>[NC-5] Constants should be in CONSTANT_CASE
For `constant` variable names, each word should use all capital letters, with underscores separating each word (CONSTANT_CASE)

*Instances (4)*:
```solidity
File: src/bridge/SequencerInbox.sol

129:     uint256 internal immutable deployTimeChainId = block.chainid;

131:     bool internal immutable hostChainIsArbitrum = ArbitrumChecker.runningOnArbitrum();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/rollup/RollupCore.sol

112:     bool internal immutable _hostChainIsArbitrum = ArbitrumChecker.runningOnArbitrum();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

31:     uint256 internal immutable deployTimeChainId = block.chainid;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-6"></a>[NC-6] `constant`s should be defined rather than using magic numbers
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*Instances (28)*:
```solidity
File: src/bridge/SequencerInbox.sol

319:             prevDelayedAcc = bridge.delayedInboxAccs(_totalDelayedMessagesRead - 2);

711:             if (data[0] & DAS_MESSAGE_HEADER_FLAG != 0 && data.length >= 33) {

905:         bytes32 ksHash = bytes32(ksWord ^ (1 << 255));

906:         if (keysetBytes.length >= 64 * 1024) revert KeysetTooLarge();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

355:         if (_numBigStepLevel > 253) {

360:         if (_numBigStepLevel + 2 != _stakeAmounts.length) {

361:             revert StakeAmountsMismatch(_stakeAmounts.length, _numBigStepLevel + 2);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

577:         if (end - start < 2) {

580:         if (end - start == 2) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

174:         uint256 postSize = meSize + 2 ** level;

296:                 sum += 2 ** i;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

34:             x >>= 128;

35:             msb += 128;

39:             x >>= 64;

40:             msb += 64;

44:             x >>= 32;

45:             msb += 32;

49:             x >>= 16;

50:             msb += 16;

54:             x >>= 8;

55:             msb += 8;

59:             x >>= 4;

60:             msb += 4;

64:             x >>= 2;

65:             msb += 2;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

347:         if (stakerCount > 50) {

348:             stakerCount = 50;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

52:         minimumAssertionPeriod = 75;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

### <a name="NC-7"></a>[NC-7] Control structures do not follow the Solidity Style Guide
See the [control structures](https://docs.soliditylang.org/en/latest/style-guide.html#control-structures) section of the Solidity Style Guide

*Instances (78)*:
```solidity
File: src/bridge/SequencerInbox.sol

104:         if (msg.sender != rollup.owner()) revert NotOwner(msg.sender, rollup.owner());

148:             if (reader4844_ != IReader4844(address(0))) revert DataBlobsNotSupported();

150:             if (reader4844_ == IReader4844(address(0))) revert InitParamZero("Reader4844");

166:         if (!isDelayBufferable) revert NotDelayBufferable();

182:         if (bridge != IBridge(address(0))) revert AlreadyInit();

183:         if (bridge_ == IBridge(address(0))) revert HadZeroInit();

209:         if (msg.sender != IOwnable(rollup).owner())

212:         if (rollup == newRollup) revert RollupNotChanged();

237:         if (!_chainIdChanged()) revert NotForked();

295:         if (_totalDelayedMessagesRead <= totalDelayedMessagesRead) revert DelayedBackwards();

314:         if (l1BlockAndTime[0] + delayBlocks_ >= block.number) revert ForceIncludeBlockTooSoon();

321:         if (

331:         uint256 newSeqMsgCount = prevSeqMsgCount; // force inclusion should not modify sequencer message count

375:         if (msg.sender != tx.origin) revert NotOrigin();

376:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

377:         if (isDelayProofRequired(afterDelayedMessagesRead)) revert DelayProofRequired();

397:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

398:         if (isDelayProofRequired(afterDelayedMessagesRead)) revert DelayProofRequired();

417:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

418:         if (!isDelayBufferable) revert NotDelayBufferable();

440:         if (msg.sender != tx.origin) revert NotOrigin();

441:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

442:         if (!isDelayBufferable) revert NotDelayBufferable();

501:         if (hostChainIsArbitrum) revert DataBlobsNotSupported();

568:         if (!isBatchPoster[msg.sender] && msg.sender != address(rollup)) revert NotBatchPoster();

569:         if (isDelayProofRequired(afterDelayedMessagesRead)) revert DelayProofRequired();

591:         if (!isBatchPoster[msg.sender] && msg.sender != address(rollup)) revert NotBatchPoster();

592:         if (!isDelayBufferable) revert NotDelayBufferable();

614:                 if (

694:         if (fullDataLen > maxDataSize) revert DataTooLarge(fullDataLen, maxDataSize);

705:             if (!isValidCallDataFlag(data[0])) revert InvalidHeaderFlag(data[0]);

714:                 if (!dasKeySetInfo[dasKeysetHash].isValidKeyset) revert NoSuchKeyset(dasKeysetHash);

736:         if (dataHashes.length == 0) revert MissingDataHashes();

773:         if (extraGas > type(uint64).max) revert ExtraGasNotUint64();

806:         if (afterDelayedMessagesRead < totalDelayedMessagesRead) revert DelayedBackwards();

807:         if (afterDelayedMessagesRead > bridge.delayedMessageCount()) revert DelayedTooFar();

848:         if (!isDelayBufferable) revert NotDelayBufferable();

849:         if (!DelayBuffer.isValidBufferConfig(bufferConfig_)) revert BadBufferConfig();

870:         if (

906:         if (keysetBytes.length >= 64 * 1024) revert KeysetTooLarge();

908:         if (dasKeySetInfo[ksHash].isValidKeyset) revert AlreadyValidDASKeyset(ksHash);

923:         if (!dasKeySetInfo[ksHash].isValidKeyset) revert NoSuchKeyset(ksHash);

959:         if (ksInfo.creationBlock == 0) revert NoSuchKeyset(ksHash);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

388:                 revert EmptyEdgeSpecificProof();

494:             if (updated) emit TimerCacheUpdated(edgeIds[i], newValue);

501:         if (updated) emit TimerCacheUpdated(edgeId, newValue);

507:         if (updated) emit TimerCacheUpdated(edgeId, newValue);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/ChallengeErrors.sol

22: error EmptyEdgeSpecificProof();

46: error HeightDiffLtTwo(uint256 h1, uint256 h2);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeErrors.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

213:     function layerZeroTypeSpecificChecks(

249:                 revert EmptyEdgeSpecificProof();

295:                 revert EmptyEdgeSpecificProof();

308:             MerkleTreeLib.verifyInclusionProof(

318:             MerkleTreeLib.verifyInclusionProof(

371:         MerkleTreeLib.verifyInclusionProof(

383:         MerkleTreeLib.verifyPrefixProof(

422:             layerZeroTypeSpecificChecks(store, args, ard, oneStepProofEntry, numBigStepLevel);

578:             revert HeightDiffLtTwo(start, end);

584:         uint256 diff = (end - 1) ^ start;

585:         uint256 mostSignificantSharedBit = UintUtilsLib.mostSignificantBit(diff);

586:         uint256 mask = type(uint256).max << mostSignificantSharedBit;

624:             MerkleTreeLib.verifyPrefixProof(

812:         MerkleTreeLib.verifyInclusionProof(

821:         MerkleTreeLib.verifyInclusionProof(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

178:         bytes32[] memory next = UintUtilsLib.mostSignificantBit(postSize) > UintUtilsLib.mostSignificantBit(meSize)

195:                 require(me[i] == 0, "Append above least significant bit");

268:         uint256 msb = UintUtilsLib.mostSignificantBit(startSize ^ endSize);

276:             return UintUtilsLib.leastSignificantBit(y);

283:             return UintUtilsLib.mostSignificantBit(z);

312:     function verifyPrefixProof(

359:     function verifyInclusionProof(bytes32 rootHash, bytes32 leaf, uint256 index, bytes32[] memory proof)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

15:         require(x > 0, "Zero has no significant bits");

21:         return mostSignificantBit(isolated);

30:         require(x != 0, "Zero has no significant bits");

68:         if (x >= 0x2) msb += 1;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

396:             chainConfig: "", // we can use an empty chain config it wont be used in the rollup initialization because we check if the rei is already connected there

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupProxy.sol

15:         if (

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

53:         if (latestConfirmedAssertion.createdAtBlock == 0) return false;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-8"></a>[NC-8] Critical Changes Should Use Two-step Procedure
The critical procedures should be two step process.

See similar findings in previous Code4rena contests for reference: <https://code4rena.com/reports/2022-06-illuminate/#2-critical-changes-should-use-two-step-procedure>

**Recommended Mitigation Steps**

Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

*Instances (2)*:
```solidity
File: src/rollup/IRollupAdmin.sol

60:     function setOwner(address newOwner) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupAdmin.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

195:     function setOwner(address newOwner) external override {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

### <a name="NC-9"></a>[NC-9] Consider disabling `renounceOwnership()`
If the plan for your project does not include eventually giving up all ownership control, consider overwriting OpenZeppelin's `Ownable`'s `renounceOwnership()` function in order to disable it.

*Instances (3)*:
```solidity
File: src/rollup/BridgeCreator.sol

21: contract BridgeCreator is Ownable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/IRollupLogic.sol

12: interface IRollupUser is IRollupCore, IOwnable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

17: contract RollupCreator is Ownable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="NC-10"></a>[NC-10] Duplicated `require()`/`revert()` Checks Should Be Refactored To A Modifier Or Function

*Instances (20)*:
```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

112:         require(me.length <= MAX_LEVEL, "Merkle expansion too large");

160:         require(me.length <= MAX_LEVEL, "Merkle expansion too large");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

15:         require(x > 0, "Zero has no significant bits");

30:         require(x != 0, "Zero has no significant bits");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

57:         require(config.loserStakeEscrow != address(0), "INVALID_ESCROW_0");

181:         require(_validator.length > 0, "EMPTY_ARRAY");

229:         require(staker.length > 0, "EMPTY_ARRAY");

272:         require(newLoserStakerEscrow != address(0), "INVALID_ESCROW_0");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

357:         require(staker.isStaked, "NOT_STAKED");

566:         require(isStaked(stakerAddress), "NOT_STAKED");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

152:             require(

156:             require(

160:             require(deployParams.maxDataSize == ethInbox.maxDataSize(), "I_MAX_DATA_SIZE_MISMATCH");

170:             require(

174:             require(

178:             require(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

63:         require(!validatorWhitelistDisabled, "WHITELIST_DISABLED");

72:         require(!validatorWhitelistDisabled, "WHITELIST_DISABLED");

175:         require(isStaked(msg.sender), "NOT_STAKED");

233:         require(isStaked(stakerAddress), "NOT_STAKED");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-11"></a>[NC-11] Events should use parameters to convey information
For example, rather than using `event Paused()` and `event Unpaused()`, use `event PauseState(address indexed whoChangedIt, bool wasPaused, bool isNowPaused)`

*Instances (3)*:
```solidity
File: src/rollup/BridgeCreator.sol

25:     event TemplatesUpdated();

26:     event ERC20TemplatesUpdated();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/RollupCreator.sol

33:     event TemplatesUpdated();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="NC-12"></a>[NC-12] Event missing indexed field
Index event fields make the field more quickly accessible [to off-chain tools](https://ethereum.stackexchange.com/questions/40396/can-somebody-please-explain-the-concept-of-event-indexing) that parse events. This is especially useful when it comes to filtering based on an address. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Where applicable, each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three applicable fields, all of the applicable fields should be indexed.

*Instances (3)*:
```solidity
File: src/rollup/BOLDUpgradeAction.sol

97:     event HashSet(bytes32 h, ExecutionState executionState, uint256 inboxMaxCount);

235:     event RollupMigrated(address rollup, address challengeManager);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/IRollupCore.sol

24:     event RollupInitialized(bytes32 machineHash, uint256 chainId);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupCore.sol)

### <a name="NC-13"></a>[NC-13] Events that mark critical parameter changes should contain both the old and the new value
This should especially be done if the new value is not required to be different from the old value

*Instances (27)*:
```solidity
File: src/bridge/SequencerInbox.sol

885:     function setMaxTimeVariation(ISequencerInbox.MaxTimeVariation memory maxTimeVariation_)
             external
             onlyRollupOwner
         {
             _setMaxTimeVariation(maxTimeVariation_);
             emit OwnerFunctionCalled(0);

894:     function setIsBatchPoster(address addr, bool isBatchPoster_)
             external
             onlyRollupOwnerOrBatchPosterManager
         {
             isBatchPoster[addr] = isBatchPoster_;
             emit OwnerFunctionCalled(1);

903:     function setValidKeyset(bytes calldata keysetBytes) external onlyRollupOwner {
             uint256 ksWord = uint256(keccak256(bytes.concat(hex"fe", keccak256(keysetBytes))));
             bytes32 ksHash = bytes32(ksWord ^ (1 << 255));
             if (keysetBytes.length >= 64 * 1024) revert KeysetTooLarge();
     
             if (dasKeySetInfo[ksHash].isValidKeyset) revert AlreadyValidDASKeyset(ksHash);
             uint256 creationBlock = block.number;
             if (hostChainIsArbitrum) {
                 creationBlock = ArbSys(address(100)).arbBlockNumber();
             }
             dasKeySetInfo[ksHash] = DasKeySetInfo({
                 isValidKeyset: true,
                 creationBlock: uint64(creationBlock)
             });
             emit SetValidKeyset(ksHash, keysetBytes);

903:     function setValidKeyset(bytes calldata keysetBytes) external onlyRollupOwner {
             uint256 ksWord = uint256(keccak256(bytes.concat(hex"fe", keccak256(keysetBytes))));
             bytes32 ksHash = bytes32(ksWord ^ (1 << 255));
             if (keysetBytes.length >= 64 * 1024) revert KeysetTooLarge();
     
             if (dasKeySetInfo[ksHash].isValidKeyset) revert AlreadyValidDASKeyset(ksHash);
             uint256 creationBlock = block.number;
             if (hostChainIsArbitrum) {
                 creationBlock = ArbSys(address(100)).arbBlockNumber();
             }
             dasKeySetInfo[ksHash] = DasKeySetInfo({
                 isValidKeyset: true,
                 creationBlock: uint64(creationBlock)
             });
             emit SetValidKeyset(ksHash, keysetBytes);
             emit OwnerFunctionCalled(2);

933:     function setIsSequencer(address addr, bool isSequencer_)
             external
             onlyRollupOwnerOrBatchPosterManager
         {
             isSequencer[addr] = isSequencer_;
             emit OwnerFunctionCalled(4); // Owner in this context can also be batch poster manager

942:     function setBatchPosterManager(address newBatchPosterManager) external onlyRollupOwner {
             batchPosterManager = newBatchPosterManager;
             emit OwnerFunctionCalled(5);

947:     function setBufferConfig(BufferConfig memory bufferConfig_) external onlyRollupOwner {
             _setBufferConfig(bufferConfig_);
             emit OwnerFunctionCalled(6);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

499:     function updateTimerCacheByChildren(bytes32 edgeId) public {
             (bool updated, uint256 newValue) = store.updateTimerCacheByChildren(edgeId);
             if (updated) emit TimerCacheUpdated(edgeId, newValue);

505:     function updateTimerCacheByClaim(bytes32 edgeId, bytes32 claimingEdgeId) public {
             (bool updated, uint256 newValue) = store.updateTimerCacheByClaim(edgeId, claimingEdgeId, NUM_BIGSTEP_LEVEL);
             if (updated) emit TimerCacheUpdated(edgeId, newValue);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

106:     function set(bytes32 h, ExecutionState calldata executionState, uint256 inboxMaxCount) public {
             require(h == stateHash(executionState, inboxMaxCount), "Invalid hash");
             preImages[h] = abi.encode(executionState, inboxMaxCount);
             emit HashSet(h, executionState, inboxMaxCount);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

53:     function updateTemplates(BridgeTemplates calldata _newTemplates) external onlyOwner {
            ethBasedTemplates = _newTemplates;
            emit TemplatesUpdated();

58:     function updateERC20Templates(BridgeTemplates calldata _newTemplates) external onlyOwner {
            erc20BasedTemplates = _newTemplates;
            emit ERC20TemplatesUpdated();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

117:     function setOutbox(IOutbox _outbox) external override {
             outbox = _outbox;
             bridge.setOutbox(address(_outbox), true);
             emit OwnerFunctionCalled(0);

138:     function setDelayedInbox(address _inbox, bool _enabled) external override {
             bridge.setDelayedInbox(address(_inbox), _enabled);
             emit OwnerFunctionCalled(2);

180:     function setValidator(address[] calldata _validator, bool[] calldata _val) external override {
             require(_validator.length > 0, "EMPTY_ARRAY");
             require(_validator.length == _val.length, "WRONG_LENGTH");
     
             for (uint256 i = 0; i < _validator.length; i++) {
                 isValidator[_validator[i]] = _val[i];
             }
             emit OwnerFunctionCalled(6);

195:     function setOwner(address newOwner) external override {
             _changeAdmin(newOwner);
             emit OwnerFunctionCalled(7);

204:     function setMinimumAssertionPeriod(uint256 newPeriod) external override {
             minimumAssertionPeriod = newPeriod;
             emit OwnerFunctionCalled(8);

213:     function setConfirmPeriodBlocks(uint64 newConfirmPeriod) external override {
             require(newConfirmPeriod > 0, "INVALID_CONFIRM_PERIOD");
             confirmPeriodBlocks = newConfirmPeriod;
             emit OwnerFunctionCalled(9);

223:     function setBaseStake(uint256 newBaseStake) external override {
             baseStake = newBaseStake;
             emit OwnerFunctionCalled(12);

269:     function setLoserStakeEscrow(address newLoserStakerEscrow) external override {
             // loser stake is now sent directly to loserStakeEscrow, it must not
             // be address(0) because some token do not allow transfers to address(0)
             require(newLoserStakerEscrow != address(0), "INVALID_ESCROW_0");
             loserStakeEscrow = newLoserStakerEscrow;
             emit OwnerFunctionCalled(25);

281:     function setWasmModuleRoot(bytes32 newWasmModuleRoot) external override {
             wasmModuleRoot = newWasmModuleRoot;
             emit OwnerFunctionCalled(26);

290:     function setSequencerInbox(address _sequencerInbox) external override {
             bridge.setSequencerInbox(_sequencerInbox);
             emit OwnerFunctionCalled(27);

299:     function setInbox(IInboxBase newInbox) external {
             inbox = newInbox;
             emit OwnerFunctionCalled(28);

308:     function setValidatorWhitelistDisabled(bool _validatorWhitelistDisabled) external {
             validatorWhitelistDisabled = _validatorWhitelistDisabled;
             emit OwnerFunctionCalled(30);

317:     function setAnyTrustFastConfirmer(address _anyTrustFastConfirmer) external {
             anyTrustFastConfirmer = _anyTrustFastConfirmer;
             emit OwnerFunctionCalled(31);

326:     function setChallengeManager(address _challengeManager) external {
             challengeManager = IEdgeChallengeManager(_challengeManager);
             emit OwnerFunctionCalled(32);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

63:     function setTemplates(
            BridgeCreator _bridgeCreator,
            IOneStepProofEntry _osp,
            IEdgeChallengeManager _challengeManagerLogic,
            IRollupAdmin _rollupAdminLogic,
            IRollupUser _rollupUserLogic,
            IUpgradeExecutor _upgradeExecutorLogic,
            address _validatorWalletCreator,
            DeployHelper _l2FactoriesDeployer
        ) external onlyOwner {
            bridgeCreator = _bridgeCreator;
            osp = _osp;
            challengeManagerTemplate = _challengeManagerLogic;
            rollupAdminLogic = _rollupAdminLogic;
            rollupUserLogic = _rollupUserLogic;
            upgradeExecutorLogic = _upgradeExecutorLogic;
            validatorWalletCreator = _validatorWalletCreator;
            l2FactoriesDeployer = _l2FactoriesDeployer;
            emit TemplatesUpdated();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="NC-14"></a>[NC-14] Function ordering does not follow the Solidity style guide
According to the [Solidity style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions), functions should be laid out in the following order :`constructor()`, `receive()`, `fallback()`, `external`, `public`, `internal`, `private`, but the cases below do not follow this pattern

*Instances (11)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

1: 
   Current order:
   external depositIntoPool
   public withdrawFromPool
   external withdrawFromPool
   
   Suggested order:
   external depositIntoPool
   external withdrawFromPool
   public withdrawFromPool

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

1: 
   Current order:
   external createAssertion
   public makeStakeWithdrawable
   public withdrawStakeBackIntoPool
   external makeStakeWithdrawableAndWithdrawBackIntoPool
   
   Suggested order:
   external createAssertion
   external makeStakeWithdrawableAndWithdrawBackIntoPool
   public makeStakeWithdrawable
   public withdrawStakeBackIntoPool

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/bridge/SequencerInbox.sol

1: 
   Current order:
   internal _chainIdChanged
   external postUpgradeInit
   external initialize
   external updateRollupAddress
   internal getTimeBounds
   external removeDelayAfterFork
   external maxTimeVariation
   internal maxTimeVariationInternal
   external forceInclusion
   external addSequencerL2BatchFromOrigin
   external addSequencerL2BatchFromOrigin
   external addSequencerL2BatchFromBlobs
   external addSequencerL2BatchFromBlobsDelayProof
   external addSequencerL2BatchFromOriginDelayProof
   internal addSequencerL2BatchFromBlobsImpl
   internal addSequencerL2BatchFromCalldataImpl
   external addSequencerL2Batch
   external addSequencerL2BatchDelayProof
   internal delayProofImpl
   internal isDelayProofRequired
   internal packHeader
   internal formEmptyDataHash
   internal isValidCallDataFlag
   internal formCallDataHash
   internal formBlobDataHash
   internal submitBatchSpendingReport
   internal addSequencerL2BatchImpl
   external inboxAccs
   external batchCount
   external forceInclusionDeadline
   internal delayBufferableBlocks
   internal _setBufferConfig
   internal _setMaxTimeVariation
   external setMaxTimeVariation
   external setIsBatchPoster
   external setValidKeyset
   external invalidateKeysetHash
   external setIsSequencer
   external setBatchPosterManager
   external setBufferConfig
   external isValidKeysetHash
   external getKeysetCreationBlock
   
   Suggested order:
   external postUpgradeInit
   external initialize
   external updateRollupAddress
   external removeDelayAfterFork
   external maxTimeVariation
   external forceInclusion
   external addSequencerL2BatchFromOrigin
   external addSequencerL2BatchFromOrigin
   external addSequencerL2BatchFromBlobs
   external addSequencerL2BatchFromBlobsDelayProof
   external addSequencerL2BatchFromOriginDelayProof
   external addSequencerL2Batch
   external addSequencerL2BatchDelayProof
   external inboxAccs
   external batchCount
   external forceInclusionDeadline
   external setMaxTimeVariation
   external setIsBatchPoster
   external setValidKeyset
   external invalidateKeysetHash
   external setIsSequencer
   external setBatchPosterManager
   external setBufferConfig
   external isValidKeysetHash
   external getKeysetCreationBlock
   internal _chainIdChanged
   internal getTimeBounds
   internal maxTimeVariationInternal
   internal addSequencerL2BatchFromBlobsImpl
   internal addSequencerL2BatchFromCalldataImpl
   internal delayProofImpl
   internal isDelayProofRequired
   internal packHeader
   internal formEmptyDataHash
   internal isValidCallDataFlag
   internal formCallDataHash
   internal formBlobDataHash
   internal submitBatchSpendingReport
   internal addSequencerL2BatchImpl
   internal delayBufferableBlocks
   internal _setBufferConfig
   internal _setMaxTimeVariation

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

1: 
   Current order:
   external initialize
   external challengePeriodBlocks
   external oneStepProofEntry
   external createLayerZeroEdge
   external bisectEdge
   external confirmEdgeByTime
   external multiUpdateTimeCacheByChildren
   external updateTimerCacheByChildren
   external updateTimerCacheByClaim
   external confirmEdgeByOneStepProof
   external refundStake
   external getLayerZeroEndHeight
   external calculateEdgeId
   external calculateMutualId
   external edgeExists
   external getEdge
   external edgeLength
   external hasRival
   external confirmedRival
   external hasLengthOneRival
   external timeUnrivaled
   external getPrevAssertionHash
   external firstRival
   external hasMadeLayerZeroRival
   public initialize
   external createLayerZeroEdge
   external bisectEdge
   public multiUpdateTimeCacheByChildren
   public updateTimerCacheByChildren
   public updateTimerCacheByClaim
   public confirmEdgeByTime
   public confirmEdgeByOneStepProof
   public refundStake
   public getLayerZeroEndHeight
   public calculateEdgeId
   public calculateMutualId
   public edgeExists
   public getEdge
   public edgeLength
   public hasRival
   public confirmedRival
   public hasLengthOneRival
   public timeUnrivaled
   public getPrevAssertionHash
   public firstRival
   external hasMadeLayerZeroRival
   
   Suggested order:
   external initialize
   external challengePeriodBlocks
   external oneStepProofEntry
   external createLayerZeroEdge
   external bisectEdge
   external confirmEdgeByTime
   external multiUpdateTimeCacheByChildren
   external updateTimerCacheByChildren
   external updateTimerCacheByClaim
   external confirmEdgeByOneStepProof
   external refundStake
   external getLayerZeroEndHeight
   external calculateEdgeId
   external calculateMutualId
   external edgeExists
   external getEdge
   external edgeLength
   external hasRival
   external confirmedRival
   external hasLengthOneRival
   external timeUnrivaled
   external getPrevAssertionHash
   external firstRival
   external hasMadeLayerZeroRival
   external createLayerZeroEdge
   external bisectEdge
   external hasMadeLayerZeroRival
   public initialize
   public multiUpdateTimeCacheByChildren
   public updateTimerCacheByChildren
   public updateTimerCacheByClaim
   public confirmEdgeByTime
   public confirmEdgeByOneStepProof
   public refundStake
   public getLayerZeroEndHeight
   public calculateEdgeId
   public calculateMutualId
   public edgeExists
   public getEdge
   public edgeLength
   public hasRival
   public confirmedRival
   public hasLengthOneRival
   public timeUnrivaled
   public getPrevAssertionHash
   public firstRival

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

1: 
   Current order:
   internal get
   internal getNoCheck
   internal add
   private layerZeroTypeSpecificChecks
   internal isPowerOfTwo
   private layerZeroCommonChecks
   private toLayerZeroEdge
   internal createLayerZeroEdge
   internal getPrevAssertionHash
   internal hasRival
   internal hasLengthOneRival
   internal timeUnrivaledTotal
   internal updateTimerCache
   internal updateTimerCacheByChildren
   internal updateTimerCacheByClaim
   internal timeUnrivaled
   internal mandatoryBisectionHeight
   internal bisectEdge
   internal setConfirmedRival
   internal nextEdgeLevel
   private checkClaimIdLink
   internal confirmEdgeByTime
   internal confirmEdgeByOneStepProof
   
   Suggested order:
   internal get
   internal getNoCheck
   internal add
   internal isPowerOfTwo
   internal createLayerZeroEdge
   internal getPrevAssertionHash
   internal hasRival
   internal hasLengthOneRival
   internal timeUnrivaledTotal
   internal updateTimerCache
   internal updateTimerCacheByChildren
   internal updateTimerCacheByClaim
   internal timeUnrivaled
   internal mandatoryBisectionHeight
   internal bisectEdge
   internal setConfirmedRival
   internal nextEdgeLevel
   internal confirmEdgeByTime
   internal confirmEdgeByOneStepProof
   private layerZeroTypeSpecificChecks
   private layerZeroCommonChecks
   private toLayerZeroEdge
   private checkClaimIdLink

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

1: 
   Current order:
   external wasmModuleRoot
   external latestConfirmed
   external getNode
   external getStakerAddress
   external stakerCount
   external getStaker
   external isValidator
   external validatorWalletCreator
   external forceRefundStaker
   external pause
   external resume
   external postUpgradeInit
   public stateHash
   public set
   public get
   external wasmModuleRoot
   external latestConfirmed
   external getNode
   external getStakerAddress
   external stakerCount
   external getStaker
   external isValidator
   external validatorWalletCreator
   public array
   private cleanupOldRollup
   private createConfig
   private upgradeSurroundingContracts
   private upgradeSequencerInbox
   external perform
   
   Suggested order:
   external wasmModuleRoot
   external latestConfirmed
   external getNode
   external getStakerAddress
   external stakerCount
   external getStaker
   external isValidator
   external validatorWalletCreator
   external forceRefundStaker
   external pause
   external resume
   external postUpgradeInit
   external wasmModuleRoot
   external latestConfirmed
   external getNode
   external getStakerAddress
   external stakerCount
   external getStaker
   external isValidator
   external validatorWalletCreator
   external perform
   public stateHash
   public set
   public get
   public array
   private cleanupOldRollup
   private createConfig
   private upgradeSurroundingContracts
   private upgradeSequencerInbox

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

1: 
   Current order:
   external updateTemplates
   external updateERC20Templates
   internal _createBridge
   external createBridge
   
   Suggested order:
   external updateTemplates
   external updateERC20Templates
   external createBridge
   internal _createBridge

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

1: 
   Current order:
   external initialize
   external setOutbox
   external removeOldOutbox
   external setDelayedInbox
   external pause
   external resume
   internal _authorizeUpgrade
   internal _authorizeSecondaryUpgrade
   external setValidator
   external setOwner
   external setMinimumAssertionPeriod
   external setConfirmPeriodBlocks
   external setBaseStake
   external forceRefundStaker
   external forceCreateAssertion
   external forceConfirmAssertion
   external setLoserStakeEscrow
   external setWasmModuleRoot
   external setSequencerInbox
   external setInbox
   external setValidatorWhitelistDisabled
   external setAnyTrustFastConfirmer
   external setChallengeManager
   
   Suggested order:
   external initialize
   external setOutbox
   external removeOldOutbox
   external setDelayedInbox
   external pause
   external resume
   external setValidator
   external setOwner
   external setMinimumAssertionPeriod
   external setConfirmPeriodBlocks
   external setBaseStake
   external forceRefundStaker
   external forceCreateAssertion
   external forceConfirmAssertion
   external setLoserStakeEscrow
   external setWasmModuleRoot
   external setSequencerInbox
   external setInbox
   external setValidatorWhitelistDisabled
   external setAnyTrustFastConfirmer
   external setChallengeManager
   internal _authorizeUpgrade
   internal _authorizeSecondaryUpgrade

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

1: 
   Current order:
   public sequencerInbox
   internal getAssertionStorage
   public getAssertion
   external getAssertionCreationBlockForLogLookup
   external getStakerAddress
   public isStaked
   public latestStakedAssertion
   public amountStaked
   external getStaker
   external withdrawableFunds
   public latestConfirmed
   public stakerCount
   internal initializeCore
   internal confirmAssertionInternal
   internal createNewStake
   internal increaseStakeBy
   internal reduceStakeTo
   internal withdrawStaker
   internal withdrawFunds
   internal increaseWithdrawableFunds
   private deleteStaker
   internal createNewAssertion
   external genesisAssertionHash
   external getFirstChildCreationBlock
   external getSecondChildCreationBlock
   external validateAssertionHash
   external validateConfig
   external isFirstChild
   external isPending
   internal requireInactiveStaker
   
   Suggested order:
   external getAssertionCreationBlockForLogLookup
   external getStakerAddress
   external getStaker
   external withdrawableFunds
   external genesisAssertionHash
   external getFirstChildCreationBlock
   external getSecondChildCreationBlock
   external validateAssertionHash
   external validateConfig
   external isFirstChild
   external isPending
   public sequencerInbox
   public getAssertion
   public isStaked
   public latestStakedAssertion
   public amountStaked
   public latestConfirmed
   public stakerCount
   internal getAssertionStorage
   internal initializeCore
   internal confirmAssertionInternal
   internal createNewStake
   internal increaseStakeBy
   internal reduceStakeTo
   internal withdrawStaker
   internal withdrawFunds
   internal increaseWithdrawableFunds
   internal createNewAssertion
   internal requireInactiveStaker
   private deleteStaker

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

1: 
   Current order:
   external setTemplates
   internal createChallengeManager
   public createRollup
   internal _deployUpgradeExecutor
   internal _deployFactories
   
   Suggested order:
   external setTemplates
   public createRollup
   internal createChallengeManager
   internal _deployUpgradeExecutor
   internal _deployFactories

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

1: 
   Current order:
   external initialize
   internal _chainIdChanged
   internal _validatorIsAfk
   external removeWhitelistAfterFork
   external removeWhitelistAfterValidatorAfk
   external confirmAssertion
   internal _newStake
   external computeAssertionHash
   public stakeOnNewAssertion
   external returnOldDeposit
   internal _addToDeposit
   external reduceDeposit
   public fastConfirmAssertion
   external fastConfirmNewAssertion
   external owner
   external newStakeOnNewAssertion
   public newStakeOnNewAssertion
   external addToDeposit
   external withdrawStakerFunds
   private receiveTokens
   
   Suggested order:
   external initialize
   external removeWhitelistAfterFork
   external removeWhitelistAfterValidatorAfk
   external confirmAssertion
   external computeAssertionHash
   external returnOldDeposit
   external reduceDeposit
   external fastConfirmNewAssertion
   external owner
   external newStakeOnNewAssertion
   external addToDeposit
   external withdrawStakerFunds
   public stakeOnNewAssertion
   public fastConfirmAssertion
   public newStakeOnNewAssertion
   internal _chainIdChanged
   internal _validatorIsAfk
   internal _newStake
   internal _addToDeposit
   private receiveTokens

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-15"></a>[NC-15] Functions should not be longer than 50 lines
Overly complex code can make understanding functionality more difficult, try to further modularize your code to ensure readability 

*Instances (295)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

29:     function depositIntoPool(uint256 amount) external {

41:     function withdrawFromPool(uint256 amount) public {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

40:     function createAssertion(AssertionInputs calldata assertionInputs) external {

60:     function makeStakeWithdrawableAndWithdrawBackIntoPool() external {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

44:     function createEdge(CreateEdgeArgs calldata args) external {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

13:     function getPool(bytes memory creationCode, bytes memory args) internal view returns (address) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol

20:     function depositIntoPool(uint256 amount) external;

24:     function withdrawFromPool(uint256 amount) external;

30:     function stakeToken() external view returns (address);

34:     function depositBalance(address account) external view returns (uint256);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/interfaces/IAssertionStakingPool.sol

12:     function createAssertion(AssertionInputs calldata assertionInputs) external;

24:     function makeStakeWithdrawableAndWithdrawBackIntoPool() external;

27:     function rollup() external view returns (address);

30:     function assertionHash() external view returns (bytes32);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/interfaces/IEdgeStakingPool.sol

15:     function createEdge(CreateEdgeArgs calldata args) external;

18:     function challengeManager() external view returns (address);

21:     function edgeId() external view returns (bytes32);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IEdgeStakingPool.sol)

```solidity
File: src/bridge/DelayBuffer.sol

68:     function update(BufferData storage self, uint64 blockNumber) internal {

82:     function calcPendingBuffer(BufferData storage self, uint64 blockNumber)

104:     function isSynced(BufferData storage self) internal view returns (bool) {

108:     function isUpdatable(BufferData storage self) internal view returns (bool) {

115:     function isValidBufferConfig(BufferConfig memory config) internal pure returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/ISequencerInbox.sol

49:     function totalDelayedMessagesRead() external view returns (uint256);

51:     function bridge() external view returns (IBridge);

55:     function HEADER_LENGTH() external view returns (uint256);

61:     function DATA_AUTHENTICATED_FLAG() external view returns (bytes1);

67:     function DATA_BLOB_HEADER_FLAG() external view returns (bytes1);

73:     function DAS_MESSAGE_HEADER_FLAG() external view returns (bytes1);

79:     function TREE_DAS_MESSAGE_HEADER_FLAG() external view returns (bytes1);

85:     function BROTLI_MESSAGE_HEADER_FLAG() external view returns (bytes1);

91:     function ZERO_HEAVY_MESSAGE_HEADER_FLAG() external view returns (bytes1);

93:     function rollup() external view returns (IOwnable);

95:     function isBatchPoster(address) external view returns (bool);

97:     function isSequencer(address) external view returns (bool);

100:     function isDelayBufferable() external view returns (bool);

102:     function maxDataSize() external view returns (uint256);

106:     function batchPosterManager() external view returns (address);

124:     function dasKeySetInfo(bytes32) external view returns (bool, uint64);

148:     function inboxAccs(uint256 index) external view returns (bytes32);

150:     function batchCount() external view returns (uint256);

152:     function isValidKeysetHash(bytes32 ksHash) external view returns (bool);

155:     function getKeysetCreationBlock(bytes32 ksHash) external view returns (uint256);

163:     function forceInclusionDeadline(uint64 blockNumber)

247:     function setMaxTimeVariation(MaxTimeVariation memory maxTimeVariation_) external;

254:     function setIsBatchPoster(address addr, bool isBatchPoster_) external;

260:     function setValidKeyset(bytes calldata keysetBytes) external;

266:     function invalidateKeysetHash(bytes32 ksHash) external;

274:     function setIsSequencer(address addr, bool isSequencer_) external;

280:     function setBatchPosterManager(address newBatchPosterManager) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

```solidity
File: src/bridge/SequencerInbox.sol

157:     function _chainIdChanged() internal view returns (bool) {

161:     function postUpgradeInit(BufferConfig memory bufferConfig_)

216:     function getTimeBounds() internal view virtual returns (IBridge.TimeBounds memory) {

605:     function delayProofImpl(uint256 afterDelayedMessagesRead, DelayProof memory delayProof)

628:     function isDelayProofRequired(uint256 afterDelayedMessagesRead) internal view returns (bool) {

637:     function packHeader(uint256 afterDelayedMessagesRead)

659:     function formEmptyDataHash(uint256 afterDelayedMessagesRead)

675:     function isValidCallDataFlag(bytes1 headerByte) internal pure returns (bool) {

688:     function formCallDataHash(bytes calldata data, uint256 afterDelayedMessagesRead)

725:     function formBlobDataHash(uint256 afterDelayedMessagesRead)

824:     function inboxAccs(uint256 index) external view returns (bytes32) {

828:     function batchCount() external view returns (uint256) {

833:     function forceInclusionDeadline(uint64 blockNumber) external view returns (uint64) {

843:     function delayBufferableBlocks(uint64 _buffer) internal view returns (uint64) {

847:     function _setBufferConfig(BufferConfig memory bufferConfig_) internal {

867:     function _setMaxTimeVariation(ISequencerInbox.MaxTimeVariation memory maxTimeVariation_)

885:     function setMaxTimeVariation(ISequencerInbox.MaxTimeVariation memory maxTimeVariation_)

894:     function setIsBatchPoster(address addr, bool isBatchPoster_)

903:     function setValidKeyset(bytes calldata keysetBytes) external onlyRollupOwner {

922:     function invalidateKeysetHash(bytes32 ksHash) external onlyRollupOwner {

933:     function setIsSequencer(address addr, bool isSequencer_)

938:         emit OwnerFunctionCalled(4); // Owner in this context can also be batch poster manager

942:     function setBatchPosterManager(address newBatchPosterManager) external onlyRollupOwner {

947:     function setBufferConfig(BufferConfig memory bufferConfig_) external onlyRollupOwner {

952:     function isValidKeysetHash(bytes32 ksHash) external view returns (bool) {

957:     function getKeysetCreationBlock(bytes32 ksHash) external view returns (uint256) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

47:     function challengePeriodBlocks() external view returns (uint64);

50:     function oneStepProofEntry() external view returns (IOneStepProofEntry);

54:     function createLayerZeroEdge(CreateEdgeArgs calldata args) external returns (bytes32);

68:     function bisectEdge(bytes32 edgeId, bytes32 bisectionHistoryRoot, bytes calldata prefixProof)

80:     function confirmEdgeByTime(bytes32 edgeId, AssertionStateData calldata claimStateData) external;

84:     function multiUpdateTimeCacheByChildren(bytes32[] calldata edgeIds) external;

94:     function updateTimerCacheByChildren(bytes32 edgeId) external;

100:     function updateTimerCacheByClaim(bytes32 edgeId, bytes32 claimingEdgeId) external;

123:     function getLayerZeroEndHeight(EdgeType eType) external view returns (uint256);

157:     function edgeExists(bytes32 edgeId) external view returns (bool);

160:     function getEdge(bytes32 edgeId) external view returns (ChallengeEdge memory);

163:     function edgeLength(bytes32 edgeId) external view returns (uint256);

167:     function hasRival(bytes32 edgeId) external view returns (bool);

171:     function confirmedRival(bytes32 mutualId) external view returns (bytes32);

174:     function hasLengthOneRival(bytes32 edgeId) external view returns (bool);

180:     function timeUnrivaled(bytes32 edgeId) external view returns (uint256);

185:     function getPrevAssertionHash(bytes32 edgeId) external view returns (bytes32);

191:     function firstRival(bytes32 mutualId) external view returns (bytes32);

195:     function hasMadeLayerZeroRival(address account, bytes32 mutualId) external view returns (bool);

371:     function createLayerZeroEdge(CreateEdgeArgs calldata args) external returns (bytes32) {

451:     function bisectEdge(bytes32 edgeId, bytes32 bisectionHistoryRoot, bytes calldata prefixProof)

491:     function multiUpdateTimeCacheByChildren(bytes32[] calldata edgeIds) public {

499:     function updateTimerCacheByChildren(bytes32 edgeId) public {

505:     function updateTimerCacheByClaim(bytes32 edgeId, bytes32 claimingEdgeId) public {

511:     function confirmEdgeByTime(bytes32 edgeId, AssertionStateData calldata claimStateData) public {

591:     function getLayerZeroEndHeight(EdgeType eType) public view returns (uint256) {

627:     function edgeExists(bytes32 edgeId) public view returns (bool) {

632:     function getEdge(bytes32 edgeId) public view returns (ChallengeEdge memory) {

637:     function edgeLength(bytes32 edgeId) public view returns (uint256) {

642:     function hasRival(bytes32 edgeId) public view returns (bool) {

647:     function confirmedRival(bytes32 mutualId) public view returns (bytes32) {

652:     function hasLengthOneRival(bytes32 edgeId) public view returns (bool) {

657:     function timeUnrivaled(bytes32 edgeId) public view returns (uint256) {

662:     function getPrevAssertionHash(bytes32 edgeId) public view returns (bytes32) {

667:     function firstRival(bytes32 mutualId) public view returns (bytes32) {

672:     function hasMadeLayerZeroRival(address account, bytes32 mutualId) external view returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/IAssertionChain.sol

14:     function bridge() external view returns (IBridge);

21:     function validateConfig(bytes32 assertionHash, ConfigData calldata configData) external view;

22:     function getFirstChildCreationBlock(bytes32 assertionHash) external view returns (uint64);

23:     function getSecondChildCreationBlock(bytes32 assertionHash) external view returns (uint64);

24:     function isFirstChild(bytes32 assertionHash) external view returns (bool);

25:     function isPending(bytes32 assertionHash) external view returns (bool);

26:     function isValidator(address) external view returns (bool);

27:     function validatorWhitelistDisabled() external view returns (bool);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/IAssertionChain.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

13:     function append(bytes32[] memory arr, bytes32 newItem) internal pure returns (bytes32[] memory) {

27:     function slice(bytes32[] memory arr, uint256 startIndex, uint256 endIndex)

45:     function concat(bytes32[] memory arr1, bytes32[] memory arr2) internal pure returns (bytes32[] memory) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

180:     function mutualId(ChallengeEdge storage ce) internal view returns (bytes32) {

184:     function mutualIdMem(ChallengeEdge memory ce) internal pure returns (bytes32) {

208:     function idMem(ChallengeEdge memory edge) internal pure returns (bytes32) {

215:     function id(ChallengeEdge storage edge) internal view returns (bytes32) {

222:     function exists(ChallengeEdge storage edge) internal view returns (bool) {

228:     function length(ChallengeEdge storage edge) internal view returns (uint256) {

239:     function setChildren(ChallengeEdge storage edge, bytes32 lowerChildId, bytes32 upperChildId) internal {

249:     function setConfirmed(ChallengeEdge storage edge) internal {

258:     function isLayerZero(ChallengeEdge storage edge) internal view returns (bool) {

264:     function setRefunded(ChallengeEdge storage edge) internal {

279:     function levelToType(uint8 level, uint8 numBigStepLevels) internal pure returns (EdgeType eType) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

141:     function get(EdgeStore storage store, bytes32 edgeId) internal view returns (ChallengeEdge storage) {

152:     function getNoCheck(EdgeStore storage store, bytes32 edgeId) internal view returns (ChallengeEdge storage) {

160:     function add(EdgeStore storage store, ChallengeEdge memory edge) internal returns (EdgeAddedData memory) {

327:     function isPowerOfTwo(uint256 x) internal pure returns (bool) {

344:     function layerZeroCommonChecks(ProofData memory proofData, CreateEdgeArgs calldata args, uint256 expectedEndHeight)

391:     function toLayerZeroEdge(bytes32 originId, bytes32 startHistoryRoot, CreateEdgeArgs calldata args)

443:     function getPrevAssertionHash(EdgeStore storage store, bytes32 edgeId) internal view returns (bytes32) {

463:     function hasRival(EdgeStore storage store, bytes32 edgeId) internal view returns (bool) {

483:     function hasLengthOneRival(EdgeStore storage store, bytes32 edgeId) internal view returns (bool) {

488:     function timeUnrivaledTotal(EdgeStore storage store, bytes32 edgeId) internal view returns (uint256) {

502:     function updateTimerCache(EdgeStore storage store, bytes32 edgeId, uint256 newValue)

516:     function updateTimerCacheByChildren(EdgeStore storage store, bytes32 edgeId) internal returns (bool, uint256) {

537:     function timeUnrivaled(EdgeStore storage store, bytes32 edgeId) internal view returns (uint256) {

576:     function mandatoryBisectionHeight(uint256 start, uint256 end) internal pure returns (uint256) {

604:     function bisectEdge(EdgeStore storage store, bytes32 edgeId, bytes32 bisectionHistoryRoot, bytes memory prefixProof)

662:     function setConfirmedRival(EdgeStore storage store, bytes32 edgeId) internal {

674:     function nextEdgeLevel(uint8 level, uint8 numBigStepLevel) internal pure returns (uint8) {

689:     function checkClaimIdLink(EdgeStore storage store, bytes32 edgeId, bytes32 claimingEdgeId, uint8 numBigStepLevel)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

110:     function root(bytes32[] memory me) internal pure returns (bytes32) {

151:     function appendCompleteSubTree(bytes32[] memory me, uint256 level, bytes32 subtreeRoot)

238:     function appendLeaf(bytes32[] memory me, bytes32 leaf) internal pure returns (bytes32[] memory) {

251:     function maximumAppendBetween(uint256 startSize, uint256 endSize) internal pure returns (uint256) {

292:     function treeSize(bytes32[] memory me) internal pure returns (uint256) {

359:     function verifyInclusionProof(bytes32 rootHash, bytes32 leaf, uint256 index, bytes32[] memory proof)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

14:     function leastSignificantBit(uint256 x) internal pure returns (uint256 msb) {

29:     function mostSignificantBit(uint256 x) internal pure returns (uint256 msb) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/Assertion.sol

85:     function childCreated(AssertionNode storage self) internal {

93:     function requireExists(AssertionNode memory self) internal pure {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Assertion.sol)

```solidity
File: src/rollup/AssertionState.sol

18:     function toExecutionState(AssertionState memory state) internal pure returns (ExecutionState memory) {

22:     function hash(AssertionState memory state) internal pure returns (bytes32) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/AssertionState.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

67:     function wasmModuleRoot() external view returns (bytes32);

68:     function latestConfirmed() external view returns (uint64);

69:     function getNode(uint64 nodeNum) external view returns (Node memory);

70:     function getStakerAddress(uint64 stakerNum) external view returns (address);

71:     function stakerCount() external view returns (uint64);

72:     function getStaker(address staker) external view returns (OldStaker memory);

73:     function isValidator(address validator) external view returns (bool);

74:     function validatorWalletCreator() external view returns (address);

78:     function forceRefundStaker(address[] memory stacker) external;

84:     function postUpgradeInit(BufferConfig memory bufferConfig_) external;

101:     function stateHash(ExecutionState calldata executionState, uint256 inboxMaxCount) public pure returns (bytes32) {

106:     function set(bytes32 h, ExecutionState calldata executionState, uint256 inboxMaxCount) public {

112:     function get(bytes32 h) public view returns (ExecutionState memory executionState, uint256 inboxMaxCount) {

130:     function wasmModuleRoot() external view returns (bytes32) {

134:     function latestConfirmed() external view returns (uint64) {

138:     function getNode(uint64 nodeNum) external view returns (Node memory) {

142:     function getStakerAddress(uint64 stakerNum) external view returns (address) {

146:     function stakerCount() external view returns (uint64) {

150:     function getStaker(address staker) external view returns (OldStaker memory) {

154:     function isValidator(address validator) external view returns (bool) {

158:     function validatorWalletCreator() external view returns (address) {

173:     function array() public view returns (uint256[] memory) {

367:     function createConfig() private view returns (Config memory) {

411:     function upgradeSurroundingContracts(address newRollupAddress) private {

464:     function perform(address[] memory validators) external {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

53:     function updateTemplates(BridgeTemplates calldata _newTemplates) external onlyOwner {

58:     function updateERC20Templates(BridgeTemplates calldata _newTemplates) external onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/IRollupAdmin.sol

16:     function initialize(Config calldata config, ContractDependencies calldata connectedContracts) external;

28:     function removeOldOutbox(address _outbox) external;

35:     function setDelayedInbox(address _inbox, bool _enabled) external;

54:     function setValidator(address[] memory _validator, bool[] memory _val) external;

66:     function setMinimumAssertionPeriod(uint256 newPeriod) external;

72:     function setConfirmPeriodBlocks(uint64 newConfirmPeriod) external;

78:     function setBaseStake(uint256 newBaseStake) external;

80:     function forceRefundStaker(address[] memory stacker) external;

95:     function setLoserStakeEscrow(address newLoserStakerEscrow) external;

101:     function setWasmModuleRoot(bytes32 newWasmModuleRoot) external;

107:     function setSequencerInbox(address _sequencerInbox) external;

113:     function setValidatorWhitelistDisabled(bool _validatorWhitelistDisabled) external;

119:     function setChallengeManager(address _challengeManager) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupAdmin.sol)

```solidity
File: src/rollup/IRollupCore.sol

48:     function confirmPeriodBlocks() external view returns (uint64);

50:     function chainId() external view returns (uint256);

52:     function baseStake() external view returns (uint256);

54:     function wasmModuleRoot() external view returns (bytes32);

56:     function bridge() external view returns (IBridge);

58:     function sequencerInbox() external view returns (ISequencerInbox);

60:     function outbox() external view returns (IOutbox);

62:     function rollupEventInbox() external view returns (IRollupEventInbox);

64:     function challengeManager() external view returns (IEdgeChallengeManager);

66:     function loserStakeEscrow() external view returns (address);

68:     function stakeToken() external view returns (address);

70:     function minimumAssertionPeriod() external view returns (uint256);

72:     function genesisAssertionHash() external pure returns (bytes32);

77:     function getAssertion(bytes32 assertionHash) external view returns (AssertionNode memory);

86:     function getAssertionCreationBlockForLogLookup(bytes32 assertionHash) external view returns (uint256);

93:     function getStakerAddress(uint64 stakerNum) external view returns (address);

100:     function isStaked(address staker) external view returns (bool);

107:     function latestStakedAssertion(address staker) external view returns (bytes32);

114:     function amountStaked(address staker) external view returns (uint256);

121:     function getStaker(address staker) external view returns (Staker memory);

128:     function withdrawableFunds(address owner) external view returns (uint256);

130:     function latestConfirmed() external view returns (bytes32);

133:     function stakerCount() external view returns (uint64);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupCore.sol)

```solidity
File: src/rollup/IRollupLogic.sol

15:     function initialize(address stakeToken) external view;

19:     function removeWhitelistAfterValidatorAfk() external;

30:     function stakeOnNewAssertion(AssertionInputs calldata assertion, bytes32 expectedAssertionHash) external;

36:     function withdrawStakerFunds() external returns (uint256);

51:     function addToDeposit(address stakerAddress, uint256 tokenAmount) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

18:     function initialize(Config calldata config, ContractDependencies calldata connectedContracts)

117:     function setOutbox(IOutbox _outbox) external override {

127:     function removeOldOutbox(address _outbox) external override {

138:     function setDelayedInbox(address _inbox, bool _enabled) external override {

166:     function _authorizeUpgrade(address newImplementation) internal override {}

171:     function _authorizeSecondaryUpgrade(address newImplementation) internal override {}

180:     function setValidator(address[] calldata _validator, bool[] calldata _val) external override {

195:     function setOwner(address newOwner) external override {

204:     function setMinimumAssertionPeriod(uint256 newPeriod) external override {

213:     function setConfirmPeriodBlocks(uint64 newConfirmPeriod) external override {

223:     function setBaseStake(uint256 newBaseStake) external override {

228:     function forceRefundStaker(address[] calldata staker) external override whenPaused {

269:     function setLoserStakeEscrow(address newLoserStakerEscrow) external override {

281:     function setWasmModuleRoot(bytes32 newWasmModuleRoot) external override {

290:     function setSequencerInbox(address _sequencerInbox) external override {

308:     function setValidatorWhitelistDisabled(bool _validatorWhitelistDisabled) external {

317:     function setAnyTrustFastConfirmer(address _anyTrustFastConfirmer) external {

326:     function setChallengeManager(address _challengeManager) external {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

116:     function sequencerInbox() public view virtual returns (ISequencerInbox) {

126:     function getAssertionStorage(bytes32 assertionHash) internal view returns (AssertionNode storage) {

134:     function getAssertion(bytes32 assertionHash) public view override returns (AssertionNode memory) {

145:     function getAssertionCreationBlockForLogLookup(bytes32 assertionHash) external view override returns (uint256) {

162:     function getStakerAddress(uint64 stakerNum) external view override returns (address) {

171:     function isStaked(address staker) public view override returns (bool) {

180:     function latestStakedAssertion(address staker) public view override returns (bytes32) {

189:     function amountStaked(address staker) public view override returns (uint256) {

198:     function getStaker(address staker) external view override returns (Staker memory) {

207:     function withdrawableFunds(address user) external view override returns (uint256) {

212:     function latestConfirmed() public view override returns (bytes32) {

217:     function stakerCount() public view override returns (uint64) {

225:     function initializeCore(AssertionNode memory initialAssertion, bytes32 assertionHash) internal {

274:     function createNewStake(address stakerAddress, uint256 depositAmount, address withdrawalAddress) internal {

286:     function increaseStakeBy(address stakerAddress, uint256 amountAdded) internal {

300:     function reduceStakeTo(address stakerAddress, uint256 target) internal returns (uint256) {

317:     function withdrawStaker(address stakerAddress) internal {

331:     function withdrawFunds(address account) internal returns (uint256) {

343:     function increaseWithdrawableFunds(address account, uint256 amount) internal {

355:     function deleteStaker(address stakerAddress) private {

520:     function genesisAssertionHash() external pure returns (bytes32) {

532:     function getFirstChildCreationBlock(bytes32 assertionHash) external view returns (uint64) {

536:     function getSecondChildCreationBlock(bytes32 assertionHash) external view returns (uint64) {

549:     function validateConfig(bytes32 assertionHash, ConfigData calldata configData) external view {

553:     function isFirstChild(bytes32 assertionHash) external view returns (bool) {

557:     function isPending(bytes32 assertionHash) external view returns (bool) {

565:     function requireInactiveStaker(address stakerAddress) internal view {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

85:     function createChallengeManager(address rollupAddr, address proxyAdminAddr, Config memory config)

137:     function createRollup(RollupDeploymentParams memory deployParams)

267:     function _deployUpgradeExecutor(address rollupOwner, ProxyAdmin proxyAdmin)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupProxy.sol

12:     function initializeProxy(Config memory config, ContractDependencies memory connectedContracts)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

27:     function initialize(address _stakeToken) external view override onlyProxy {

33:     function _chainIdChanged() internal view returns (bool) {

51:     function _validatorIsAfk() internal view returns (bool) {

71:     function removeWhitelistAfterValidatorAfk() external {

137:     function _newStake(uint256 depositAmount, address withdrawalAddress) internal onlyValidator whenNotPaused {

150:     function computeAssertionHash(bytes32 prevAssertionHash, AssertionState calldata state, bytes32 inboxAcc)

163:     function stakeOnNewAssertion(AssertionInputs calldata assertion, bytes32 expectedAssertionHash)

222:     function returnOldDeposit() external override onlyValidator whenNotPaused {

232:     function _addToDeposit(address stakerAddress, uint256 depositAmount) internal onlyValidator whenNotPaused {

241:     function reduceDeposit(uint256 target) external onlyValidator whenNotPaused {

273:     function fastConfirmNewAssertion(AssertionInputs calldata assertion, bytes32 expectedAssertionHash)

308:     function owner() external view returns (address) {

349:     function addToDeposit(address stakerAddress, uint256 tokenAmount) external onlyValidator whenNotPaused {

358:     function withdrawStakerFunds() external override whenNotPaused returns (uint256) {

366:     function receiveTokens(uint256 tokenAmount) private {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-16"></a>[NC-16] Interfaces should be defined in separate files from their usage
The interfaces below should be defined in separate files, so that it's easier for future projects to import them, and to avoid duplication later on if they need to be used elsewhere in the project

*Instances (5)*:
```solidity
File: src/challengeV2/EdgeChallengeManager.sol

18: interface IEdgeChallengeManager {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

49: interface IOldRollup {

77: interface IOldRollupAdmin {

83: interface ISeqInboxPostUpgradeInit {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/IRollupLogic.sol

12: interface IRollupUser is IRollupCore, IOwnable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

### <a name="NC-17"></a>[NC-17] Lack of checks in setters
Be it sanity checks (like checks against `0`-values) or initial setting checks: it's best for Setter functions to have them

*Instances (24)*:
```solidity
File: src/bridge/DelayBuffer.sol

68:     function update(BufferData storage self, uint64 blockNumber) internal {
            self.bufferBlocks = calcPendingBuffer(self, blockNumber);
    
            // store a new starting reference point
            // any buffer updates will be applied retroactively in the next batch post
            self.prevBlockNumber = blockNumber;
            self.prevSequencedBlockNumber = uint64(block.number);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/SequencerInbox.sol

885:     function setMaxTimeVariation(ISequencerInbox.MaxTimeVariation memory maxTimeVariation_)
             external
             onlyRollupOwner
         {
             _setMaxTimeVariation(maxTimeVariation_);
             emit OwnerFunctionCalled(0);

894:     function setIsBatchPoster(address addr, bool isBatchPoster_)
             external
             onlyRollupOwnerOrBatchPosterManager
         {
             isBatchPoster[addr] = isBatchPoster_;
             emit OwnerFunctionCalled(1);

933:     function setIsSequencer(address addr, bool isSequencer_)
             external
             onlyRollupOwnerOrBatchPosterManager
         {
             isSequencer[addr] = isSequencer_;
             emit OwnerFunctionCalled(4); // Owner in this context can also be batch poster manager

942:     function setBatchPosterManager(address newBatchPosterManager) external onlyRollupOwner {
             batchPosterManager = newBatchPosterManager;
             emit OwnerFunctionCalled(5);

947:     function setBufferConfig(BufferConfig memory bufferConfig_) external onlyRollupOwner {
             _setBufferConfig(bufferConfig_);
             emit OwnerFunctionCalled(6);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

94:     function updateTimerCacheByChildren(bytes32 edgeId) external;

100:     function updateTimerCacheByClaim(bytes32 edgeId, bytes32 claimingEdgeId) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

516:     function updateTimerCacheByChildren(EdgeStore storage store, bytes32 edgeId) internal returns (bool, uint256) {
             return updateTimerCache(store, edgeId, timeUnrivaledTotal(store, edgeId));

520:     function updateTimerCacheByClaim(
             EdgeStore storage store,
             bytes32 edgeId,
             bytes32 claimingEdgeId,
             uint8 numBigStepLevel
         ) internal returns (bool, uint256) {
             // calculate the time unrivaled without inheritance
             uint256 totalTimeUnrivaled = timeUnrivaled(store, edgeId);
             checkClaimIdLink(store, edgeId, claimingEdgeId, numBigStepLevel);
             totalTimeUnrivaled += store.edges[claimingEdgeId].totalTimeUnrivaledCache;
             return updateTimerCache(store, edgeId, totalTimeUnrivaled);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/rollup/BridgeCreator.sol

53:     function updateTemplates(BridgeTemplates calldata _newTemplates) external onlyOwner {
            ethBasedTemplates = _newTemplates;
            emit TemplatesUpdated();

58:     function updateERC20Templates(BridgeTemplates calldata _newTemplates) external onlyOwner {
            erc20BasedTemplates = _newTemplates;
            emit ERC20TemplatesUpdated();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

117:     function setOutbox(IOutbox _outbox) external override {
             outbox = _outbox;
             bridge.setOutbox(address(_outbox), true);
             emit OwnerFunctionCalled(0);

138:     function setDelayedInbox(address _inbox, bool _enabled) external override {
             bridge.setDelayedInbox(address(_inbox), _enabled);
             emit OwnerFunctionCalled(2);

195:     function setOwner(address newOwner) external override {
             _changeAdmin(newOwner);
             emit OwnerFunctionCalled(7);

204:     function setMinimumAssertionPeriod(uint256 newPeriod) external override {
             minimumAssertionPeriod = newPeriod;
             emit OwnerFunctionCalled(8);

223:     function setBaseStake(uint256 newBaseStake) external override {
             baseStake = newBaseStake;
             emit OwnerFunctionCalled(12);

281:     function setWasmModuleRoot(bytes32 newWasmModuleRoot) external override {
             wasmModuleRoot = newWasmModuleRoot;
             emit OwnerFunctionCalled(26);

290:     function setSequencerInbox(address _sequencerInbox) external override {
             bridge.setSequencerInbox(_sequencerInbox);
             emit OwnerFunctionCalled(27);

299:     function setInbox(IInboxBase newInbox) external {
             inbox = newInbox;
             emit OwnerFunctionCalled(28);

308:     function setValidatorWhitelistDisabled(bool _validatorWhitelistDisabled) external {
             validatorWhitelistDisabled = _validatorWhitelistDisabled;
             emit OwnerFunctionCalled(30);

317:     function setAnyTrustFastConfirmer(address _anyTrustFastConfirmer) external {
             anyTrustFastConfirmer = _anyTrustFastConfirmer;
             emit OwnerFunctionCalled(31);

326:     function setChallengeManager(address _challengeManager) external {
             challengeManager = IEdgeChallengeManager(_challengeManager);
             emit OwnerFunctionCalled(32);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

63:     function setTemplates(
            BridgeCreator _bridgeCreator,
            IOneStepProofEntry _osp,
            IEdgeChallengeManager _challengeManagerLogic,
            IRollupAdmin _rollupAdminLogic,
            IRollupUser _rollupUserLogic,
            IUpgradeExecutor _upgradeExecutorLogic,
            address _validatorWalletCreator,
            DeployHelper _l2FactoriesDeployer
        ) external onlyOwner {
            bridgeCreator = _bridgeCreator;
            osp = _osp;
            challengeManagerTemplate = _challengeManagerLogic;
            rollupAdminLogic = _rollupAdminLogic;
            rollupUserLogic = _rollupUserLogic;
            upgradeExecutorLogic = _upgradeExecutorLogic;
            validatorWalletCreator = _validatorWalletCreator;
            l2FactoriesDeployer = _l2FactoriesDeployer;
            emit TemplatesUpdated();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="NC-18"></a>[NC-18] Lines are too long
Usually lines in source code are limited to [80](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) characters. Today's screens are much larger so it's reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over [164](https://github.com/aizatto/character-length) characters, the lines below should be split when they reach that length

*Instances (1)*:
```solidity
File: src/rollup/BOLDUpgradeAction.sol

396:             chainConfig: "", // we can use an empty chain config it wont be used in the rollup initialization because we check if the rei is already connected there

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

### <a name="NC-19"></a>[NC-19] Missing Event for critical parameters change
Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

*Instances (3)*:
```solidity
File: src/bridge/SequencerInbox.sol

208:     function updateRollupAddress() external {
             if (msg.sender != IOwnable(rollup).owner())
                 revert NotOwner(msg.sender, IOwnable(rollup).owner());
             IOwnable newRollup = bridge.rollup();
             if (rollup == newRollup) revert RollupNotChanged();
             rollup = newRollup;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

94:     function updateTimerCacheByChildren(bytes32 edgeId) external;

100:     function updateTimerCacheByClaim(bytes32 edgeId, bytes32 claimingEdgeId) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

### <a name="NC-20"></a>[NC-20] NatSpec is completely non-existent on functions that should have them
Public and external functions that aren't view or pure should have NatSpec comments

*Instances (38)*:
```solidity
File: src/bridge/SequencerInbox.sol

161:     function postUpgradeInit(BufferConfig memory bufferConfig_)

177:     function initialize(

947:     function setBufferConfig(BufferConfig memory bufferConfig_) external onlyRollupOwner {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

78:     function forceRefundStaker(address[] memory stacker) external;

78:     function forceRefundStaker(address[] memory stacker) external;

78:     function forceRefundStaker(address[] memory stacker) external;

78:     function forceRefundStaker(address[] memory stacker) external;

79:     function pause() external;

79:     function pause() external;

79:     function pause() external;

79:     function pause() external;

80:     function resume() external;

80:     function resume() external;

80:     function resume() external;

80:     function resume() external;

84:     function postUpgradeInit(BufferConfig memory bufferConfig_) external;

84:     function postUpgradeInit(BufferConfig memory bufferConfig_) external;

84:     function postUpgradeInit(BufferConfig memory bufferConfig_) external;

84:     function postUpgradeInit(BufferConfig memory bufferConfig_) external;

106:     function set(bytes32 h, ExecutionState calldata executionState, uint256 inboxMaxCount) public {

106:     function set(bytes32 h, ExecutionState calldata executionState, uint256 inboxMaxCount) public {

106:     function set(bytes32 h, ExecutionState calldata executionState, uint256 inboxMaxCount) public {

106:     function set(bytes32 h, ExecutionState calldata executionState, uint256 inboxMaxCount) public {

464:     function perform(address[] memory validators) external {

464:     function perform(address[] memory validators) external {

464:     function perform(address[] memory validators) external {

464:     function perform(address[] memory validators) external {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

53:     function updateTemplates(BridgeTemplates calldata _newTemplates) external onlyOwner {

58:     function updateERC20Templates(BridgeTemplates calldata _newTemplates) external onlyOwner {

99:     function createBridge(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

18:     function initialize(Config calldata config, ContractDependencies calldata connectedContracts)

228:     function forceRefundStaker(address[] calldata staker) external override whenPaused {

237:     function forceCreateAssertion(

258:     function forceConfirmAssertion(

269:     function setLoserStakeEscrow(address newLoserStakerEscrow) external override {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

63:     function setTemplates(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupProxy.sol

12:     function initializeProxy(Config memory config, ContractDependencies memory connectedContracts)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

62:     function removeWhitelistAfterFork() external {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-21"></a>[NC-21] Incomplete NatSpec: `@param` is missing on actually documented functions
The following functions are missing `@param` NatSpec comments.

*Instances (6)*:
```solidity
File: src/challengeV2/EdgeChallengeManager.sol

72:     /// @notice An edge can be confirmed if the total amount of time it and a single chain of its direct ancestors
        ///         has spent unrivaled is greater than the challenge period.
        /// @dev    Edges inherit time from their parents, so the sum of unrivaled timers is compared against the threshold.
        ///         Given that an edge cannot become unrivaled after becoming rivaled, once the threshold is passed
        ///         it will always remain passed. The direct ancestors of an edge are linked by parent-child links for edges
        ///         of the same level, and claimId-edgeId links for zero layer edges that claim an edge in the level below.
        ///         This method also includes the amount of time the assertion being claimed spent without a sibling
        /// @param edgeId                   The id of the edge to confirm
        function confirmEdgeByTime(bytes32 edgeId, AssertionStateData calldata claimStateData) external;

117:     /// @notice When zero layer block edges are created a stake is also provided
         ///         The stake on this edge can be refunded if the edge is confirme
         function refundStake(bytes32 edgeId) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

77:     /**
         * @notice Confirm a unresolved assertion
         * @param confirmState The state to confirm
         * @param winningEdgeId The winning edge if a challenge is started
         */
        function confirmAssertion(
            bytes32 assertionHash,
            bytes32 prevAssertionHash,
            AssertionState calldata confirmState,
            bytes32 winningEdgeId,
            ConfigData calldata prevConfig,
            bytes32 inboxAcc

247:     /**
          * @notice This allow the anyTrustFastConfirmer to force confirm any pending assertion
          *         the anyTrustFastConfirmer is supposed to be set only on an AnyTrust chain to
          *         a contract that can call this function when received sufficient signatures
          */
         function fastConfirmAssertion(
             bytes32 assertionHash,
             bytes32 parentAssertionHash,
             AssertionState calldata confirmState,
             bytes32 inboxAcc

263:     /**
          * @notice This allow the anyTrustFastConfirmer to immediately create and confirm an assertion
          *         the anyTrustFastConfirmer is supposed to be set only on an AnyTrust chain to
          *         a contract that can call this function when received sufficient signatures
          *         The logic in this function is similar to stakeOnNewAssertion, but without staker checks
          *
          *         We trust the anyTrustFastConfirmer to not call this function multiple times on the same prev,
          *         as doing so would result in incorrect accounting of withdrawable funds in the loserStakeEscrow.
          *         This is because the protocol assume there is only 1 unique confirmable child assertion.
          */
         function fastConfirmNewAssertion(AssertionInputs calldata assertion, bytes32 expectedAssertionHash)

312:     /**
          * @notice Deprecated, use the function with `withdrawalAddress` instead
          *         Using this default `withdrawalAddress` to msg.sender
          */
         function newStakeOnNewAssertion(
             uint256 tokenAmount,
             AssertionInputs calldata assertion,
             bytes32 expectedAssertionHash

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-22"></a>[NC-22] Incomplete NatSpec: `@return` is missing on actually documented functions
The following functions are missing `@return` NatSpec comments.

*Instances (2)*:
```solidity
File: src/challengeV2/EdgeChallengeManager.sol

52:     /// @notice Performs necessary checks and creates a new layer zero edge
        /// @param args             Edge creation args
        function createLayerZeroEdge(CreateEdgeArgs calldata args) external returns (bytes32);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

355:     /**
          * @notice Withdraw uncommitted funds owned by sender from the rollup chain
          */
         function withdrawStakerFunds() external override whenNotPaused returns (uint256) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-23"></a>[NC-23] File's first line is not an SPDX Identifier

*Instances (39)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPoolCreator.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPoolCreator.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/interfaces/IAssertionStakingPool.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/interfaces/IAssertionStakingPoolCreator.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAssertionStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/interfaces/IEdgeStakingPool.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IEdgeStakingPool.sol)

```solidity
File: src/assertionStakingPool/interfaces/IEdgeStakingPoolCreator.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IEdgeStakingPoolCreator.sol)

```solidity
File: src/bridge/DelayBuffer.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/DelayBufferTypes.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBufferTypes.sol)

```solidity
File: src/bridge/ISequencerInbox.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

```solidity
File: src/bridge/SequencerInbox.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

1: // Copyright 2023, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/IAssertionChain.sol

1: // Copyright 2023, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/IAssertionChain.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

1: // Copyright 2023, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

1: // Copyright 2023, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/ChallengeErrors.sol

1: // Copyright 2023, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeErrors.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

1: // Copyright 2023, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/Enums.sol

1: // Copyright 2023, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/Enums.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

1: // Copyright 2023, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

1: // Copyright 2023, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/libraries/Error.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/libraries/Error.sol)

```solidity
File: src/rollup/Assertion.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Assertion.sol)

```solidity
File: src/rollup/AssertionState.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/AssertionState.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/Config.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Config.sol)

```solidity
File: src/rollup/IRollupAdmin.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupAdmin.sol)

```solidity
File: src/rollup/IRollupCore.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupCore.sol)

```solidity
File: src/rollup/IRollupLogic.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupLib.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupLib.sol)

```solidity
File: src/rollup/RollupProxy.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

1: // Copyright 2021-2022, Offchain Labs, Inc.

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-24"></a>[NC-24] Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor
If a function is supposed to be access-controlled, a `modifier` should be used instead of a `require/if` statement for more readability.

*Instances (23)*:
```solidity
File: src/bridge/SequencerInbox.sol

104:         if (msg.sender != rollup.owner()) revert NotOwner(msg.sender, rollup.owner());

109:         if (msg.sender != rollup.owner() && msg.sender != batchPosterManager) {

209:         if (msg.sender != IOwnable(rollup).owner())

375:         if (msg.sender != tx.origin) revert NotOrigin();

376:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

397:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

417:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

440:         if (msg.sender != tx.origin) revert NotOrigin();

441:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

507:         if (msg.sender == tx.origin && !isUsingFeeToken) {

568:         if (!isBatchPoster[msg.sender] && msg.sender != address(rollup)) revert NotBatchPoster();

591:         if (!isBatchPoster[msg.sender] && msg.sender != address(rollup)) revert NotBatchPoster();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

376:         if (whitelistEnabled && !assertionChain.isValidator(msg.sender)) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

432:             if (store.hasMadeLayerZeroRival[msg.sender][mutualId]) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

21:         require(isValidator[msg.sender] || validatorWhitelistDisabled, "NOT_VALIDATOR");

139:         require(!isStaked(msg.sender), "ALREADY_STAKED");

175:         require(isStaked(msg.sender), "NOT_STAKED");

182:         require(amountStaked(msg.sender) >= assertion.beforeStateData.configData.requiredStake, "INSUFFICIENT_STAKE");

194:         bytes32 lastAssertion = latestStakedAssertion(msg.sender);

223:         requireInactiveStaker(msg.sender);

242:         requireInactiveStaker(msg.sender);

258:         require(msg.sender == anyTrustFastConfirmer, "NOT_FAST_CONFIRMER");

321:         newStakeOnNewAssertion(tokenAmount, assertion, expectedAssertionHash, msg.sender);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-25"></a>[NC-25] Consider using named mappings
Consider moving to solidity version 0.8.18 or later, and using [named mappings](https://ethereum.stackexchange.com/questions/51629/how-to-name-the-arguments-in-mapping/145555#145555) to make it easier to understand the purpose of each mapping

*Instances (14)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

22:     mapping(address => uint256) public depositBalance;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/bridge/SequencerInbox.sol

95:     mapping(address => bool) public isBatchPoster;

101:     mapping(bytes32 => DasKeySetInfo) public dasKeySetInfo;

115:     mapping(address => bool) public isSequencer;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

76:     mapping(bytes32 => ChallengeEdge) edges;

81:     mapping(bytes32 => bytes32) firstRivals;

84:     mapping(bytes32 => bytes32) confirmedRivals;

86:     mapping(address => mapping(bytes32 => bool)) hasMadeLayerZeroRival;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

99:     mapping(bytes32 => bytes) internal preImages;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupCore.sol

96:     mapping(address => bool) public isValidator;

99:     mapping(bytes32 => AssertionNode) private _assertions;

102:     mapping(address => Staker) public _stakerMap;

104:     mapping(address => uint256) private _withdrawableFunds;

114:     mapping(bytes32 => uint256) internal _assertionCreatedAtArbSysBlock;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

### <a name="NC-26"></a>[NC-26] Adding a `return` statement when the function defines a named return variable, is redundant

*Instances (4)*:
```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

278:     /// @notice Returns the edge type for a given level, given the total number of big step levels
         function levelToType(uint8 level, uint8 numBigStepLevels) internal pure returns (EdgeType eType) {
             if (level == 0) {
                 return EdgeType.Block;

278:     /// @notice Returns the edge type for a given level, given the total number of big step levels
         function levelToType(uint8 level, uint8 numBigStepLevels) internal pure returns (EdgeType eType) {
             if (level == 0) {
                 return EdgeType.Block;
             } else if (level <= numBigStepLevels) {
                 return EdgeType.BigStep;

278:     /// @notice Returns the edge type for a given level, given the total number of big step levels
         function levelToType(uint8 level, uint8 numBigStepLevels) internal pure returns (EdgeType eType) {
             if (level == 0) {
                 return EdgeType.Block;
             } else if (level <= numBigStepLevels) {
                 return EdgeType.BigStep;
             } else if (level == numBigStepLevels + 1) {
                 return EdgeType.SmallStep;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

10:     /// @notice The least significant bit in the bit representation of a uint
        /// @dev    Zero indexed from the least sig bit. Eg 1010 => 1, 1100 => 2, 1001 => 0
        ///         Finds lsb in linear (uint size) time
        /// @param x Cannot be zero, since zero that has no signficant bits
        function leastSignificantBit(uint256 x) internal pure returns (uint256 msb) {
            require(x > 0, "Zero has no significant bits");
    
            // isolate the least sig bit
            uint256 isolated = ((x - 1) & x) ^ x;
            
            // since we removed all higher bits, least sig == most sig
            return mostSignificantBit(isolated);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

### <a name="NC-27"></a>[NC-27] Take advantage of Custom Error's return value property
An important feature of Custom Error is that values such as address, tokenID, msg.value can be written inside the () sign, this kind of approach provides a serious advantage in debugging and examining the revert details of dapps such as tenderly.

*Instances (66)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

31:             revert ZeroAmount();

43:             revert ZeroAmount();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

19:             revert PoolDoesntExist();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/bridge/SequencerInbox.sol

104:         if (msg.sender != rollup.owner()) revert NotOwner(msg.sender, rollup.owner());

148:             if (reader4844_ != IReader4844(address(0))) revert DataBlobsNotSupported();

166:         if (!isDelayBufferable) revert NotDelayBufferable();

171:             revert AlreadyInit();

182:         if (bridge != IBridge(address(0))) revert AlreadyInit();

183:         if (bridge_ == IBridge(address(0))) revert HadZeroInit();

194:             revert NativeTokenMismatch();

210:             revert NotOwner(msg.sender, IOwnable(rollup).owner());

212:         if (rollup == newRollup) revert RollupNotChanged();

237:         if (!_chainIdChanged()) revert NotForked();

295:         if (_totalDelayedMessagesRead <= totalDelayedMessagesRead) revert DelayedBackwards();

314:         if (l1BlockAndTime[0] + delayBlocks_ >= block.number) revert ForceIncludeBlockTooSoon();

324:         ) revert IncorrectMessagePreimage();

362:         revert Deprecated();

375:         if (msg.sender != tx.origin) revert NotOrigin();

376:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

377:         if (isDelayProofRequired(afterDelayedMessagesRead)) revert DelayProofRequired();

397:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

398:         if (isDelayProofRequired(afterDelayedMessagesRead)) revert DelayProofRequired();

417:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

418:         if (!isDelayBufferable) revert NotDelayBufferable();

440:         if (msg.sender != tx.origin) revert NotOrigin();

441:         if (!isBatchPoster[msg.sender]) revert NotBatchPoster();

442:         if (!isDelayBufferable) revert NotDelayBufferable();

501:         if (hostChainIsArbitrum) revert DataBlobsNotSupported();

568:         if (!isBatchPoster[msg.sender] && msg.sender != address(rollup)) revert NotBatchPoster();

569:         if (isDelayProofRequired(afterDelayedMessagesRead)) revert DelayProofRequired();

591:         if (!isBatchPoster[msg.sender] && msg.sender != address(rollup)) revert NotBatchPoster();

592:         if (!isDelayBufferable) revert NotDelayBufferable();

621:                     revert InvalidDelayedAccPreimage();

736:         if (dataHashes.length == 0) revert MissingDataHashes();

773:         if (extraGas > type(uint64).max) revert ExtraGasNotUint64();

806:         if (afterDelayedMessagesRead < totalDelayedMessagesRead) revert DelayedBackwards();

807:         if (afterDelayedMessagesRead > bridge.delayedMessageCount()) revert DelayedTooFar();

848:         if (!isDelayBufferable) revert NotDelayBufferable();

849:         if (!DelayBuffer.isValidBufferConfig(bufferConfig_)) revert BadBufferConfig();

876:             revert BadMaxTimeVariation();

906:         if (keysetBytes.length >= 64 * 1024) revert KeysetTooLarge();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

318:             revert EmptyAssertionChain();

322:             revert EmptyOneStepProofEntry();

326:             revert EmptyChallengePeriod();

332:             revert EmptyStakeReceiver();

351:             revert ZeroBigStepLevels();

388:                 revert EmptyEdgeSpecificProof();

514:             revert EdgeNotLayerZero(topEdge.id(), topEdge.staker, topEdge.claimId);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

77:             revert EmptyOriginId();

83:             revert EmptyStartRoot();

86:             revert EmptyEndRoot();

104:             revert EmptyStaker();

107:             revert EmptyClaimId();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

230:                 revert AssertionHashEmpty();

238:                 revert AssertionNotPending();

244:                 revert AssertionNoSibling();

249:                 revert EmptyEdgeSpecificProof();

256:                 revert EmptyStartMachineStatus();

259:                 revert EmptyEndMachineStatus();

285:                 revert ClaimEdgeNotPending();

295:                 revert EmptyEdgeSpecificProof();

379:             revert EmptyPrefixProof();

473:             revert EmptyFirstRival();

546:             revert EmptyFirstRival();

699:             revert OriginIdMutualIdMismatch(store.edges[edgeId].mutualId(), store.edges[claimingEdgeId].originId);

789:             revert EdgeNotLengthOne(store.edges[edgeId].length());

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

### <a name="NC-28"></a>[NC-28] Avoid the use of sensitive terms
Use [alternative variants](https://www.zdnet.com/article/mysql-drops-master-slave-and-blacklist-whitelist-terminology/), e.g. allowlist/denylist instead of whitelist/blacklist

*Instances (24)*:
```solidity
File: src/challengeV2/EdgeChallengeManager.sol

374:         bool whitelistEnabled = !assertionChain.validatorWhitelistDisabled();

376:         if (whitelistEnabled && !assertionChain.isValidator(msg.sender)) {

418:             args, ard, oneStepProofEntry, expectedEndHeight, NUM_BIGSTEP_LEVEL, whitelistEnabled

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/IAssertionChain.sol

27:     function validatorWhitelistDisabled() external view returns (bool);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/IAssertionChain.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

418:         bool whitelistEnabled

430:         if (whitelistEnabled) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

204:     bool public immutable DISABLE_VALIDATOR_WHITELIST;

245:         bool disableValidatorWhitelist;

327:         DISABLE_VALIDATOR_WHITELIST = settings.disableValidatorWhitelist;

534:         if (DISABLE_VALIDATOR_WHITELIST) {

535:             IRollupAdmin(address(rollup)).setValidatorWhitelistDisabled(DISABLE_VALIDATOR_WHITELIST);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/IRollupAdmin.sol

113:     function setValidatorWhitelistDisabled(bool _validatorWhitelistDisabled) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupAdmin.sol)

```solidity
File: src/rollup/IRollupLogic.sol

17:     function removeWhitelistAfterFork() external;

19:     function removeWhitelistAfterValidatorAfk() external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

308:     function setValidatorWhitelistDisabled(bool _validatorWhitelistDisabled) external {

309:         validatorWhitelistDisabled = _validatorWhitelistDisabled;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

108:     bool public validatorWhitelistDisabled;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

21:         require(isValidator[msg.sender] || validatorWhitelistDisabled, "NOT_VALIDATOR");

62:     function removeWhitelistAfterFork() external {

63:         require(!validatorWhitelistDisabled, "WHITELIST_DISABLED");

65:         validatorWhitelistDisabled = true;

71:     function removeWhitelistAfterValidatorAfk() external {

72:         require(!validatorWhitelistDisabled, "WHITELIST_DISABLED");

74:         validatorWhitelistDisabled = true;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-29"></a>[NC-29] Contract does not follow the Solidity style guide's suggested layout ordering
The [style guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout) says that, within a contract, the ordering should be:

1) Type declarations
2) State variables
3) Events
4) Modifiers
5) Functions

However, the contract(s) below do not follow this ordering

*Instances (8)*:
```solidity
File: src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol

1: 
   Current order:
   EventDefinition.StakeDeposited
   EventDefinition.StakeWithdrawn
   ErrorDefinition.ZeroAmount
   ErrorDefinition.AmountExceedsBalance
   FunctionDefinition.depositIntoPool
   FunctionDefinition.withdrawFromPool
   FunctionDefinition.withdrawFromPool
   FunctionDefinition.stakeToken
   FunctionDefinition.depositBalance
   
   Suggested order:
   ErrorDefinition.ZeroAmount
   ErrorDefinition.AmountExceedsBalance
   EventDefinition.StakeDeposited
   EventDefinition.StakeWithdrawn
   FunctionDefinition.depositIntoPool
   FunctionDefinition.withdrawFromPool
   FunctionDefinition.withdrawFromPool
   FunctionDefinition.stakeToken
   FunctionDefinition.depositBalance

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol)

```solidity
File: src/bridge/ISequencerInbox.sol

1: 
   Current order:
   StructDefinition.MaxTimeVariation
   EventDefinition.SequencerBatchDelivered
   EventDefinition.OwnerFunctionCalled
   EventDefinition.SequencerBatchData
   EventDefinition.SetValidKeyset
   EventDefinition.InvalidateKeyset
   FunctionDefinition.totalDelayedMessagesRead
   FunctionDefinition.bridge
   FunctionDefinition.HEADER_LENGTH
   FunctionDefinition.DATA_AUTHENTICATED_FLAG
   FunctionDefinition.DATA_BLOB_HEADER_FLAG
   FunctionDefinition.DAS_MESSAGE_HEADER_FLAG
   FunctionDefinition.TREE_DAS_MESSAGE_HEADER_FLAG
   FunctionDefinition.BROTLI_MESSAGE_HEADER_FLAG
   FunctionDefinition.ZERO_HEAVY_MESSAGE_HEADER_FLAG
   FunctionDefinition.rollup
   FunctionDefinition.isBatchPoster
   FunctionDefinition.isSequencer
   FunctionDefinition.isDelayBufferable
   FunctionDefinition.maxDataSize
   FunctionDefinition.batchPosterManager
   StructDefinition.DasKeySetInfo
   FunctionDefinition.maxTimeVariation
   FunctionDefinition.dasKeySetInfo
   FunctionDefinition.removeDelayAfterFork
   FunctionDefinition.forceInclusion
   FunctionDefinition.inboxAccs
   FunctionDefinition.batchCount
   FunctionDefinition.isValidKeysetHash
   FunctionDefinition.getKeysetCreationBlock
   FunctionDefinition.forceInclusionDeadline
   FunctionDefinition.addSequencerL2BatchFromOrigin
   FunctionDefinition.addSequencerL2BatchFromOrigin
   FunctionDefinition.addSequencerL2Batch
   FunctionDefinition.addSequencerL2BatchFromBlobs
   FunctionDefinition.addSequencerL2BatchFromBlobsDelayProof
   FunctionDefinition.addSequencerL2BatchFromOriginDelayProof
   FunctionDefinition.addSequencerL2BatchDelayProof
   FunctionDefinition.setMaxTimeVariation
   FunctionDefinition.setIsBatchPoster
   FunctionDefinition.setValidKeyset
   FunctionDefinition.invalidateKeysetHash
   FunctionDefinition.setIsSequencer
   FunctionDefinition.setBatchPosterManager
   FunctionDefinition.updateRollupAddress
   FunctionDefinition.initialize
   
   Suggested order:
   StructDefinition.MaxTimeVariation
   StructDefinition.DasKeySetInfo
   EventDefinition.SequencerBatchDelivered
   EventDefinition.OwnerFunctionCalled
   EventDefinition.SequencerBatchData
   EventDefinition.SetValidKeyset
   EventDefinition.InvalidateKeyset
   FunctionDefinition.totalDelayedMessagesRead
   FunctionDefinition.bridge
   FunctionDefinition.HEADER_LENGTH
   FunctionDefinition.DATA_AUTHENTICATED_FLAG
   FunctionDefinition.DATA_BLOB_HEADER_FLAG
   FunctionDefinition.DAS_MESSAGE_HEADER_FLAG
   FunctionDefinition.TREE_DAS_MESSAGE_HEADER_FLAG
   FunctionDefinition.BROTLI_MESSAGE_HEADER_FLAG
   FunctionDefinition.ZERO_HEAVY_MESSAGE_HEADER_FLAG
   FunctionDefinition.rollup
   FunctionDefinition.isBatchPoster
   FunctionDefinition.isSequencer
   FunctionDefinition.isDelayBufferable
   FunctionDefinition.maxDataSize
   FunctionDefinition.batchPosterManager
   FunctionDefinition.maxTimeVariation
   FunctionDefinition.dasKeySetInfo
   FunctionDefinition.removeDelayAfterFork
   FunctionDefinition.forceInclusion
   FunctionDefinition.inboxAccs
   FunctionDefinition.batchCount
   FunctionDefinition.isValidKeysetHash
   FunctionDefinition.getKeysetCreationBlock
   FunctionDefinition.forceInclusionDeadline
   FunctionDefinition.addSequencerL2BatchFromOrigin
   FunctionDefinition.addSequencerL2BatchFromOrigin
   FunctionDefinition.addSequencerL2Batch
   FunctionDefinition.addSequencerL2BatchFromBlobs
   FunctionDefinition.addSequencerL2BatchFromBlobsDelayProof
   FunctionDefinition.addSequencerL2BatchFromOriginDelayProof
   FunctionDefinition.addSequencerL2BatchDelayProof
   FunctionDefinition.setMaxTimeVariation
   FunctionDefinition.setIsBatchPoster
   FunctionDefinition.setValidKeyset
   FunctionDefinition.invalidateKeysetHash
   FunctionDefinition.setIsSequencer
   FunctionDefinition.setBatchPosterManager
   FunctionDefinition.updateRollupAddress
   FunctionDefinition.initialize

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

```solidity
File: src/bridge/SequencerInbox.sol

1: 
   Current order:
   VariableDeclaration.totalDelayedMessagesRead
   VariableDeclaration.bridge
   VariableDeclaration.HEADER_LENGTH
   VariableDeclaration.DATA_AUTHENTICATED_FLAG
   VariableDeclaration.DATA_BLOB_HEADER_FLAG
   VariableDeclaration.DAS_MESSAGE_HEADER_FLAG
   VariableDeclaration.TREE_DAS_MESSAGE_HEADER_FLAG
   VariableDeclaration.BROTLI_MESSAGE_HEADER_FLAG
   VariableDeclaration.ZERO_HEAVY_MESSAGE_HEADER_FLAG
   VariableDeclaration.GAS_PER_BLOB
   VariableDeclaration.rollup
   VariableDeclaration.isBatchPoster
   VariableDeclaration.__LEGACY_MAX_TIME_VARIATION
   VariableDeclaration.dasKeySetInfo
   ModifierDefinition.onlyRollupOwner
   ModifierDefinition.onlyRollupOwnerOrBatchPosterManager
   VariableDeclaration.isSequencer
   VariableDeclaration.reader4844
   VariableDeclaration.delayBlocks
   VariableDeclaration.futureBlocks
   VariableDeclaration.delaySeconds
   VariableDeclaration.futureSeconds
   VariableDeclaration.batchPosterManager
   VariableDeclaration.maxDataSize
   VariableDeclaration.deployTimeChainId
   VariableDeclaration.hostChainIsArbitrum
   VariableDeclaration.isUsingFeeToken
   VariableDeclaration.isDelayBufferable
   UsingForDirective.BufferData
   VariableDeclaration.buffer
   FunctionDefinition.constructor
   FunctionDefinition._chainIdChanged
   FunctionDefinition.postUpgradeInit
   FunctionDefinition.initialize
   FunctionDefinition.updateRollupAddress
   FunctionDefinition.getTimeBounds
   FunctionDefinition.removeDelayAfterFork
   FunctionDefinition.maxTimeVariation
   FunctionDefinition.maxTimeVariationInternal
   FunctionDefinition.forceInclusion
   FunctionDefinition.addSequencerL2BatchFromOrigin
   FunctionDefinition.addSequencerL2BatchFromOrigin
   FunctionDefinition.addSequencerL2BatchFromBlobs
   FunctionDefinition.addSequencerL2BatchFromBlobsDelayProof
   FunctionDefinition.addSequencerL2BatchFromOriginDelayProof
   FunctionDefinition.addSequencerL2BatchFromBlobsImpl
   FunctionDefinition.addSequencerL2BatchFromCalldataImpl
   FunctionDefinition.addSequencerL2Batch
   FunctionDefinition.addSequencerL2BatchDelayProof
   FunctionDefinition.delayProofImpl
   FunctionDefinition.isDelayProofRequired
   FunctionDefinition.packHeader
   FunctionDefinition.formEmptyDataHash
   FunctionDefinition.isValidCallDataFlag
   FunctionDefinition.formCallDataHash
   FunctionDefinition.formBlobDataHash
   FunctionDefinition.submitBatchSpendingReport
   FunctionDefinition.addSequencerL2BatchImpl
   FunctionDefinition.inboxAccs
   FunctionDefinition.batchCount
   FunctionDefinition.forceInclusionDeadline
   FunctionDefinition.delayBufferableBlocks
   FunctionDefinition._setBufferConfig
   FunctionDefinition._setMaxTimeVariation
   FunctionDefinition.setMaxTimeVariation
   FunctionDefinition.setIsBatchPoster
   FunctionDefinition.setValidKeyset
   FunctionDefinition.invalidateKeysetHash
   FunctionDefinition.setIsSequencer
   FunctionDefinition.setBatchPosterManager
   FunctionDefinition.setBufferConfig
   FunctionDefinition.isValidKeysetHash
   FunctionDefinition.getKeysetCreationBlock
   
   Suggested order:
   UsingForDirective.BufferData
   VariableDeclaration.totalDelayedMessagesRead
   VariableDeclaration.bridge
   VariableDeclaration.HEADER_LENGTH
   VariableDeclaration.DATA_AUTHENTICATED_FLAG
   VariableDeclaration.DATA_BLOB_HEADER_FLAG
   VariableDeclaration.DAS_MESSAGE_HEADER_FLAG
   VariableDeclaration.TREE_DAS_MESSAGE_HEADER_FLAG
   VariableDeclaration.BROTLI_MESSAGE_HEADER_FLAG
   VariableDeclaration.ZERO_HEAVY_MESSAGE_HEADER_FLAG
   VariableDeclaration.GAS_PER_BLOB
   VariableDeclaration.rollup
   VariableDeclaration.isBatchPoster
   VariableDeclaration.__LEGACY_MAX_TIME_VARIATION
   VariableDeclaration.dasKeySetInfo
   VariableDeclaration.isSequencer
   VariableDeclaration.reader4844
   VariableDeclaration.delayBlocks
   VariableDeclaration.futureBlocks
   VariableDeclaration.delaySeconds
   VariableDeclaration.futureSeconds
   VariableDeclaration.batchPosterManager
   VariableDeclaration.maxDataSize
   VariableDeclaration.deployTimeChainId
   VariableDeclaration.hostChainIsArbitrum
   VariableDeclaration.isUsingFeeToken
   VariableDeclaration.isDelayBufferable
   VariableDeclaration.buffer
   ModifierDefinition.onlyRollupOwner
   ModifierDefinition.onlyRollupOwnerOrBatchPosterManager
   FunctionDefinition.constructor
   FunctionDefinition._chainIdChanged
   FunctionDefinition.postUpgradeInit
   FunctionDefinition.initialize
   FunctionDefinition.updateRollupAddress
   FunctionDefinition.getTimeBounds
   FunctionDefinition.removeDelayAfterFork
   FunctionDefinition.maxTimeVariation
   FunctionDefinition.maxTimeVariationInternal
   FunctionDefinition.forceInclusion
   FunctionDefinition.addSequencerL2BatchFromOrigin
   FunctionDefinition.addSequencerL2BatchFromOrigin
   FunctionDefinition.addSequencerL2BatchFromBlobs
   FunctionDefinition.addSequencerL2BatchFromBlobsDelayProof
   FunctionDefinition.addSequencerL2BatchFromOriginDelayProof
   FunctionDefinition.addSequencerL2BatchFromBlobsImpl
   FunctionDefinition.addSequencerL2BatchFromCalldataImpl
   FunctionDefinition.addSequencerL2Batch
   FunctionDefinition.addSequencerL2BatchDelayProof
   FunctionDefinition.delayProofImpl
   FunctionDefinition.isDelayProofRequired
   FunctionDefinition.packHeader
   FunctionDefinition.formEmptyDataHash
   FunctionDefinition.isValidCallDataFlag
   FunctionDefinition.formCallDataHash
   FunctionDefinition.formBlobDataHash
   FunctionDefinition.submitBatchSpendingReport
   FunctionDefinition.addSequencerL2BatchImpl
   FunctionDefinition.inboxAccs
   FunctionDefinition.batchCount
   FunctionDefinition.forceInclusionDeadline
   FunctionDefinition.delayBufferableBlocks
   FunctionDefinition._setBufferConfig
   FunctionDefinition._setMaxTimeVariation
   FunctionDefinition.setMaxTimeVariation
   FunctionDefinition.setIsBatchPoster
   FunctionDefinition.setValidKeyset
   FunctionDefinition.invalidateKeysetHash
   FunctionDefinition.setIsSequencer
   FunctionDefinition.setBatchPosterManager
   FunctionDefinition.setBufferConfig
   FunctionDefinition.isValidKeysetHash
   FunctionDefinition.getKeysetCreationBlock

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

1: 
   Current order:
   FunctionDefinition.initialize
   FunctionDefinition.challengePeriodBlocks
   FunctionDefinition.oneStepProofEntry
   FunctionDefinition.createLayerZeroEdge
   FunctionDefinition.bisectEdge
   FunctionDefinition.confirmEdgeByTime
   FunctionDefinition.multiUpdateTimeCacheByChildren
   FunctionDefinition.updateTimerCacheByChildren
   FunctionDefinition.updateTimerCacheByClaim
   FunctionDefinition.confirmEdgeByOneStepProof
   FunctionDefinition.refundStake
   FunctionDefinition.getLayerZeroEndHeight
   FunctionDefinition.calculateEdgeId
   FunctionDefinition.calculateMutualId
   FunctionDefinition.edgeExists
   FunctionDefinition.getEdge
   FunctionDefinition.edgeLength
   FunctionDefinition.hasRival
   FunctionDefinition.confirmedRival
   FunctionDefinition.hasLengthOneRival
   FunctionDefinition.timeUnrivaled
   FunctionDefinition.getPrevAssertionHash
   FunctionDefinition.firstRival
   FunctionDefinition.hasMadeLayerZeroRival
   UsingForDirective.EdgeStore
   UsingForDirective.ChallengeEdge
   UsingForDirective.IERC20
   EventDefinition.EdgeAdded
   EventDefinition.EdgeBisected
   EventDefinition.EdgeConfirmedByTime
   EventDefinition.EdgeConfirmedByOneStepProof
   EventDefinition.TimerCacheUpdated
   EventDefinition.EdgeRefunded
   VariableDeclaration.store
   VariableDeclaration.excessStakeReceiver
   VariableDeclaration.stakeToken
   VariableDeclaration.stakeAmounts
   VariableDeclaration.challengePeriodBlocks
   VariableDeclaration.assertionChain
   VariableDeclaration.oneStepProofEntry
   VariableDeclaration.LAYERZERO_BLOCKEDGE_HEIGHT
   VariableDeclaration.LAYERZERO_BIGSTEPEDGE_HEIGHT
   VariableDeclaration.LAYERZERO_SMALLSTEPEDGE_HEIGHT
   VariableDeclaration.NUM_BIGSTEP_LEVEL
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.createLayerZeroEdge
   FunctionDefinition.bisectEdge
   FunctionDefinition.multiUpdateTimeCacheByChildren
   FunctionDefinition.updateTimerCacheByChildren
   FunctionDefinition.updateTimerCacheByClaim
   FunctionDefinition.confirmEdgeByTime
   FunctionDefinition.confirmEdgeByOneStepProof
   FunctionDefinition.refundStake
   FunctionDefinition.getLayerZeroEndHeight
   FunctionDefinition.calculateEdgeId
   FunctionDefinition.calculateMutualId
   FunctionDefinition.edgeExists
   FunctionDefinition.getEdge
   FunctionDefinition.edgeLength
   FunctionDefinition.hasRival
   FunctionDefinition.confirmedRival
   FunctionDefinition.hasLengthOneRival
   FunctionDefinition.timeUnrivaled
   FunctionDefinition.getPrevAssertionHash
   FunctionDefinition.firstRival
   FunctionDefinition.hasMadeLayerZeroRival
   
   Suggested order:
   UsingForDirective.EdgeStore
   UsingForDirective.ChallengeEdge
   UsingForDirective.IERC20
   VariableDeclaration.store
   VariableDeclaration.excessStakeReceiver
   VariableDeclaration.stakeToken
   VariableDeclaration.stakeAmounts
   VariableDeclaration.challengePeriodBlocks
   VariableDeclaration.assertionChain
   VariableDeclaration.oneStepProofEntry
   VariableDeclaration.LAYERZERO_BLOCKEDGE_HEIGHT
   VariableDeclaration.LAYERZERO_BIGSTEPEDGE_HEIGHT
   VariableDeclaration.LAYERZERO_SMALLSTEPEDGE_HEIGHT
   VariableDeclaration.NUM_BIGSTEP_LEVEL
   EventDefinition.EdgeAdded
   EventDefinition.EdgeBisected
   EventDefinition.EdgeConfirmedByTime
   EventDefinition.EdgeConfirmedByOneStepProof
   EventDefinition.TimerCacheUpdated
   EventDefinition.EdgeRefunded
   FunctionDefinition.initialize
   FunctionDefinition.challengePeriodBlocks
   FunctionDefinition.oneStepProofEntry
   FunctionDefinition.createLayerZeroEdge
   FunctionDefinition.bisectEdge
   FunctionDefinition.confirmEdgeByTime
   FunctionDefinition.multiUpdateTimeCacheByChildren
   FunctionDefinition.updateTimerCacheByChildren
   FunctionDefinition.updateTimerCacheByClaim
   FunctionDefinition.confirmEdgeByOneStepProof
   FunctionDefinition.refundStake
   FunctionDefinition.getLayerZeroEndHeight
   FunctionDefinition.calculateEdgeId
   FunctionDefinition.calculateMutualId
   FunctionDefinition.edgeExists
   FunctionDefinition.getEdge
   FunctionDefinition.edgeLength
   FunctionDefinition.hasRival
   FunctionDefinition.confirmedRival
   FunctionDefinition.hasLengthOneRival
   FunctionDefinition.timeUnrivaled
   FunctionDefinition.getPrevAssertionHash
   FunctionDefinition.firstRival
   FunctionDefinition.hasMadeLayerZeroRival
   FunctionDefinition.constructor
   FunctionDefinition.initialize
   FunctionDefinition.createLayerZeroEdge
   FunctionDefinition.bisectEdge
   FunctionDefinition.multiUpdateTimeCacheByChildren
   FunctionDefinition.updateTimerCacheByChildren
   FunctionDefinition.updateTimerCacheByClaim
   FunctionDefinition.confirmEdgeByTime
   FunctionDefinition.confirmEdgeByOneStepProof
   FunctionDefinition.refundStake
   FunctionDefinition.getLayerZeroEndHeight
   FunctionDefinition.calculateEdgeId
   FunctionDefinition.calculateMutualId
   FunctionDefinition.edgeExists
   FunctionDefinition.getEdge
   FunctionDefinition.edgeLength
   FunctionDefinition.hasRival
   FunctionDefinition.confirmedRival
   FunctionDefinition.hasLengthOneRival
   FunctionDefinition.timeUnrivaled
   FunctionDefinition.getPrevAssertionHash
   FunctionDefinition.firstRival
   FunctionDefinition.hasMadeLayerZeroRival

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

1: 
   Current order:
   StructDefinition.Assertion
   EventDefinition.NodeCreated
   FunctionDefinition.wasmModuleRoot
   FunctionDefinition.latestConfirmed
   FunctionDefinition.getNode
   FunctionDefinition.getStakerAddress
   FunctionDefinition.stakerCount
   FunctionDefinition.getStaker
   FunctionDefinition.isValidator
   FunctionDefinition.validatorWalletCreator
   FunctionDefinition.forceRefundStaker
   FunctionDefinition.pause
   FunctionDefinition.resume
   FunctionDefinition.postUpgradeInit
   UsingForDirective.GlobalState
   EventDefinition.HashSet
   VariableDeclaration.preImages
   FunctionDefinition.stateHash
   FunctionDefinition.set
   FunctionDefinition.get
   VariableDeclaration.rollup
   FunctionDefinition.constructor
   FunctionDefinition.wasmModuleRoot
   FunctionDefinition.latestConfirmed
   FunctionDefinition.getNode
   FunctionDefinition.getStakerAddress
   FunctionDefinition.stakerCount
   FunctionDefinition.getStaker
   FunctionDefinition.isValidator
   FunctionDefinition.validatorWalletCreator
   VariableDeclaration._array
   FunctionDefinition.constructor
   FunctionDefinition.array
   UsingForDirective.AssertionState
   VariableDeclaration.BLOCK_LEAF_SIZE
   VariableDeclaration.BIGSTEP_LEAF_SIZE
   VariableDeclaration.SMALLSTEP_LEAF_SIZE
   VariableDeclaration.NUM_BIGSTEP_LEVEL
   VariableDeclaration.L1_TIMELOCK
   VariableDeclaration.OLD_ROLLUP
   VariableDeclaration.BRIDGE
   VariableDeclaration.SEQ_INBOX
   VariableDeclaration.REI
   VariableDeclaration.OUTBOX
   VariableDeclaration.INBOX
   VariableDeclaration.CONFIRM_PERIOD_BLOCKS
   VariableDeclaration.CHALLENGE_PERIOD_BLOCKS
   VariableDeclaration.STAKE_TOKEN
   VariableDeclaration.STAKE_AMOUNT
   VariableDeclaration.CHAIN_ID
   VariableDeclaration.ANY_TRUST_FAST_CONFIRMER
   VariableDeclaration.DISABLE_VALIDATOR_WHITELIST
   VariableDeclaration.CHALLENGE_GRACE_PERIOD_BLOCKS
   VariableDeclaration.MINI_STAKE_AMOUNTS_STORAGE
   VariableDeclaration.IS_DELAY_BUFFERABLE
   VariableDeclaration.MAX
   VariableDeclaration.THRESHOLD
   VariableDeclaration.REPLENISH_RATE_IN_BASIS
   VariableDeclaration.OSP
   VariableDeclaration.PROXY_ADMIN_OUTBOX
   VariableDeclaration.PROXY_ADMIN_BRIDGE
   VariableDeclaration.PROXY_ADMIN_REI
   VariableDeclaration.PROXY_ADMIN_SEQUENCER_INBOX
   VariableDeclaration.PROXY_ADMIN_INBOX
   VariableDeclaration.PREIMAGE_LOOKUP
   VariableDeclaration.ROLLUP_READER
   VariableDeclaration.IMPL_BRIDGE
   VariableDeclaration.IMPL_SEQUENCER_INBOX
   VariableDeclaration.IMPL_INBOX
   VariableDeclaration.IMPL_REI
   VariableDeclaration.IMPL_OUTBOX
   VariableDeclaration.IMPL_PATCHED_OLD_ROLLUP_USER
   VariableDeclaration.IMPL_NEW_ROLLUP_USER
   VariableDeclaration.IMPL_NEW_ROLLUP_ADMIN
   VariableDeclaration.IMPL_CHALLENGE_MANAGER
   EventDefinition.RollupMigrated
   StructDefinition.Settings
   StructDefinition.ProxyAdmins
   StructDefinition.Implementations
   StructDefinition.Contracts
   FunctionDefinition.constructor
   FunctionDefinition.cleanupOldRollup
   FunctionDefinition.createConfig
   FunctionDefinition.upgradeSurroundingContracts
   FunctionDefinition.upgradeSequencerInbox
   FunctionDefinition.perform
   
   Suggested order:
   UsingForDirective.GlobalState
   UsingForDirective.AssertionState
   VariableDeclaration.preImages
   VariableDeclaration.rollup
   VariableDeclaration._array
   VariableDeclaration.BLOCK_LEAF_SIZE
   VariableDeclaration.BIGSTEP_LEAF_SIZE
   VariableDeclaration.SMALLSTEP_LEAF_SIZE
   VariableDeclaration.NUM_BIGSTEP_LEVEL
   VariableDeclaration.L1_TIMELOCK
   VariableDeclaration.OLD_ROLLUP
   VariableDeclaration.BRIDGE
   VariableDeclaration.SEQ_INBOX
   VariableDeclaration.REI
   VariableDeclaration.OUTBOX
   VariableDeclaration.INBOX
   VariableDeclaration.CONFIRM_PERIOD_BLOCKS
   VariableDeclaration.CHALLENGE_PERIOD_BLOCKS
   VariableDeclaration.STAKE_TOKEN
   VariableDeclaration.STAKE_AMOUNT
   VariableDeclaration.CHAIN_ID
   VariableDeclaration.ANY_TRUST_FAST_CONFIRMER
   VariableDeclaration.DISABLE_VALIDATOR_WHITELIST
   VariableDeclaration.CHALLENGE_GRACE_PERIOD_BLOCKS
   VariableDeclaration.MINI_STAKE_AMOUNTS_STORAGE
   VariableDeclaration.IS_DELAY_BUFFERABLE
   VariableDeclaration.MAX
   VariableDeclaration.THRESHOLD
   VariableDeclaration.REPLENISH_RATE_IN_BASIS
   VariableDeclaration.OSP
   VariableDeclaration.PROXY_ADMIN_OUTBOX
   VariableDeclaration.PROXY_ADMIN_BRIDGE
   VariableDeclaration.PROXY_ADMIN_REI
   VariableDeclaration.PROXY_ADMIN_SEQUENCER_INBOX
   VariableDeclaration.PROXY_ADMIN_INBOX
   VariableDeclaration.PREIMAGE_LOOKUP
   VariableDeclaration.ROLLUP_READER
   VariableDeclaration.IMPL_BRIDGE
   VariableDeclaration.IMPL_SEQUENCER_INBOX
   VariableDeclaration.IMPL_INBOX
   VariableDeclaration.IMPL_REI
   VariableDeclaration.IMPL_OUTBOX
   VariableDeclaration.IMPL_PATCHED_OLD_ROLLUP_USER
   VariableDeclaration.IMPL_NEW_ROLLUP_USER
   VariableDeclaration.IMPL_NEW_ROLLUP_ADMIN
   VariableDeclaration.IMPL_CHALLENGE_MANAGER
   StructDefinition.Assertion
   StructDefinition.Settings
   StructDefinition.ProxyAdmins
   StructDefinition.Implementations
   StructDefinition.Contracts
   EventDefinition.NodeCreated
   EventDefinition.HashSet
   EventDefinition.RollupMigrated
   FunctionDefinition.wasmModuleRoot
   FunctionDefinition.latestConfirmed
   FunctionDefinition.getNode
   FunctionDefinition.getStakerAddress
   FunctionDefinition.stakerCount
   FunctionDefinition.getStaker
   FunctionDefinition.isValidator
   FunctionDefinition.validatorWalletCreator
   FunctionDefinition.forceRefundStaker
   FunctionDefinition.pause
   FunctionDefinition.resume
   FunctionDefinition.postUpgradeInit
   FunctionDefinition.stateHash
   FunctionDefinition.set
   FunctionDefinition.get
   FunctionDefinition.constructor
   FunctionDefinition.wasmModuleRoot
   FunctionDefinition.latestConfirmed
   FunctionDefinition.getNode
   FunctionDefinition.getStakerAddress
   FunctionDefinition.stakerCount
   FunctionDefinition.getStaker
   FunctionDefinition.isValidator
   FunctionDefinition.validatorWalletCreator
   FunctionDefinition.constructor
   FunctionDefinition.array
   FunctionDefinition.constructor
   FunctionDefinition.cleanupOldRollup
   FunctionDefinition.createConfig
   FunctionDefinition.upgradeSurroundingContracts
   FunctionDefinition.upgradeSequencerInbox
   FunctionDefinition.perform

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

1: 
   Current order:
   VariableDeclaration.ethBasedTemplates
   VariableDeclaration.erc20BasedTemplates
   EventDefinition.TemplatesUpdated
   EventDefinition.ERC20TemplatesUpdated
   StructDefinition.BridgeTemplates
   StructDefinition.BridgeContracts
   FunctionDefinition.constructor
   FunctionDefinition.updateTemplates
   FunctionDefinition.updateERC20Templates
   FunctionDefinition._createBridge
   FunctionDefinition.createBridge
   
   Suggested order:
   VariableDeclaration.ethBasedTemplates
   VariableDeclaration.erc20BasedTemplates
   StructDefinition.BridgeTemplates
   StructDefinition.BridgeContracts
   EventDefinition.TemplatesUpdated
   EventDefinition.ERC20TemplatesUpdated
   FunctionDefinition.constructor
   FunctionDefinition.updateTemplates
   FunctionDefinition.updateERC20Templates
   FunctionDefinition._createBridge
   FunctionDefinition.createBridge

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/RollupCreator.sol

1: 
   Current order:
   UsingForDirective.IERC20
   EventDefinition.RollupCreated
   EventDefinition.TemplatesUpdated
   StructDefinition.RollupDeploymentParams
   VariableDeclaration.bridgeCreator
   VariableDeclaration.osp
   VariableDeclaration.challengeManagerTemplate
   VariableDeclaration.rollupAdminLogic
   VariableDeclaration.rollupUserLogic
   VariableDeclaration.upgradeExecutorLogic
   VariableDeclaration.validatorWalletCreator
   VariableDeclaration.l2FactoriesDeployer
   FunctionDefinition.constructor
   FunctionDefinition.receive
   FunctionDefinition.setTemplates
   FunctionDefinition.createChallengeManager
   FunctionDefinition.createRollup
   FunctionDefinition._deployUpgradeExecutor
   FunctionDefinition._deployFactories
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.bridgeCreator
   VariableDeclaration.osp
   VariableDeclaration.challengeManagerTemplate
   VariableDeclaration.rollupAdminLogic
   VariableDeclaration.rollupUserLogic
   VariableDeclaration.upgradeExecutorLogic
   VariableDeclaration.validatorWalletCreator
   VariableDeclaration.l2FactoriesDeployer
   StructDefinition.RollupDeploymentParams
   EventDefinition.RollupCreated
   EventDefinition.TemplatesUpdated
   FunctionDefinition.constructor
   FunctionDefinition.receive
   FunctionDefinition.setTemplates
   FunctionDefinition.createChallengeManager
   FunctionDefinition.createRollup
   FunctionDefinition._deployUpgradeExecutor
   FunctionDefinition._deployFactories

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

1: 
   Current order:
   UsingForDirective.AssertionNode
   UsingForDirective.GlobalState
   UsingForDirective.IERC20
   ModifierDefinition.onlyValidator
   FunctionDefinition.initialize
   VariableDeclaration.deployTimeChainId
   FunctionDefinition._chainIdChanged
   VariableDeclaration.VALIDATOR_AFK_BLOCKS
   FunctionDefinition._validatorIsAfk
   FunctionDefinition.removeWhitelistAfterFork
   FunctionDefinition.removeWhitelistAfterValidatorAfk
   FunctionDefinition.confirmAssertion
   FunctionDefinition._newStake
   FunctionDefinition.computeAssertionHash
   FunctionDefinition.stakeOnNewAssertion
   FunctionDefinition.returnOldDeposit
   FunctionDefinition._addToDeposit
   FunctionDefinition.reduceDeposit
   FunctionDefinition.fastConfirmAssertion
   FunctionDefinition.fastConfirmNewAssertion
   FunctionDefinition.owner
   FunctionDefinition.newStakeOnNewAssertion
   FunctionDefinition.newStakeOnNewAssertion
   FunctionDefinition.addToDeposit
   FunctionDefinition.withdrawStakerFunds
   FunctionDefinition.receiveTokens
   
   Suggested order:
   UsingForDirective.AssertionNode
   UsingForDirective.GlobalState
   UsingForDirective.IERC20
   VariableDeclaration.deployTimeChainId
   VariableDeclaration.VALIDATOR_AFK_BLOCKS
   ModifierDefinition.onlyValidator
   FunctionDefinition.initialize
   FunctionDefinition._chainIdChanged
   FunctionDefinition._validatorIsAfk
   FunctionDefinition.removeWhitelistAfterFork
   FunctionDefinition.removeWhitelistAfterValidatorAfk
   FunctionDefinition.confirmAssertion
   FunctionDefinition._newStake
   FunctionDefinition.computeAssertionHash
   FunctionDefinition.stakeOnNewAssertion
   FunctionDefinition.returnOldDeposit
   FunctionDefinition._addToDeposit
   FunctionDefinition.reduceDeposit
   FunctionDefinition.fastConfirmAssertion
   FunctionDefinition.fastConfirmNewAssertion
   FunctionDefinition.owner
   FunctionDefinition.newStakeOnNewAssertion
   FunctionDefinition.newStakeOnNewAssertion
   FunctionDefinition.addToDeposit
   FunctionDefinition.withdrawStakerFunds
   FunctionDefinition.receiveTokens

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-30"></a>[NC-30] Use Underscores for Number Literals (add an underscore every 3 digits)

*Instances (3)*:
```solidity
File: src/bridge/DelayBuffer.sol

17:     uint256 public constant BASIS = 10000;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/SequencerInbox.sol

906:         if (keysetBytes.length >= 64 * 1024) revert KeysetTooLarge();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

49:     uint256 public constant VALIDATOR_AFK_BLOCKS = 201600;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-31"></a>[NC-31] Internal and private variables and functions names should begin with an underscore
According to the Solidity Style Guide, Non-`external` variable and function names should begin with an [underscore](https://docs.soliditylang.org/en/latest/style-guide.html#underscore-prefix-for-non-external-functions-and-variables)

*Instances (105)*:
```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

13:     function getPool(bytes memory creationCode, bytes memory args) internal view returns (address) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/bridge/DelayBuffer.sol

33:     function calcBuffer(

68:     function update(BufferData storage self, uint64 blockNumber) internal {

82:     function calcPendingBuffer(BufferData storage self, uint64 blockNumber)

104:     function isSynced(BufferData storage self) internal view returns (bool) {

108:     function isUpdatable(BufferData storage self) internal view returns (bool) {

115:     function isValidBufferConfig(BufferConfig memory config) internal pure returns (bool) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/SequencerInbox.sol

119:     uint64 internal delayBlocks;

120:     uint64 internal futureBlocks;

121:     uint64 internal delaySeconds;

122:     uint64 internal futureSeconds;

216:     function getTimeBounds() internal view virtual returns (IBridge.TimeBounds memory) {

269:     function maxTimeVariationInternal()

455:     function addSequencerL2BatchFromBlobsImpl(

512:     function addSequencerL2BatchFromCalldataImpl(

605:     function delayProofImpl(uint256 afterDelayedMessagesRead, DelayProof memory delayProof)

628:     function isDelayProofRequired(uint256 afterDelayedMessagesRead) internal view returns (bool) {

637:     function packHeader(uint256 afterDelayedMessagesRead)

659:     function formEmptyDataHash(uint256 afterDelayedMessagesRead)

675:     function isValidCallDataFlag(bytes1 headerByte) internal pure returns (bool) {

688:     function formCallDataHash(bytes calldata data, uint256 afterDelayedMessagesRead)

725:     function formBlobDataHash(uint256 afterDelayedMessagesRead)

755:     function submitBatchSpendingReport(

791:     function addSequencerL2BatchImpl(

843:     function delayBufferableBlocks(uint64 _buffer) internal view returns (uint64) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

266:     EdgeStore internal store;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

13:     function append(bytes32[] memory arr, bytes32 newItem) internal pure returns (bytes32[] memory) {

27:     function slice(bytes32[] memory arr, uint256 startIndex, uint256 endIndex)

45:     function concat(bytes32[] memory arr1, bytes32[] memory arr2) internal pure returns (bytes32[] memory) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

69:     function newEdgeChecks(

93:     function newLayerZeroEdge(

133:     function newChildEdge(

166:     function mutualIdComponent(

180:     function mutualId(ChallengeEdge storage ce) internal view returns (bytes32) {

184:     function mutualIdMem(ChallengeEdge memory ce) internal pure returns (bytes32) {

189:     function idComponent(

208:     function idMem(ChallengeEdge memory edge) internal pure returns (bytes32) {

215:     function id(ChallengeEdge storage edge) internal view returns (bytes32) {

222:     function exists(ChallengeEdge storage edge) internal view returns (bool) {

228:     function length(ChallengeEdge storage edge) internal view returns (uint256) {

239:     function setChildren(ChallengeEdge storage edge, bytes32 lowerChildId, bytes32 upperChildId) internal {

249:     function setConfirmed(ChallengeEdge storage edge) internal {

258:     function isLayerZero(ChallengeEdge storage edge) internal view returns (bool) {

264:     function setRefunded(ChallengeEdge storage edge) internal {

279:     function levelToType(uint8 level, uint8 numBigStepLevels) internal pure returns (EdgeType eType) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

141:     function get(EdgeStore storage store, bytes32 edgeId) internal view returns (ChallengeEdge storage) {

152:     function getNoCheck(EdgeStore storage store, bytes32 edgeId) internal view returns (ChallengeEdge storage) {

160:     function add(EdgeStore storage store, ChallengeEdge memory edge) internal returns (EdgeAddedData memory) {

213:     function layerZeroTypeSpecificChecks(

327:     function isPowerOfTwo(uint256 x) internal pure returns (bool) {

344:     function layerZeroCommonChecks(ProofData memory proofData, CreateEdgeArgs calldata args, uint256 expectedEndHeight)

391:     function toLayerZeroEdge(bytes32 originId, bytes32 startHistoryRoot, CreateEdgeArgs calldata args)

411:     function createLayerZeroEdge(

443:     function getPrevAssertionHash(EdgeStore storage store, bytes32 edgeId) internal view returns (bytes32) {

463:     function hasRival(EdgeStore storage store, bytes32 edgeId) internal view returns (bool) {

483:     function hasLengthOneRival(EdgeStore storage store, bytes32 edgeId) internal view returns (bool) {

488:     function timeUnrivaledTotal(EdgeStore storage store, bytes32 edgeId) internal view returns (uint256) {

502:     function updateTimerCache(EdgeStore storage store, bytes32 edgeId, uint256 newValue)

516:     function updateTimerCacheByChildren(EdgeStore storage store, bytes32 edgeId) internal returns (bool, uint256) {

520:     function updateTimerCacheByClaim(

537:     function timeUnrivaled(EdgeStore storage store, bytes32 edgeId) internal view returns (uint256) {

576:     function mandatoryBisectionHeight(uint256 start, uint256 end) internal pure returns (uint256) {

604:     function bisectEdge(EdgeStore storage store, bytes32 edgeId, bytes32 bisectionHistoryRoot, bytes memory prefixProof)

662:     function setConfirmedRival(EdgeStore storage store, bytes32 edgeId) internal {

674:     function nextEdgeLevel(uint8 level, uint8 numBigStepLevel) internal pure returns (uint8) {

689:     function checkClaimIdLink(EdgeStore storage store, bytes32 edgeId, bytes32 claimingEdgeId, uint8 numBigStepLevel)

723:     function confirmEdgeByTime(

766:     function confirmEdgeByOneStepProof(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

110:     function root(bytes32[] memory me) internal pure returns (bytes32) {

151:     function appendCompleteSubTree(bytes32[] memory me, uint256 level, bytes32 subtreeRoot)

238:     function appendLeaf(bytes32[] memory me, bytes32 leaf) internal pure returns (bytes32[] memory) {

251:     function maximumAppendBetween(uint256 startSize, uint256 endSize) internal pure returns (uint256) {

292:     function treeSize(bytes32[] memory me) internal pure returns (uint256) {

312:     function verifyPrefixProof(

359:     function verifyInclusionProof(bytes32 rootHash, bytes32 leaf, uint256 index, bytes32[] memory proof)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

14:     function leastSignificantBit(uint256 x) internal pure returns (uint256 msb) {

29:     function mostSignificantBit(uint256 x) internal pure returns (uint256 msb) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/rollup/Assertion.sol

70:     function createAssertion(

85:     function childCreated(AssertionNode storage self) internal {

93:     function requireExists(AssertionNode memory self) internal pure {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Assertion.sol)

```solidity
File: src/rollup/AssertionState.sol

18:     function toExecutionState(AssertionState memory state) internal pure returns (ExecutionState memory) {

22:     function hash(AssertionState memory state) internal pure returns (bytes32) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/AssertionState.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

99:     mapping(bytes32 => bytes) internal preImages;

341:     function cleanupOldRollup() private {

367:     function createConfig() private view returns (Config memory) {

411:     function upgradeSurroundingContracts(address newRollupAddress) private {

433:     function upgradeSequencerInbox() private {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupCore.sol

126:     function getAssertionStorage(bytes32 assertionHash) internal view returns (AssertionNode storage) {

225:     function initializeCore(AssertionNode memory initialAssertion, bytes32 assertionHash) internal {

236:     function confirmAssertionInternal(

274:     function createNewStake(address stakerAddress, uint256 depositAmount, address withdrawalAddress) internal {

286:     function increaseStakeBy(address stakerAddress, uint256 amountAdded) internal {

300:     function reduceStakeTo(address stakerAddress, uint256 target) internal returns (uint256) {

317:     function withdrawStaker(address stakerAddress) internal {

331:     function withdrawFunds(address account) internal returns (uint256) {

343:     function increaseWithdrawableFunds(address account, uint256 amount) internal {

355:     function deleteStaker(address stakerAddress) private {

365:     function createNewAssertion(

565:     function requireInactiveStaker(address stakerAddress) internal view {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

85:     function createChallengeManager(address rollupAddr, address proxyAdminAddr, Config memory config)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupLib.sol

23:     function assertionHash(

37:     function assertionHash(

54:     function configHash(

73:     function validateConfigHash(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupLib.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

366:     function receiveTokens(uint256 tokenAmount) private {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="NC-32"></a>[NC-32] Event is missing `indexed` fields
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

*Instances (17)*:
```solidity
File: src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol

9:     event StakeDeposited(address indexed sender, uint256 amount);

11:     event StakeWithdrawn(address indexed sender, uint256 amount);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/interfaces/IAssertionStakingPoolCreator.sol

12:     event NewAssertionPoolCreated(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/interfaces/IAssertionStakingPoolCreator.sol)

```solidity
File: src/bridge/ISequencerInbox.sol

41:     event SequencerBatchData(uint256 indexed batchSequenceNumber, bytes data);

44:     event SetValidKeyset(bytes32 indexed keysetHash, bytes keysetBytes);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

243:     event EdgeConfirmedByTime(bytes32 indexed edgeId, bytes32 indexed mutualId, uint256 totalTimeUnrivaled);

253:     event TimerCacheUpdated(bytes32 indexed edgeId, uint256 newValue);

260:     event EdgeRefunded(bytes32 indexed edgeId, bytes32 indexed mutualId, address stakeToken, uint256 stakeAmount);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

97:     event HashSet(bytes32 h, ExecutionState executionState, uint256 inboxMaxCount);

235:     event RollupMigrated(address rollup, address challengeManager);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/IRollupCore.sol

24:     event RollupInitialized(bytes32 machineHash, uint256 chainId);

26:     event AssertionCreated(

38:     event AssertionConfirmed(bytes32 indexed assertionHash, bytes32 blockHash, bytes32 sendRoot);

40:     event RollupChallengeStarted(

44:     event UserStakeUpdated(address indexed user, address indexed withdrawalAddress, uint256 initialBalance, uint256 finalBalance);

46:     event UserWithdrawableFundsUpdated(address indexed user, uint256 initialBalance, uint256 finalBalance);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

20:     event RollupCreated(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="NC-33"></a>[NC-33] Constants should be defined rather than using magic numbers

*Instances (10)*:
```solidity
File: src/rollup/RollupAdminLogic.sol

225:         emit OwnerFunctionCalled(12);

234:         emit OwnerFunctionCalled(22);

255:         emit OwnerFunctionCalled(23);

266:         emit OwnerFunctionCalled(24);

274:         emit OwnerFunctionCalled(25);

283:         emit OwnerFunctionCalled(26);

292:         emit OwnerFunctionCalled(27);

301:         emit OwnerFunctionCalled(28);

310:         emit OwnerFunctionCalled(30);

319:         emit OwnerFunctionCalled(31);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

### <a name="NC-34"></a>[NC-34] `public` functions not called by the contract should be declared `external` instead

*Instances (24)*:
```solidity
File: src/assertionStakingPool/AssertionStakingPoolCreator.sol

25:     function getPool(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPoolCreator.sol

25:     function getPool(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPoolCreator.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

305:     function initialize(

491:     function multiUpdateTimeCacheByChildren(bytes32[] calldata edgeIds) public {

499:     function updateTimerCacheByChildren(bytes32 edgeId) public {

505:     function updateTimerCacheByClaim(bytes32 edgeId, bytes32 claimingEdgeId) public {

511:     function confirmEdgeByTime(bytes32 edgeId, AssertionStateData calldata claimStateData) public {

539:     function confirmEdgeByOneStepProof(

572:     function refundStake(bytes32 edgeId) public {

604:     function calculateEdgeId(

616:     function calculateMutualId(

627:     function edgeExists(bytes32 edgeId) public view returns (bool) {

632:     function getEdge(bytes32 edgeId) public view returns (ChallengeEdge memory) {

637:     function edgeLength(bytes32 edgeId) public view returns (uint256) {

642:     function hasRival(bytes32 edgeId) public view returns (bool) {

647:     function confirmedRival(bytes32 mutualId) public view returns (bytes32) {

652:     function hasLengthOneRival(bytes32 edgeId) public view returns (bool) {

657:     function timeUnrivaled(bytes32 edgeId) public view returns (uint256) {

662:     function getPrevAssertionHash(bytes32 edgeId) public view returns (bytes32) {

667:     function firstRival(bytes32 mutualId) public view returns (bytes32) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

106:     function set(bytes32 h, ExecutionState calldata executionState, uint256 inboxMaxCount) public {

112:     function get(bytes32 h) public view returns (ExecutionState memory executionState, uint256 inboxMaxCount) {

173:     function array() public view returns (uint256[] memory) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupCreator.sol

137:     function createRollup(RollupDeploymentParams memory deployParams)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="NC-35"></a>[NC-35] Variables need not be initialized to zero
The default value for variables is zero, so initializing them to zero is superfluous.

*Instances (18)*:
```solidity
File: src/bridge/SequencerInbox.sol

317:         bytes32 prevDelayedAcc = 0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

492:         for (uint256 i = 0; i < edgeIds.length; i++) {

517:         uint64 assertionBlocks = 0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

15:         for (uint256 i = 0; i < arr.length; i++) {

47:         for (uint256 i = 0; i < arr1.length; i++) {

50:         for (uint256 i = 0; i < arr2.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

114:         bytes32 accum = 0;

115:         for (uint256 i = 0; i < me.length; i++) {

188:         for (uint256 i = 0; i < me.length; i++) {

293:         uint256 sum = 0;

294:         for (uint256 i = 0; i < me.length; i++) {

326:         uint256 proofIndex = 0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

350:         for (uint64 i = 0; i < stakerCount; i++) {

528:             for (uint256 i = 0; i < validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

184:         for (uint256 i = 0; i < _validator.length; i++) {

230:         for (uint256 i = 0; i < staker.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

225:         for (uint256 i = 0; i < deployParams.batchPosters.length; i++) {

235:             for (uint256 i = 0; i < deployParams.validators.length; i++) {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)


## Low Issues


| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | Use of `tx.origin` is unsafe in almost every context | 4 |
| [L-2](#L-2) | Use a 2-step ownership transfer pattern | 3 |
| [L-3](#L-3) | Some tokens may revert when zero value transfers are made | 9 |
| [L-4](#L-4) | Missing checks for `address(0)` when assigning values to address state variables | 4 |
| [L-5](#L-5) | `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()` | 3 |
| [L-6](#L-6) | Use of `tx.origin` is unsafe in almost every context | 4 |
| [L-7](#L-7) | Do not leave an implementation contract uninitialized | 1 |
| [L-8](#L-8) | Division by zero not prevented | 2 |
| [L-9](#L-9) | Duplicate import statements | 2 |
| [L-10](#L-10) | Empty `receive()/payable fallback()` function does not authenticate requests | 1 |
| [L-11](#L-11) | External call recipient may consume all transaction gas | 1 |
| [L-12](#L-12) | Fallback lacking `payable` | 3 |
| [L-13](#L-13) | Initializers could be front-run | 21 |
| [L-14](#L-14) | `pragma experimental ABIEncoderV2` is deprecated | 1 |
| [L-15](#L-15) | Loss of precision | 3 |
| [L-16](#L-16) | Solidity version 0.8.20+ may not work on other chains due to `PUSH0` | 24 |
| [L-17](#L-17) | Use `Ownable2Step.transferOwnership` instead of `Ownable.transferOwnership` | 5 |
| [L-18](#L-18) | File allows a version of solidity that is susceptible to an assembly optimizer bug | 17 |
| [L-19](#L-19) | Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when downcasting | 7 |
| [L-20](#L-20) | Unspecific compiler version pragma | 2 |
| [L-21](#L-21) | Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions | 25 |
| [L-22](#L-22) | Upgradeable contract not initialized | 57 |
### <a name="L-1"></a>[L-1] Use of `tx.origin` is unsafe in almost every context
According to [Vitalik Buterin](https://ethereum.stackexchange.com/questions/196/how-do-i-make-my-dapp-serenity-proof), contracts should _not_ `assume that tx.origin will continue to be usable or meaningful`. An example of this is [EIP-3074](https://eips.ethereum.org/EIPS/eip-3074#allowing-txorigin-as-signer-1) which explicitly mentions the intention to change its semantics when it's used with new op codes. There have also been calls to [remove](https://github.com/ethereum/solidity/issues/683) `tx.origin`, and there are [security issues](solidity.readthedocs.io/en/v0.4.24/security-considerations.html#tx-origin) associated with using it for authorization. For these reasons, it's best to completely avoid the feature.

*Instances (4)*:
```solidity
File: src/bridge/SequencerInbox.sol

375:         if (msg.sender != tx.origin) revert NotOrigin();

440:         if (msg.sender != tx.origin) revert NotOrigin();

507:         if (msg.sender == tx.origin && !isUsingFeeToken) {

764:         address batchPoster = tx.origin;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

### <a name="L-2"></a>[L-2] Use a 2-step ownership transfer pattern
Recommend considering implementing a two step process where the owner or admin nominates an account and the nominated account needs to call an `acceptOwnership()` function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account. Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

*Instances (3)*:
```solidity
File: src/rollup/BridgeCreator.sol

21: contract BridgeCreator is Ownable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/IRollupLogic.sol

12: interface IRollupUser is IRollupCore, IOwnable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

17: contract RollupCreator is Ownable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="L-3"></a>[L-3] Some tokens may revert when zero value transfers are made
Example: https://github.com/d-xo/weird-erc20#revert-on-zero-value-transfers.

In spite of the fact that EIP-20 [states](https://github.com/ethereum/EIPs/blob/46b9b698815abbfa628cd1097311deee77dd45c5/EIPS/eip-20.md?plain=1#L116) that zero-valued transfers must be accepted, some tokens, such as LEND will revert if this is attempted, which may cause transactions that involve other tokens (such as batch operations) to fully revert. Consider skipping the transfer if the amount is zero, which will also save gas.

*Instances (9)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

35:         IERC20(stakeToken).safeTransferFrom(msg.sender, address(this), amount);

51:         IERC20(stakeToken).safeTransfer(msg.sender, amount);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

434:             st.safeTransferFrom(msg.sender, receiver, sa);

581:             st.safeTransfer(edge.staker, sa);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/RollupCreator.sol

312:             IERC20(_nativeToken).safeTransferFrom(msg.sender, _inbox, totalFee);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

215:             IERC20(stakeToken).safeTransfer(loserStakeEscrow, assertion.beforeStateData.configData.requiredStake);

295:                 IERC20(stakeToken).safeTransfer(loserStakeEscrow, assertion.beforeStateData.configData.requiredStake);

362:         IERC20(stakeToken).safeTransfer(msg.sender, amount);

367:         IERC20(stakeToken).safeTransferFrom(msg.sender, address(this), tokenAmount);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="L-4"></a>[L-4] Missing checks for `address(0)` when assigning values to address state variables

*Instances (4)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

25:         stakeToken = _stakeToken;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

35:         rollup = _rollup;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

39:         challengeManager = _challengeManager;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

```solidity
File: src/bridge/SequencerInbox.sol

943:         batchPosterManager = newBatchPosterManager;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

### <a name="L-5"></a>[L-5] `abi.encodePacked()` should not be used with dynamic types when passing the result to a hash function such as `keccak256()`
Use `abi.encode()` instead which will pad items to 32 bytes, which will [prevent hash collisions](https://docs.soliditylang.org/en/v0.8.13/abi-spec.html#non-standard-packed-mode) (e.g. `abi.encodePacked(0x123,0x456)` => `0x123456` => `abi.encodePacked(0x1,0x23456)`, but `abi.encode(0x123,0x456)` => `0x0...1230...456`). "Unless there is a compelling reason, `abi.encode` should be preferred". If there is only one argument to `abi.encodePacked()` it can often be cast to `bytes()` or `bytes32()` [instead](https://ethereum.stackexchange.com/questions/30912/how-to-compare-strings-in-solidity#answer-82739).
If all arguments are strings and or bytes, `bytes.concat()` should be used instead

*Instances (3)*:
```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

14:         bytes32 bytecodeHash = keccak256(abi.encodePacked(creationCode, args));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/bridge/SequencerInbox.sol

744:             keccak256(bytes.concat(header, DATA_BLOB_HEADER_FLAG, abi.encodePacked(dataHashes))),

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

135:     bytes32 public constant UNRIVALED = keccak256(abi.encodePacked("UNRIVALED"));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

### <a name="L-6"></a>[L-6] Use of `tx.origin` is unsafe in almost every context
According to [Vitalik Buterin](https://ethereum.stackexchange.com/questions/196/how-do-i-make-my-dapp-serenity-proof), contracts should _not_ `assume that tx.origin will continue to be usable or meaningful`. An example of this is [EIP-3074](https://eips.ethereum.org/EIPS/eip-3074#allowing-txorigin-as-signer-1) which explicitly mentions the intention to change its semantics when it's used with new op codes. There have also been calls to [remove](https://github.com/ethereum/solidity/issues/683) `tx.origin`, and there are [security issues](solidity.readthedocs.io/en/v0.4.24/security-considerations.html#tx-origin) associated with using it for authorization. For these reasons, it's best to completely avoid the feature.

*Instances (4)*:
```solidity
File: src/bridge/SequencerInbox.sol

375:         if (msg.sender != tx.origin) revert NotOrigin();

440:         if (msg.sender != tx.origin) revert NotOrigin();

507:         if (msg.sender == tx.origin && !isUsingFeeToken) {

764:         address batchPoster = tx.origin;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

### <a name="L-7"></a>[L-7] Do not leave an implementation contract uninitialized
An uninitialized implementation contract can be taken over by an attacker, which may impact the proxy. To prevent the implementation contract from being used, it's advisable to invoke the `_disableInitializers` function in the constructor to automatically lock it when it is deployed. This should look similar to this:
```solidity
  /// @custom:oz-upgrades-unsafe-allow constructor
  constructor() {
      _disableInitializers();
  }
```

Sources:
- https://docs.openzeppelin.com/contracts/4.x/api/proxy#Initializable-_disableInitializers--
- https://twitter.com/0xCygaar/status/1621417995905167360?s=20

*Instances (1)*:
```solidity
File: src/bridge/SequencerInbox.sol

140:     constructor(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

### <a name="L-8"></a>[L-8] Division by zero not prevented
The divisions below take an input parameter which does not have any zero-value checks, which may lead to the functions reverting when zero is passed.

*Instances (2)*:
```solidity
File: src/bridge/SequencerInbox.sol

746:             block.basefee > 0 ? blobCost / block.basefee : 0

771:             extraGas += l1Fees / block.basefee;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

### <a name="L-9"></a>[L-9] Duplicate import statements

*Instances (2)*:
```solidity
File: src/rollup/RollupUserLogic.sol

9: import {IRollupUser} from "./IRollupLogic.sol";

12: import "./IRollupLogic.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="L-10"></a>[L-10] Empty `receive()/payable fallback()` function does not authenticate requests
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds. If the concern is having to spend a small amount of gas to check the sender against an immutable address, the code should at least have a function to rescue unused Ether.

*Instances (1)*:
```solidity
File: src/rollup/RollupCreator.sol

61:     receive() external payable {}

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="L-11"></a>[L-11] External call recipient may consume all transaction gas
There is no limit specified on the amount of gas used, so the recipient can use up all of the transaction's gas, causing it to revert. Use `addr.call{gas: <amount>}("")` or [this](https://github.com/nomad-xyz/ExcessivelySafeCall) library instead.

*Instances (1)*:
```solidity
File: src/rollup/RollupCreator.sol

304:             (bool sent, ) = msg.sender.call{value: address(this).balance}("");

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="L-12"></a>[L-12] Fallback lacking `payable`

*Instances (3)*:
```solidity
File: src/rollup/RollupProxy.sol

7: import "../libraries/AdminFallbackProxy.sol";

11: contract RollupProxy is AdminFallbackProxy {

32:             _fallback();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

### <a name="L-13"></a>[L-13] Initializers could be front-run
Initializers could be front-run, allowing an attacker to either set their own values, take ownership of the contract, and in the best case forcing a re-deployment

*Instances (21)*:
```solidity
File: src/bridge/ISequencerInbox.sol

287:     function initialize(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

```solidity
File: src/bridge/SequencerInbox.sol

177:     function initialize(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

34:     function initialize(

305:     function initialize(

316:     ) public initializer {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

503:         challengeManager.initialize({

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

118:             IEthBridge(address(frame.bridge)).initialize(IOwnable(rollup));

120:             IERC20Bridge(address(frame.bridge)).initialize(IOwnable(rollup), nativeToken);

122:         frame.sequencerInbox.initialize(IBridge(frame.bridge), maxTimeVariation, bufferConfig);

123:         frame.inbox.initialize(frame.bridge, frame.sequencerInbox);

124:         frame.rollupEventInbox.initialize(frame.bridge);

125:         frame.outbox.initialize(frame.bridge);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/IRollupAdmin.sol

16:     function initialize(Config calldata config, ContractDependencies calldata connectedContracts) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupAdmin.sol)

```solidity
File: src/rollup/IRollupLogic.sol

15:     function initialize(address stakeToken) external view;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

18:     function initialize(Config calldata config, ContractDependencies calldata connectedContracts)

22:         initializer

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

226:         __Pausable_init();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

99:         challengeManager.initialize({

282:         upgradeExecutor.initialize(address(upgradeExecutor), executors);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupProxy.sol

20:             _initialize(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

27:     function initialize(address _stakeToken) external view override onlyProxy {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="L-14"></a>[L-14] `pragma experimental ABIEncoderV2` is deprecated
Use `pragma abicoder v2` [instead](https://github.com/ethereum/solidity/blob/69411436139acf5dbcfc5828446f18b9fcfee32c/docs/080-breaking-changes.rst#silent-changes-of-the-semantics)

*Instances (1)*:
```solidity
File: src/bridge/ISequencerInbox.sol

7: pragma experimental ABIEncoderV2;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

### <a name="L-15"></a>[L-15] Loss of precision
Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator

*Instances (3)*:
```solidity
File: src/bridge/DelayBuffer.sol

46:         buffer += (elapsed * replenishRateInBasis) / BASIS;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/SequencerInbox.sol

746:             block.basefee > 0 ? blobCost / block.basefee : 0

771:             extraGas += l1Fees / block.basefee;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

### <a name="L-16"></a>[L-16] Solidity version 0.8.20+ may not work on other chains due to `PUSH0`
The compiler for Solidity 0.8.20 switches the default target EVM version to [Shanghai](https://blog.soliditylang.org/2023/05/10/solidity-0.8.20-release-announcement/#important-note), which includes the new `PUSH0` op code. This op code may not yet be implemented on all L2s, so deployment on these chains will fail. To work around this issue, use an earlier [EVM](https://docs.soliditylang.org/en/v0.8.20/using-the-compiler.html?ref=zaryabs.com#setting-the-evm-version-to-target) [version](https://book.getfoundry.sh/reference/config/solidity-compiler#evm_version). While the project itself may or may not compile with 0.8.20, other projects with which it integrates, or which extend this project may, and those projects will have problems deploying these contracts/libraries.

*Instances (24)*:
```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

6: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPoolCreator.sol

6: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPoolCreator.sol

6: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

6: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/bridge/DelayBuffer.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/SequencerInbox.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/ArrayUtilsLib.sol

5: pragma solidity ^0.8.17;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ArrayUtilsLib.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

5: pragma solidity ^0.8.17;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/ChallengeErrors.sol

5: pragma solidity ^0.8.17;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeErrors.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

5: pragma solidity ^0.8.17;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/challengeV2/libraries/Enums.sol

5: pragma solidity ^0.8.17;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/Enums.sol)

```solidity
File: src/challengeV2/libraries/MerkleTreeLib.sol

5: pragma solidity ^0.8.17;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/MerkleTreeLib.sol)

```solidity
File: src/challengeV2/libraries/UintUtilsLib.sol

5: pragma solidity ^0.8.17;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/UintUtilsLib.sol)

```solidity
File: src/libraries/Error.sol

5: pragma solidity ^0.8.4;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/libraries/Error.sol)

```solidity
File: src/rollup/Assertion.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Assertion.sol)

```solidity
File: src/rollup/AssertionState.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/AssertionState.sol)

```solidity
File: src/rollup/BridgeCreator.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/Config.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Config.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupLib.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupLib.sol)

```solidity
File: src/rollup/RollupProxy.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="L-17"></a>[L-17] Use `Ownable2Step.transferOwnership` instead of `Ownable.transferOwnership`
Use [Ownable2Step.transferOwnership](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol) which is safer. Use it as it is more secure due to 2-stage ownership transfer.

**Recommended Mitigation Steps**

Use <a href="https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol">Ownable2Step.sol</a>
  
  ```solidity
      function acceptOwnership() external {
          address sender = _msgSender();
          require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
          _transferOwnership(sender);
      }
```

*Instances (5)*:
```solidity
File: src/rollup/BridgeCreator.sol

18: import "@openzeppelin/contracts/access/Ownable.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/IRollupAdmin.sol

10: import "../bridge/IOwnable.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupAdmin.sol)

```solidity
File: src/rollup/IRollupLogic.sol

10: import "../bridge/IOwnable.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

13: import "@openzeppelin/contracts/access/Ownable.sol";

204:         proxyAdmin.transferOwnership(address(upgradeExecutor));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="L-18"></a>[L-18] File allows a version of solidity that is susceptible to an assembly optimizer bug
In solidity versions 0.8.13 and 0.8.14, there is an [optimizer bug](https://github.com/ethereum/solidity-blog/blob/499ab8abc19391be7b7b34f88953a067029a5b45/_posts/2022-06-15-inline-assembly-memory-side-effects-bug.md) where, if the use of a variable is in a separate `assembly` block from the block in which it was stored, the `mstore` operation is optimized out, leading to uninitialized memory. The code currently does not have such a pattern of execution, but it does use `mstore`s in `assembly` blocks, so it is a risk for future changes. The affected solidity versions should be avoided if at all possible.

*Instances (17)*:
```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

6: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/AssertionStakingPoolCreator.sol

6: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPoolCreator.sol

6: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPoolCreator.sol)

```solidity
File: src/assertionStakingPool/StakingPoolCreatorUtils.sol

6: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/StakingPoolCreatorUtils.sol)

```solidity
File: src/bridge/DelayBuffer.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/SequencerInbox.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/libraries/Error.sol

5: pragma solidity ^0.8.4;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/libraries/Error.sol)

```solidity
File: src/rollup/Assertion.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Assertion.sol)

```solidity
File: src/rollup/AssertionState.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/AssertionState.sol)

```solidity
File: src/rollup/BridgeCreator.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/Config.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Config.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupLib.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupLib.sol)

```solidity
File: src/rollup/RollupProxy.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

5: pragma solidity ^0.8.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="L-19"></a>[L-19] Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when downcasting
Downcasting from `uint256`/`int256` in Solidity does not revert on overflow. This can result in undesired exploitation or bugs, since developers usually assume that overflows raise errors. [OpenZeppelin's SafeCast library](https://docs.openzeppelin.com/contracts/3.x/api/utils#SafeCast) restores this intuition by reverting the transaction when such an operation overflows. Using this library eliminates an entire class of bugs, so it's recommended to use it always. Some exceptions are acceptable like with the classic `uint256(uint160(address(variable)))`

*Instances (7)*:
```solidity
File: src/bridge/SequencerInbox.sol

648:             uint64(afterDelayedMessagesRead)

780:             uint64(extraGas)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

510:             store.edges[edgeId].totalTimeUnrivaledCache = uint64(newValue);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

83:                 nextInboxPosition: uint64(currentInboxCount)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

218:         return uint64(_stakerList.length);

275:         uint64 stakerIndex = uint64(_stakerList.length);

495:                 nextInboxPosition: uint64(nextInboxPosition)

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

### <a name="L-20"></a>[L-20] Unspecific compiler version pragma

*Instances (2)*:
```solidity
File: src/bridge/DelayBufferTypes.sol

7: pragma solidity >=0.6.9 <0.9.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBufferTypes.sol)

```solidity
File: src/bridge/ISequencerInbox.sol

6: pragma solidity >=0.6.9 <0.9.0;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

### <a name="L-21"></a>[L-21] Upgradeable contract is missing a `__gap[50]` storage variable to allow for new storage variables in later versions
See [this](https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps) link for a description of this storage variable. While some contracts may not currently be sub-classed, adding the variable now protects against forgetting to add it in the future.

*Instances (25)*:
```solidity
File: src/challengeV2/EdgeChallengeManager.sol

14: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

7: import "@openzeppelin/contracts-upgradeable/utils/Create2Upgradeable.sol";

362:         DoubleLogicUUPSUpgradeable(address(OLD_ROLLUP)).upgradeSecondaryTo(IMPL_PATCHED_OLD_ROLLUP_USER);

415:         TransparentUpgradeableProxy bridge = TransparentUpgradeableProxy(payable(BRIDGE));

421:         TransparentUpgradeableProxy inbox = TransparentUpgradeableProxy(payable(INBOX));

424:         TransparentUpgradeableProxy rollupEventInbox = TransparentUpgradeableProxy(payable(REI));

428:         TransparentUpgradeableProxy outbox = TransparentUpgradeableProxy(payable(OUTBOX));

434:         TransparentUpgradeableProxy sequencerInbox = TransparentUpgradeableProxy(payable(SEQ_INBOX));

474:                 new TransparentUpgradeableProxy(

500:             Create2Upgradeable.computeAddress(rollupSalt, keccak256(type(RollupProxy).creationCode));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

70:             address(new TransparentUpgradeableProxy(address(templates.bridge), adminProxy, ""))

74:                 new TransparentUpgradeableProxy(

86:             address(new TransparentUpgradeableProxy(address(templates.inbox), adminProxy, ""))

90:                 new TransparentUpgradeableProxy(address(templates.rollupEventInbox), adminProxy, "")

94:             address(new TransparentUpgradeableProxy(address(templates.outbox), adminProxy, ""))

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

12: import "../libraries/DoubleLogicUUPSUpgradeable.sol";

13: import "@openzeppelin/contracts/proxy/beacon/UpgradeableBeacon.sol";

15: contract RollupAdminLogic is RollupCore, IRollupAdmin, DoubleLogicUUPSUpgradeable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

7: import "@openzeppelin/contracts-upgradeable/security/PausableUpgradeable.sol";

22: abstract contract RollupCore is IRollupCore, PausableUpgradeable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

12: import "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol";

91:                 new TransparentUpgradeableProxy(

273:                 new TransparentUpgradeableProxy(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

10: import "../libraries/UUPSNotUpgradeable.sol";

15: contract RollupUserLogic is RollupCore, UUPSNotUpgradeable, IRollupUser {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="L-22"></a>[L-22] Upgradeable contract not initialized
Upgradeable contracts are initialized via an initializer function rather than by a constructor. Leaving such a contract uninitialized may lead to it being taken over by a malicious user

*Instances (57)*:
```solidity
File: src/bridge/ISequencerInbox.sol

287:     function initialize(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/ISequencerInbox.sol)

```solidity
File: src/bridge/SequencerInbox.sol

177:     function initialize(

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/EdgeChallengeManager.sol

14: import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

34:     function initialize(

301:         _disableInitializers();

305:     function initialize(

316:     ) public initializer {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/EdgeChallengeManager.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

7: import "@openzeppelin/contracts-upgradeable/utils/Create2Upgradeable.sol";

362:         DoubleLogicUUPSUpgradeable(address(OLD_ROLLUP)).upgradeSecondaryTo(IMPL_PATCHED_OLD_ROLLUP_USER);

415:         TransparentUpgradeableProxy bridge = TransparentUpgradeableProxy(payable(BRIDGE));

421:         TransparentUpgradeableProxy inbox = TransparentUpgradeableProxy(payable(INBOX));

424:         TransparentUpgradeableProxy rollupEventInbox = TransparentUpgradeableProxy(payable(REI));

428:         TransparentUpgradeableProxy outbox = TransparentUpgradeableProxy(payable(OUTBOX));

434:         TransparentUpgradeableProxy sequencerInbox = TransparentUpgradeableProxy(payable(SEQ_INBOX));

474:                 new TransparentUpgradeableProxy(

500:             Create2Upgradeable.computeAddress(rollupSalt, keccak256(type(RollupProxy).creationCode));

503:         challengeManager.initialize({

524:         rollup.initializeProxy(config, connectedContracts);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

70:             address(new TransparentUpgradeableProxy(address(templates.bridge), adminProxy, ""))

74:                 new TransparentUpgradeableProxy(

86:             address(new TransparentUpgradeableProxy(address(templates.inbox), adminProxy, ""))

90:                 new TransparentUpgradeableProxy(address(templates.rollupEventInbox), adminProxy, "")

94:             address(new TransparentUpgradeableProxy(address(templates.outbox), adminProxy, ""))

118:             IEthBridge(address(frame.bridge)).initialize(IOwnable(rollup));

120:             IERC20Bridge(address(frame.bridge)).initialize(IOwnable(rollup), nativeToken);

122:         frame.sequencerInbox.initialize(IBridge(frame.bridge), maxTimeVariation, bufferConfig);

123:         frame.inbox.initialize(frame.bridge, frame.sequencerInbox);

124:         frame.rollupEventInbox.initialize(frame.bridge);

125:         frame.outbox.initialize(frame.bridge);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/IRollupAdmin.sol

16:     function initialize(Config calldata config, ContractDependencies calldata connectedContracts) external;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupAdmin.sol)

```solidity
File: src/rollup/IRollupCore.sol

24:     event RollupInitialized(bytes32 machineHash, uint256 chainId);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupCore.sol)

```solidity
File: src/rollup/IRollupLogic.sol

15:     function initialize(address stakeToken) external view;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

12: import "../libraries/DoubleLogicUUPSUpgradeable.sol";

13: import "@openzeppelin/contracts/proxy/beacon/UpgradeableBeacon.sol";

15: contract RollupAdminLogic is RollupCore, IRollupAdmin, DoubleLogicUUPSUpgradeable {

18:     function initialize(Config calldata config, ContractDependencies calldata connectedContracts)

22:         initializer

37:             connectedContracts.rollupEventInbox.rollupInitialized(config.chainId, config.chainConfig);

86:         initializeCore(initialAssertion, genesisHash);

105:         emit RollupInitialized(config.wasmModuleRoot, config.chainId);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupCore.sol

7: import "@openzeppelin/contracts-upgradeable/security/PausableUpgradeable.sol";

22: abstract contract RollupCore is IRollupCore, PausableUpgradeable {

225:     function initializeCore(AssertionNode memory initialAssertion, bytes32 assertionHash) internal {

226:         __Pausable_init();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCore.sol)

```solidity
File: src/rollup/RollupCreator.sol

12: import "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol";

91:                 new TransparentUpgradeableProxy(

99:         challengeManager.initialize({

209:         rollup.initializeProxy(

273:                 new TransparentUpgradeableProxy(

282:         upgradeExecutor.initialize(address(upgradeExecutor), executors);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

```solidity
File: src/rollup/RollupProxy.sol

12:     function initializeProxy(Config memory config, ContractDependencies memory connectedContracts)

20:             _initialize(

23:                     IRollupAdmin.initialize,

28:                 abi.encodeCall(IRollupUser.initialize, (config.stakeToken)),

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupProxy.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

10: import "../libraries/UUPSNotUpgradeable.sol";

15: contract RollupUserLogic is RollupCore, UUPSNotUpgradeable, IRollupUser {

27:     function initialize(address _stakeToken) external view override onlyProxy {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)


## Medium Issues


| |Issue|Instances|
|-|:-|:-:|
| [M-1](#M-1) | Contracts are vulnerable to fee-on-transfer accounting-related issues | 2 |
| [M-2](#M-2) | `block.number` means different things on different L2s | 21 |
| [M-3](#M-3) | Centralization Risk for trusted owners | 15 |
| [M-4](#M-4) | `increaseAllowance/decreaseAllowance` won't work on mainnet for USDT | 2 |
| [M-5](#M-5) | Lack of EIP-712 compliance: using `keccak256()` directly on an array or struct variable | 3 |
### <a name="M-1"></a>[M-1] Contracts are vulnerable to fee-on-transfer accounting-related issues
Consistently check account balance before and after transfers for Fee-On-Transfer discrepancies. As arbitrary ERC20 tokens can be used, the amount here should be calculated every time to take into consideration a possible fee-on-transfer or deflation.
Also, it's a good practice for the future of the solution.

Use the balance before and after the transfer to calculate the received amount instead of assuming that it would be equal to the amount passed as a parameter. Or explicitly document that such tokens shouldn't be used and won't be supported

*Instances (2)*:
```solidity
File: src/assertionStakingPool/AbsBoldStakingPool.sol

35:         IERC20(stakeToken).safeTransferFrom(msg.sender, address(this), amount);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AbsBoldStakingPool.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

367:         IERC20(stakeToken).safeTransferFrom(msg.sender, address(this), tokenAmount);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="M-2"></a>[M-2] `block.number` means different things on different L2s
On Optimism, `block.number` is the L2 block number, but on Arbitrum, it's the L1 block number, and `ArbSys(address(100)).arbBlockNumber()` must be used. Furthermore, L2 block numbers often occur much more frequently than L1 block numbers (any may even occur on a per-transaction basis), so using block numbers for timing results in inconsistencies, especially when voting is involved across multiple chains. As of version 4.9, OpenZeppelin has [modified](https://blog.openzeppelin.com/introducing-openzeppelin-contracts-v4.9#governor) their governor code to use a clock rather than block numbers, to avoid these sorts of issues, but this still requires that the project [implement](https://docs.openzeppelin.com/contracts/4.x/governance#token_2) a [clock](https://eips.ethereum.org/EIPS/eip-6372) for each L2.

*Instances (21)*:
```solidity
File: src/bridge/DelayBuffer.sol

74:         self.prevSequencedBlockNumber = uint64(block.number);

105:         return block.number - self.prevBlockNumber <= self.threshold;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/DelayBuffer.sol)

```solidity
File: src/bridge/SequencerInbox.sol

228:         if (block.number > delayBlocks_) {

229:             bounds.minBlockNumber = uint64(block.number) - delayBlocks_;

231:         bounds.maxBlockNumber = uint64(block.number) + futureBlocks_;

314:         if (l1BlockAndTime[0] + delayBlocks_ >= block.number) revert ForceIncludeBlockTooSoon();

863:             buffer.update(uint64(block.number));

909:         uint256 creationBlock = block.number;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/challengeV2/libraries/ChallengeEdgeLib.sol

120:             createdAtBlock: uint64(block.number),

151:             createdAtBlock: uint64(block.number),

254:         edge.confirmedAtBlock = uint64(block.number);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/ChallengeEdgeLib.sol)

```solidity
File: src/challengeV2/libraries/EdgeChallengeManagerLib.sol

552:             return block.number - store.edges[edgeId].createdAtBlock;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/challengeV2/libraries/EdgeChallengeManagerLib.sol)

```solidity
File: src/rollup/Assertion.sol

75:         assertion.createdAtBlock = uint64(block.number);

87:             self.firstChildBlock = uint64(block.number);

89:             self.secondChildBlock = uint64(block.number);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/Assertion.sol)

```solidity
File: src/rollup/RollupAdminLogic.sol

24:         rollupDeploymentBlock = block.number;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupAdminLogic.sol)

```solidity
File: src/rollup/RollupUserLogic.sol

57:             return latestConfirmedAssertion.firstChildBlock + VALIDATOR_AFK_BLOCKS < block.number;

59:         return latestConfirmedAssertion.createdAtBlock + VALIDATOR_AFK_BLOCKS < block.number;

110:         require(block.number >= assertion.createdAtBlock + prevConfig.confirmPeriodBlocks, "BEFORE_DEADLINE");

125:                 block.number >= winningEdge.confirmedAtBlock + challengeGracePeriodBlocks,

205:             uint256 timeSincePrev = block.number - getAssertionStorage(prevAssertion).createdAtBlock;

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupUserLogic.sol)

### <a name="M-3"></a>[M-3] Centralization Risk for trusted owners

#### Impact:
Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

*Instances (15)*:
```solidity
File: src/bridge/SequencerInbox.sol

93:     IOwnable public rollup;

209:         if (msg.sender != IOwnable(rollup).owner())

210:             revert NotOwner(msg.sender, IOwnable(rollup).owner());

211:         IOwnable newRollup = bridge.rollup();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/bridge/SequencerInbox.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

417:         IBridge(BRIDGE).updateRollupAddress(IOwnable(newRollupAddress));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/BridgeCreator.sol

21: contract BridgeCreator is Ownable {

48:     ) Ownable() {

53:     function updateTemplates(BridgeTemplates calldata _newTemplates) external onlyOwner {

58:     function updateERC20Templates(BridgeTemplates calldata _newTemplates) external onlyOwner {

118:             IEthBridge(address(frame.bridge)).initialize(IOwnable(rollup));

120:             IERC20Bridge(address(frame.bridge)).initialize(IOwnable(rollup), nativeToken);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BridgeCreator.sol)

```solidity
File: src/rollup/IRollupLogic.sol

12: interface IRollupUser is IRollupCore, IOwnable {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/IRollupLogic.sol)

```solidity
File: src/rollup/RollupCreator.sol

17: contract RollupCreator is Ownable {

58:     constructor() Ownable() {}

72:     ) external onlyOwner {

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)

### <a name="M-4"></a>[M-4] `increaseAllowance/decreaseAllowance` won't work on mainnet for USDT
On mainnet, the mitigation to be compatible with `increaseAllowance/decreaseAllowance` isn't applied: https://etherscan.io/token/0xdac17f958d2ee523a2206206994597c13d831ec7#code, meaning it reverts on setting a non-zero & non-max allowance, unless the allowance is already zero.

*Instances (2)*:
```solidity
File: src/assertionStakingPool/AssertionStakingPool.sol

43:         IERC20(stakeToken).safeIncreaseAllowance(rollup, requiredStake);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/AssertionStakingPool.sol)

```solidity
File: src/assertionStakingPool/EdgeStakingPool.sol

46:         IERC20(stakeToken).safeIncreaseAllowance(address(challengeManager), requiredStake);

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/assertionStakingPool/EdgeStakingPool.sol)

### <a name="M-5"></a>[M-5] Lack of EIP-712 compliance: using `keccak256()` directly on an array or struct variable
Directly using the actual variable instead of encoding the array values goes against the EIP-712 specification https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md#definition-of-encodedata. 
**Note**: OpenSea's [Seaport's example with offerHashes and considerationHashes](https://github.com/ProjectOpenSea/seaport/blob/a62c2f8f484784735025d7b03ccb37865bc39e5a/reference/lib/ReferenceGettersAndDerivers.sol#L130-L131) can be used as a reference to understand how array of structs should be encoded.

*Instances (3)*:
```solidity
File: src/rollup/AssertionState.sol

23:         return keccak256(abi.encode(state));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/AssertionState.sol)

```solidity
File: src/rollup/BOLDUpgradeAction.sol

498:         bytes32 rollupSalt = keccak256(abi.encode(config));

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/BOLDUpgradeAction.sol)

```solidity
File: src/rollup/RollupCreator.sol

188:         RollupProxy rollup = new RollupProxy{salt: keccak256(abi.encode(deployParams))}();

```
[Link to code](https://github.com/code-423n4/2024-05-arbitrum-foundation/tree/main/src/rollup/RollupCreator.sol)
