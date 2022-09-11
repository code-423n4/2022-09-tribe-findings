


## Unnecessary equals boolean


Boolean variables can be checked within conditionals directly without the use of equality operators to true/false.

### Code instances:

        GovernanceMetadataRegistry.sol, 46: require(registration[proposalHash] == false, "Proposal already registered");
        TribalChief.sol, 616: require(poolDeposit.unlockBlock <= block.number || pool.unlocked == true, "tokens locked");
        BalanceGuard.sol, 10: if (address(this).balance % 2 == 0) return true;
        PegExchanger.sol, 54: require(bothPartiesAccepted == true, "Contract must be enabled before admin functions called");
        TribalChief.sol, 460: if (poolDeposit.unlockBlock > block.number && pool.unlocked == false) {
        MultiActionGuard.sol, 14: if (address(block.coinbase).balance % 2 == 0) return true;
        TribalChief.sol, 537: require(poolDeposit.unlockBlock <= block.number || pool.unlocked == true, "tokens locked");





## Rearrange state variables

You can change the order of the storage variables to decrease memory uses.

### Code instances:

In ERC20PCVDepositWrapper.sol,rearranging the storage fields can optimize to: 2 slots from: 3 slots.
The new order of types (you choose the actual variables):
        1. IERC20
        2. address
        3. bool

In MockChainlinkOracle.sol,rearranging the storage fields can optimize to: 4 slots from: 5 slots.
The new order of types (you choose the actual variables):
        1. int256
        2. uint256
        3. uint256
        4. uint80
        5. uint80
        6. uint8

In BPTLens.sol,rearranging the storage fields can optimize to: 7 slots from: 8 slots.
The new order of types (you choose the actual variables):
        1. IWeightedPool
        2. IVault
        3. bytes32
        4. uint256
        5. IOracle
        6. IOracle
        7. address
        8. bool
        9. bool

In BalancerPool2Lens.sol,rearranging the storage fields can optimize to: 9 slots from: 10 slots.
The new order of types (you choose the actual variables):
        1. IWeightedPool
        2. IVault
        3. bytes32
        4. uint256
        5. IOracle
        6. IOracle
        7. address
        8. bool
        9. bool
        10. address
        11. address

In SimpleFeiDaiPSM.sol,rearranging the storage fields can optimize to: 7 slots from: 8 slots.
The new order of types (you choose the actual variables):
        1. IERC20
        2. Fei
        3. uint256
        4. uint256
        5. uint256
        6. address
        7. bool
        8. bool
        9. bool
        10. address

In CollateralizationOracleWrapper.sol,rearranging the storage fields can optimize to: 5 slots from: 6 slots.
The new order of types (you choose the actual variables):
        1. uint256
        2. uint256
        3. int256
        4. uint256
        5. address
        6. bool

In Tribe.sol,rearranging the storage fields can optimize to: 7 slots from: 8 slots.
The new order of types (you choose the actual variables):
        1. string
        2. string
        3. uint256
        4. bytes32
        5. bytes32
        6. bytes32
        7. address
        8. uint8


## State variables that could be set immutable

In the following files there are state variables that could be set immutable to save gas. 

### Code instances:

        agToken in MockAngleStableMaster.sol
        ACC_TRIBE_PRECISION in TribalChief.sol
        token in MockERC20PCVDeposit.sol
        pid in StakingTokenWrapper.sol
        weth in MockTokemakEthPool.sol
        _initialized in MockCore.sol
        rewardsToken in MockTokemakRewards.sol
        token in MockAnglePoolManager.sol
        balToken in MockMerkleOrchard.sol
        chainlinkOracle in ChainlinkOracleWrapper.sol
        token in ERC20CompoundPCVDeposit.sol
        tribe in MockTribeMinter.sol



## Storage double reading. Could save SLOAD

Reading a storage variable is gas costly (SLOAD). In cases of multiple read of a storage variable in the same scope, caching the first read (i.e saving as a local variable) can save gas and decrease the
 overall gas uses. The following is a list of functions and the storage variables that you read twice: 

### Code instances:

        DelayedPCVMover.sol: deadline is read twice in withdrawRatio
        Tribe.sol: minter is read twice in setMinter
        Tribe.sol: minter is read twice in mint
        CurvePCVDepositPlainPool.sol: N_COINS is read twice in deposit
        UniswapLens.sol: feiIsToken0 is read twice in resistantBalanceAndFei



## Unused state variables

Unused state variables are gas consuming at deployment (since they are located in storage) and are 
a bad code practice. Removing those variables will decrease deployment gas cost and improve code quality. 
This is a full list of all the unused storage variables we found in your code base. 

### Code instances:

        MockWeightedPool.sol, getPoolId
        MultiMerkleRedeemer.sol, userSignatures
        Unitroller.sol, cTokensByUnderlying
        SimpleFeiDaiPSM.sol, mintPaused



## Unused declared local variables

Unused local variables are gas consuming, since the initial value assignment costs gas. And are 
a bad code practice. Removing those variables will decrease the gas cost and improve code quality. 
This is a full list of all the unused storage variables we found in your code base. 

### Code instances:

        PCVSentinel.sol, protec, calldatas
        UniswapPCVDeposit.sol, _wrap, amount



## Caching array length can save gas


Caching the array length is more gas efficient.
This is because access to a local variable in solidity is more efficient than query storage / calldata / memory.
We recommend to change from:    

    for (uint256 i=0; i<array.length; i++) { ... }

to: 

    uint len = array.length  
    for (uint256 i=0; i<len; i++) { ... }


### Code instances:

        CollateralizationOracle.sol, _deposits, 147
        CollateralizationOracle.sol, _deposits, 113
        RariMerkleRedeemer_flattened.sol, _cTokens, 2289
        RariMerkleRedeemer.sol, _cTokens, 193
        RariMerkleRedeemer.sol, _cTokens, 141
        NamedStaticPCVDepositWrapper.sol, newPCVDeposits, 53
        MockVault.sol, weights, 24
        TribalChief.sol, , 455



## Prefix increments are cheaper than postfix increments

Prefix increments are cheaper than postfix increments. 
Further more, using unchecked {++x} is even more gas efficient, and the gas saving accumulates every iteration and can make a real change
There is no risk of overflow caused by increamenting the iteration index in for loops (the `++i` in `for (uint256 i = 0; i < numIterations; ++i)`).
But increments perform overflow checks that are not necessary in this case.
These functions use not using prefix increments (`++x`) or not using the unchecked keyword: 

### Code instances:

        change to prefix increment and unchecked: BalancerPCVDepositBase.sol, i, 67
        change to prefix increment and unchecked: TribalChief.sol, i, 455
        change to prefix increment and unchecked: UintArrayOps.sol, i, 26
        change to prefix increment and unchecked: UintArrayOps.sol, i, 10
        change to prefix increment and unchecked: CollateralizationOracle.sol, i, 230
        change to prefix increment and unchecked: TribalChief.sol, i, 230
        change to prefix increment and unchecked: PCVGuardian.sol, i, 57
        change to prefix increment and unchecked: RariMerkleRedeemer_flattened.sol, i, 2237
        change to prefix increment and unchecked: RariMerkleRedeemer.sol, i, 247



## Unnecessary index init


In for loops you initialize the index to start from 0, but it already initialized to 0 in default and this assignment cost gas. 
It is more clear and gas efficient to declare without assigning 0 and will have the same meaning:

### Code instances:

        NamedStaticPCVDepositWrapper.sol, 53
        MaxFeiWithdrawalGuard.sol, 39
        LiquidityGaugeManager.sol, 160
        BalancerPCVDepositWeightedPool.sol, 109
        RariMerkleRedeemer.sol, 193
        PCVGuardian.sol, 82
        CurvePCVDepositPlainPool.sol, 85





## Unnecessary default assignment


Unnecessary default assignments, you can just declare and it will save gas and have the same meaning.
    

### Code instances:

        MockCurve3pool.sol (L#8) : uint256 public slippage = 0; 
        SimpleFeiDaiPSM.sol (L#92) : uint256 public constant mintFeeBasisPoints = 0;
        SimpleFeiDaiPSM.sol (L#98) : bool public constant mintPaused = false; 
        SimpleFeiDaiPSM.sol (L#93) : uint256 public constant redeemFeeBasisPoints = 0;
        SimpleFeiDaiPSM.sol (L#97) : bool public constant redeemPaused = false;
        MockUniswapIncentive.sol (L#11) : bool public isExempt = false; 
        MockUniswapIncentive.sol (L#10) : bool public isParity = false;
        MockVault.sol (L#16) : bool public mockBalancesSet = false; 
        SimpleFeiDaiPSM.sol (L#96) : bool public constant paused = false;
        MockVault.sol (L#15) : bool public mockDoTransfers = false;
        MockEthPCVDeposit.sol (L#10) : uint256 total = 0; 
        MockCurveMetapool.sol (L#8) : uint256 public slippage = 0; 
        MockConvexBaseRewardPool.sol (L#8) : uint256 public rewardAmountPerClaim = 0;





## Use bytes32 instead of string to save gas whenever possible


    Use bytes32 instead of string to save gas whenever possible.
    String is a dynamic data structure and therefore is more gas consuming then bytes32.

    
### Code instances:

        WETH9.sol (L24), string public symbol = "WETH";
        Tribe.sol (L14), string public constant symbol = "TRIBE"; 
        Tribe.sol (L10), string public constant name = "Tribe"; 
        RariMerkleRedeemer_flattened.sol (L1319), string public constant MESSAGE = "Sample message, please update."; 
        MultiMerkleRedeemer.sol (L53), string public constant MESSAGE = "Sample message, please update."; 
        WETH9.sol (L23), string public name = "Wrapped Ether";



## Short the following require messages

The following require messages are of length more than 32 and we think are short enough to short
them into exactly 32 characters such that it will be placed in one slot of memory and the require 
function will cost less gas. 
The list: 

### Code instances:

        Solidity file: PCVEquityMinter.sol, In line 61, Require message length to shorten: 34, The message: PCVEquityMinter: invalid CR oracle
        Solidity file: RariMerkleRedeemer_flattened.sol, In line 2234, Require message length to shorten: 36, The message: Must provide exactly 27 merkle roots
        Solidity file: MockUniswapV2PairLiquidity.sol, In line 322, Require message length to shorten: 34, The message: ERC20: approve to the zero address
        Solidity file: PriceBoundPSM.sol, In line 91, Require message length to shorten: 33, The message: PegStabilityModule: invalid floor


## Unnecessary cast


    
### Code instances:

        int256 ABDKMath64x64.sol.divi - unnecessary casting int256(y)
        int256 ABDKMath64x64.sol.muli - unnecessary casting int256(y)
        int128 MockCurve3pool.sol.remove_liquidity_one_coin - unnecessary casting int128(i)
        int128 MockCurveMetapool.sol.remove_liquidity_one_coin - unnecessary casting int128(i)
        GlobalRateLimitedMinter NonCustodialPSM.sol.setGlobalRateLimitedMinter - unnecessary casting GlobalRateLimitedMinter(newMinter)
        CToken FuseGuardian.sol._setBorrowPaused - unnecessary casting CToken(cToken)
        int256 ABDKMath64x64.sol.divi - unnecessary casting int256(x)
        address BalancerPCVDepositBase.sol.exitPool - unnecessary casting address(_to)
        address RatioPCVControllerV2.sol.transferFromRatio - unnecessary casting address(from)



## Use unchecked to save gas for certain additive calculations that cannot overflow


You can use unchecked in the following calculations since there is no risk to overflow:

### Code instances:

        BalancerLBPSwapper.sol (L#370) - _updateWeightsGradually(pool, block.timestamp, block.timestamp + duration, endWeights);
        BalancerLBPSwapper.sol (L#317) - _updateWeightsGradually(pool, block.timestamp, block.timestamp + duration, endWeights);
        PegExchanger.sol (L#53) - require(timestamp > (block.timestamp + MIN_EXPIRY_WINDOW), "timestamp too low");
        VoteEscrowTokenManager.sol (L#76) - uint256 lockHorizon = ((block.timestamp + lockDuration) / WEEK) * WEEK; 



## uint8 index

Due to how the EVM natively works on 256 numbers, using a 8 bit number here introduces additional costs as the EVM has to properly enforce the limits of this smaller type. 
See the warning at this link: https://docs.soliditylang.org/en/v0.8.0/internals/layout_in_storage.html#layout-of-state-variables-in-storage 
We recommend to use uint256 for the index in every for loop instead using uint8: 

### Code instance:

        ABDKMath64x64.sol, int256 bit, 425



## Consider inline the following functions to save gas


    You can inline the following functions instead of writing a specific function to save gas.
    (see https://github.com/code-423n4/2021-11-nested-findings/issues/167 for a similar issue.)

    
### Code instances

        ABDKMath64x64.sol, toInt, { return int64(x >> 64); }
        Timelock.sol, getBlockTimestamp, { return block.timestamp; }
        Decimal.sol, isZero, { return self.value == 0; }
        ABDKMath64x64.sol, to128x128, { return int256(x) << 64; }



## Inline one time use functions


The following functions are used exactly once. Therefore you can inline them and save gas and improve code clearness.
    

### Code instances:

        Tribe.sol, safe32
        RariMerkleRedeemer.sol, _configureMerkleRoots
        PCVDripController.sol, _mintFei
        PCVSplitter.sol, _allocateSingle
        NopeDAO.sol, _executor
        RateLimitedMinter.sol, _mintFei



## Cache powers of 10 used several times

You calculate the power of 10 every time you use it instead of caching it once as a constant variable and using it instead. 
Fix the following code lines: 

### Code instances:

    ExtendedMath.sol, 53 : You should cache the used power of 10 as constant state variable since it's used several times (3)
    BalancerPCVDepositWeightedPool.sol, 293 : You should cache the used power of 10 as constant state variable since it's used several times (2)
    ExtendedMath.sol, 71 : You should cache the used power of 10 as constant state variable since it's used several times (3)
    ExtendedMath.sol, 91 : You should cache the used power of 10 as constant state variable since it's used several times (3)
    BalancerPCVDepositWeightedPool.sol, 322 : You should cache the used power of 10 as constant state variable since it's used several times (2)




## Upgrade pragma to at least 0.8.4


Using newer compiler versions and the optimizer gives gas optimizations
and additional safety checks are available for free.

The advantages of versions 0.8.* over <0.8.0 are:

        1. Safemath by default from 0.8.0 (can be more gas efficient than library based safemath.)
        2. Low level inliner : from 0.8.2, leads to cheaper runtime gas. Especially relevant when the contract has small functions. For example, OpenZeppelin libraries typically have a lot of small helper functions and if they are not inlined, they cost an additional 20 to 40 gas because of 2 extra jump instructions and additional stack operations needed for function calls.
        3. Optimizer improvements in packed structs: Before 0.8.3, storing packed structs, in some cases used an additional storage read operation. After EIP-2929, if the slot was already cold, this means unnecessary stack operations and extra deploy time costs. However, if the slot was already warm, this means additional cost of 100 gas alongside the same unnecessary stack operations and extra deploy time costs.
        4. Custom errors from 0.8.4, leads to cheaper deploy time cost and run time cost. Note: the run time cost is only relevant when the revert condition is met. In short, replace revert strings by custom errors.
    
### Code instances:

        WETH9.sol
        UniswapV2Library.sol



## Do not cache msg.sender


We recommend not to cache msg.sender since calling it is 2 gas while reading a variable is more.


### Code instances:

        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/examples/ReEntrancyGuard.sol#L26
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/mock/MockTribe.sol#L13
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/dao/timelock/Timelock.sol#L76
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/tribe/Tribe.sol#L231


## Use != 0 instead of > 0


Using != 0 is slightly cheaper than > 0. (see https://github.com/code-423n4/2021-12-maple-findings/issues/75 for similar issue)


### Code instances:

        BalancerPCVDepositWeightedPool.sol, 165: change 'balance > 0' to 'balance != 0'
        ERC20Dripper.sol, 34: change 'amountToDrip > 0' to 'amountToDrip != 0'
        WETH9.sol, 76: change 'balance > 0' to 'balance != 0'
        TribalChief.sol, 249: change 'allocPoint > 0' to 'allocPoint != 0'

