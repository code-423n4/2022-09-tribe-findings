* [QA REPORT](#qa-report)
        * [Loss of precision: multiplications should be before divisions](#loss-of-precision-multiplications-should-be-before-divisions)
        * [Use safeTransfer() instead transfer()](#use-safetransfer-instead-transfer)
        * [Missing zero address check for initializers functions](#missing-zero-address-check-for-initializers-functions)
        * [Array access is out of bounds](#array-access-is-out-of-bounds)
        * [Unused success return value](#unused-success-return-value)
        * [Use timelock modifier for setter functions](#use-timelock-modifier-for-setter-functions)
        * [Should approve(0) first](#should-approve0-first)
        * [Use safe math for solidity version <8](#use-safe-math-for-solidity-version-8)
        * [Conditions that are based on block number](#conditions-that-are-based-on-block-number)
        * [Missing 0 address check at transfer](#missing-0-address-check-at-transfer)
        * [Missing two steps verification process](#missing-two-steps-verification-process)
        * [Approve return value is ignored](#approve-return-value-is-ignored)
        * [Missing zero address check in a state variable setter function](#missing-zero-address-check-in-a-state-variable-setter-function)
        * [Make sure the following functions has to be payable](#make-sure-the-following-functions-has-to-be-payable)
        * [Several functions are declaring named returns but then are using return statements. I suggest choosing only one for readability reasons.](#several-functions-are-declaring-named-returns-but-then-are-using-return-statements-i-suggest-choosing-only-one-for-readability-reasons)
        * [Add event to the following functions](#add-event-to-the-following-functions)
        * [Missing an event after critical initialize() functions](#missing-an-event-after-critical-initialize-functions)
        * [Consider adding constant variables instead of hardcoded strings](#consider-adding-constant-variables-instead-of-hardcoded-strings)
        * [Events not emitted for important state changes](#events-not-emitted-for-important-state-changes)

# QA REPORT

## Loss of precision: multiplications should be before divisions
Consider changing the order of the following instances math operators such that multiplications comes before divisions to improve calculation precision with no cost.

### Code Instances:
- [UniswapPCVDeposit.sol#L153](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/uniswap/UniswapPCVDeposit.sol#L153)
- [VoteEscrowTokenManager.sol#L75](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/metagov/utils/VoteEscrowTokenManager.sol#L75)
- [UniswapLens.sol#L62](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/uniswap/UniswapLens.sol#L62)
- [PCVEquityMinter.sol#L64](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fei/minter/PCVEquityMinter.sol#L64)

## Use safeTransfer() instead transfer()
Use openzeppelin safeTransfer() method instead of transfer() in the following locations.

### Code Instances:
- [UniswapLiquidityRemover.t.sol#L101](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/utils/UniswapLiquidityRemover.t.sol#L101)
- [ConvexPCVDeposit.sol#L81](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/convex/ConvexPCVDeposit.sol#L81)
- [AngleEuroRedeemer.sol#L108](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/angle/AngleEuroRedeemer.sol#L108)
- [CurvePCVDepositPlainPool.sol#L119](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/curve/CurvePCVDepositPlainPool.sol#L119)

## Missing zero address check for initializers functions
Missing checks for zero-addresses may lead to infunctional protocol. In this case the function is an initializer then the value can be passed only once and is important to be validated. If the variable addresses are updated incorrectly.

For instance, [TribalChief.sol#L140](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/TribalChief.sol#L140)

## Array access is out of bounds
There is no check for the access to be in the array bounds.

### Code Instances:
- [TribalChief.sol#L339](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/TribalChief.sol#L339)
- [BalancerLBPSwapper.sol#L260](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerLBPSwapper.sol#L260)
- [BPTLens.sol#L136](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BPTLens.sol#L136)
- [rariMerkleRedeemer.t.sol#L566](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/shutdown/fuse/rariMerkleRedeemer.t.sol#L566)

## Unused success return value
The following calls ignores the return value of the called function that might indicate the the call failed.

### Code Instances:
- [LiquidityGaugeManager.sol#L128](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/metagov/utils/LiquidityGaugeManager.sol#L128)
- [StakingTokenWrapper.sol#L36](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/StakingTokenWrapper.sol#L36)
- [TribeMinter.sol#L182](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/tribe/TribeMinter.sol#L182)
- [ReserveStabilizer.sol#L80](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/tribe/stabilizer/ReserveStabilizer.sol#L80)

## Use timelock modifier for setter functions
It is good to have a timelock for functions that set key/critical variables.

### Code Instances:
- [FeiDAO.sol#L63](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/dao/governor/FeiDAO.sol#L63)
- [OracleRef.sol#L50](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/refs/OracleRef.sol#L50)
- [CollateralizationOracleWrapper.sol#L106](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/oracle/collateralization/CollateralizationOracleWrapper.sol#L106)
- [TribeReserveStabilizer.sol#L103](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/tribe/stabilizer/TribeReserveStabilizer.sol#L103)

## Should approve(0) first
Some tokens (like USDT L199) do not work when changing the allowance from an existing non-zero allowance value.
They must first be approved by zero and then the actual allowance must be approved.

### Code Instances:
- [PSMRouter.sol#L25](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/PSMRouter.sol#L25)
- [ConvexPCVDeposit.sol#L73](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/convex/ConvexPCVDeposit.sol#L73)
- [rariMerkleRedeemer.t.sol#L344](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/shutdown/fuse/rariMerkleRedeemer.t.sol#L344)
- [LiquidityGaugeManager.sol#L128](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/metagov/utils/LiquidityGaugeManager.sol#L128)

## Use safe math for solidity version <8
You should use safe math for solidity version <8 since there is no default over/under flow check it those versions.'

For instance, [DSTest.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/utils/DSTest.sol)

## Conditions that are based on block number
The following condition statements are based on block number that can be manipulated by a malicious miner.

### Code Instances:
- [TribalChief.sol#L459](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/TribalChief.sol#L459)
- [TribalChief.sol#L344](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/TribalChief.sol#L344)

## Missing 0 address check at transfer
Some contracts does not support 0 transfer, then the transaction will revert with no explanation. We recommend to add a require statement that the amount is not 0.

### Code Instances:
- [TimelockedDelegator.sol#L77](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/timelocks/TimelockedDelegator.sol#L77)
- [RariMerkleRedeemer.sol#L249](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L249)
- [NonCustodialPSM.sol#L251](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/NonCustodialPSM.sol#L251)
- [CompoundPCVDepositBase.sol#L39](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/compound/CompoundPCVDepositBase.sol#L39)

## Missing two steps verification process
The process of transferring ownership is dangerous since typing the wrong address can lead to severe implications. It is better to have to steps verification process with set and claim functions to decrease the chances of human error. Consider changing to two steps verification process of transferring privileges. Human mistakes can happen.

### Code Instances:
- [AngleUniswapPCVDeposit.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/angle/AngleUniswapPCVDeposit.sol)
- [CoreRef.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/refs/CoreRef.sol)
- [BaseBalancerPoolManager.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/manager/BaseBalancerPoolManager.sol)
- [NopeDAO.t.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/governance/NopeDAO.t.sol)

## Approve return value is ignored
According to the ERC20 specs, the approve function is allowed to return a success value, that may be negative. Same as transfer return value, approve return value should be handled.

### Code Instances:
- [rariMerkleRedeemer.t.sol#L589](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/shutdown/fuse/rariMerkleRedeemer.t.sol#L589)
- [rariMerkleRedeemer.t.sol#L663](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/shutdown/fuse/rariMerkleRedeemer.t.sol#L663)
- [BalancerLBPSwapper.sol#L176](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerLBPSwapper.sol#L176)
- [rariMerkleRedeemer.t.sol#L344](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/shutdown/fuse/rariMerkleRedeemer.t.sol#L344)

## Missing zero address check in a state variable setter function
A state variable of type 'address' is set without a non-zero verification. This can lead to undesired behavior.

### Code Instances:
- [CollateralizationOracleWrapper.sol#L75](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/oracle/collateralization/CollateralizationOracleWrapper.sol#L75)
- [AutoRewardsDistributor.sol#L40](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fuse/rewards/AutoRewardsDistributor.sol#L40)
- [VlAuraDelegatorPCVDeposit.sol#L84](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/metagov/VlAuraDelegatorPCVDeposit.sol#L84)
- [TribeMinter.sol#L107](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/tribe/TribeMinter.sol#L107)

## Make sure the following functions has to be payable
I didn't see a use of using payable in the following functions, consider changing it.

### Code Instances:
- [EthTokemakPCVDeposit.sol#L26](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/tokemak/EthTokemakPCVDeposit.sol#L26)
- [BalancerPCVDepositBase.sol#L73](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerPCVDepositBase.sol#L73)
- [RecoverEthGuard.sol#L35](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/examples/RecoverEthGuard.sol#L35)
- [UniswapPCVDeposit.sol#L51](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/uniswap/UniswapPCVDeposit.sol#L51)

## Several functions are declaring named returns but then are using return statements. I suggest choosing only one for readability reasons.
Using both named returns and a return statement isn't necessary. Removing one of those can improve code clarity.

### Code Instances:
- [RariMerkleRedeemer.sol#L81](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81)
- [PCVSentinel.sol#L45](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/PCVSentinel.sol#L45)
- [BalanceGuard.sol#L23](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/examples/BalanceGuard.sol#L23)
- [ReEntrancyGuard.sol#L22](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/examples/ReEntrancyGuard.sol#L22)

## Add event to the following functions


### Code Instances:
- [BalancerLBPSwapper.sol#L125](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerLBPSwapper.sol#L125)
- [GOhmOracleIntegrationTest.t.sol#L30](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/oracle/GOhmOracleIntegrationTest.t.sol#L30)
- [RateLimited.sol#L37](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/utils/RateLimited.sol#L37)
- [PodExecutor.t.sol#L70](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/unit/governance/PodExecutor.t.sol#L70)

## Missing an event after critical initialize() functions
To record the initialize parameters for off-chain monitoring and transparency reasons, you might find it useful to emit an event after the initialize() functions

### Code Instances:
- [Core.sol#L20](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/core/Core.sol#L20)
- [StakingTokenWrapper.sol#L30](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/StakingTokenWrapper.sol#L30)

## Consider adding constant variables instead of hardcoded strings
A good practice is to use constant variables instead of hardcoded strings in the code.

### Code Instances:
- [Tribe.sol#L299](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/tribe/Tribe.sol#L299)
- [MultiActionGuard.sol#L36](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/examples/MultiActionGuard.sol#L36)
- [TribalChief.sol#L261](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/TribalChief.sol#L261)
- [PegStabilityModule.sol#L91](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/PegStabilityModule.sol#L91)

## Events not emitted for important state changes
When changing state variables events are not emitted. Emitting events allows monitoring activities with off-chain monitoring tools.

### Code Instances:
- [REPTbRedeemer.sol#L17](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/merger/REPTbRedeemer.sol#L17)
- [CollateralizationOracleKeeper.sol#L22](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/keeper/CollateralizationOracleKeeper.sol#L22)
- [DelayedPCVMover.sol#L40](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/utils/DelayedPCVMover.sol#L40)
- [CompoundPCVDepositBase.sol#L30](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/compound/CompoundPCVDepositBase.sol#L30)

--