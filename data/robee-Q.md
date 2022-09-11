## approve return value is ignored

Some tokens don't correctly implement the EIP20 standard and their approve function returns void instead of a success boolean.
Calling these functions with the correct EIP20 function signatures will always revert.
Tokens that don't correctly implement the latest EIP20 spec, like USDT, will be unusable in the mentioned contracts as they revert the transaction because of the missing return value.
We recommend using OpenZeppelin’s SafeERC20 versions with the safeApprove function that handle the return value check as well as non-standard-compliant tokens.
The list of occurrences in format (solidity file, line number, actual line)
### Code instances:

    ERC20TokemakPCVDeposit.sol, 29,         token.approve(pool, amount);
    VoteEscrowTokenManager.sol, 84,             liquidToken.approve(address(veToken), tokenBalance);
    BalancerLBPSwapper.sol, 176,         IERC20(tokenReceived).approve(address(_vault), type(uint256).max);
    VotiumBriber.sol, 55,         token.approve(address(votiumBribe), tokenAmount);
    AavePCVDeposit.sol, 86,         token.approve(address(lendingPool), pendingBalance);
    PSMRouter.sol, 25,         _fei.approve(address(_psm), type(uint256).max);
    ERC20CompoundPCVDeposit.sol, 28,         token.approve(address(cToken), amount);
    ConvexPCVDeposit.sol, 73,         curvePool.approve(address(convexBooster), lpTokenBalance);
    BalancerLBPSwapper.sol, 175,         IERC20(tokenSpent).approve(address(_vault), type(uint256).max);
    UniswapPCVDeposit.sol, 218,         IERC20(_token).approve(address(router), maxTokens);
    LiquidityGaugeManager.sol, 116,         IERC20(token).approve(gaugeAddress, amount);
    LiquidityGaugeManager.sol, 128,         IERC20(token).approve(gaugeAddress, amount);
    VoteEscrowTokenManager.sol, 79,             liquidToken.approve(address(veToken), tokenBalance);
    IVault.sol, 607,      * must have allowed the Vault to use their tokens via `IERC20.approve()`. This matches the behavior of
    BalancerPCVDepositWeightedPool.sol, 202,                 IERC20(address(poolAssets[i])).approve(address(vault), balances[i]);



## Assert instead require to validate user inputs


        From solidity docs: Properly functioning code should never reach a failing assert statement; if this happens there is a bug in your contract which you should fix.
        With assert the user pays the gas and with require it doesn't. The ETH network gas isn't cheap and users can see it as a scam.

### Code instances:

        FuseFixer.sol : reachable assert in line 75
        FuseFixer.sol : reachable assert in line 77
        TribalChief.sol : reachable assert in line 570
        FuseFixer.sol : reachable assert in line 79
        FuseFixer.sol : reachable assert in line 81
        ExchangerTimelock.sol : reachable assert in line 43
        ABDKMath64x64.sol : reachable assert in line 598
        FuseFixer.sol : reachable assert in line 73
        TribalChief.sol : reachable assert in line 565
        FuseFixer.sol : reachable assert in line 74
        FuseFixer.sol : reachable assert in line 72
        CollateralizationOracleGuardian.sol : reachable assert in line 77
        FuseFixer.sol : reachable assert in line 76
        FuseFixer.sol : reachable assert in line 80
        FuseFixer.sol : reachable assert in line 78
        ExchangerTimelock.sol : reachable assert in line 39
        TribeMinter.sol : reachable assert in line 185



## transfer return value of a general ERC20 is ignored

Need to use safeTransfer instead of transfer. As there are popular tokens, such as USDT that transfer/trasnferFrom method doesn’t return anything. The transfer return value has to be checked (as there are some other tokens that returns false instead revert), that means you must
 1. Check the transfer return value
Another popular possibility is to add a whiteList.
Although the instances are only mocks we think it is better to fix.
Those are the appearances (solidity file, line number, actual line):

### Code instances:

        MockCurveMetapool.sol, 23 (add_liquidity), IERC20(coins[0]).transferFrom(msg.sender, address(this), amounts[0]);
        MockCurveMetapool.sol, 52 (remove_liquidity_one_coin), IERC20(coins[uint256(uint128(i))]).transfer(msg.sender, _amountOut);
        MockPCVDepositV2.sol, 42 (withdraw), IERC20(balanceReportedIn).transfer(to, amount);
        MockUniswapV2PairLiquidity.sol, 99 (burnToken), IERC20(token0).transfer(to, amount0);
        MockLendingPool.sol, 21 (deposit), IERC20(asset).transferFrom(msg.sender, address(this), amount);


## Unbounded loop on array can lead to DoS

The attacker can push unlimitedly to an array, that some function loop over this array.
If increasing the array size enough, calling the function that does a loop over the array will always revert since there is a gas limit.
This is an High Risk issue since those arrays are publicly allows to push items into them.

### Code instance:

        TribalChief.sol (L254): Unbounded loop on the array poolInfo that can be publicly pushed by ['add']



## Duplicates in array


        You allow in some arrays to have duplicates. Sometimes you assumes there are no duplicates in the array.

### Code instances:

    PodFactory._createOptimisticPod pushed (safeAddress)
    TribalChief.deposit pushed (poolDeposit)
    TribalChief.add pushed (_rewarder)
    TribalChief.add pushed (_stakedToken)



## Loss Of Precision

This issue is about arithmetic computation that could have been done more percise.
The following are places in the codebase in which you multiplied after the divisions.
Doing the multiplications at start lead to more accurate calculations.
This is a list of places in the code that this appears (Solidity file, line number, actual line):

### Code instances:

        UniswapPCVDeposit.sol, 153, uint256 resistantOtherInPool = Decimal.one().div(priceOfToken).mul(k).asUint256().sqrt();
        UniswapLens.sol, 62, uint256 resistantOtherInPool = Decimal.one().div(priceOfToken).mul(k).asUint256().sqrt();
        AngleUniswapPCVDeposit.sol, 61, uint256 minAgTokenOut = Decimal .from(amountFei) .div(readOracle()) .mul(Constants.BASIS_POINTS_GRANULARITY - maxBasisPointsFromPegLP) .div(Constants.BASIS_POINTS_GRANULARITY) .asUint256();


## Missing fee parameter validation


Some fee parameters of functions are not checked for invalid values. Validate the parameters:


### Code instances:

        FixedPricePSM.constructor (_redeemFeeBasisPoints)
        PegStabilityModule._setRedeemFee (newRedeemFeeBasisPoints)
        PegStabilityModule.constructor (_redeemFeeBasisPoints)
        PegStabilityModule.setMintFee (newMintFeeBasisPoints)
        PegStabilityModule.constructor (_mintFeeBasisPoints)
        NonCustodialPSM._setRedeemFee (newRedeemFeeBasisPoints)
        PegStabilityModule.setRedeemFee (newRedeemFeeBasisPoints)
        FixedPricePSM.constructor (_mintFeeBasisPoints)
        PriceBoundPSM.constructor (_redeemFeeBasisPoints)
        BaseBalancerPoolManager.setSwapFee (swapFee)



## safeApprove of openZeppelin is deprecated


You use safeApprove of openZeppelin although it's deprecated.
(see https://github.com/OpenZeppelin/openzeppelin-contracts/blob/566a774222707e424896c0c390a84dc3c13bdcb2/contracts/token/ERC20/utils/SafeERC20.sol#L38)
You should change it to increase/decrease Allowance as OpenZeppilin says.

### Code instances:

        Deprecated safeApprove in RariMerkleRedeemer_flattened.sol line 1743: _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
        Deprecated safeApprove in FuseFixer.sol line 150: SafeERC20.safeApprove(IERC20(underlying), ctoken, type(uint256).max);
        Deprecated safeApprove in BAMMDeposit.sol line 31: lusd.safeApprove(address(BAMM), amount);
        Deprecated safeApprove in PSMRouter.sol line 24: IERC20(address(Constants.WETH)).approve(address(_psm), type(uint256).max);



## Require with empty message

The following requires are with empty messages.
This is very important to add a message for any require. So the user has enough information to know the reason of failure.
### Code instances:

        Solidity file: ABDKMath64x64.sol, In line 277 with Empty Require message.
        Solidity file: WETH9.sol, In line 51 with Empty Require message.
        Solidity file: MaxFeiWithdrawalGuard.sol, In line 38 with Empty Require message.
        Solidity file: ABDKMath64x64.sol, In line 442 with Empty Require message.


## Require with not comprehensive message

The following requires has a non comprehensive messages.
This is very important to add a comprehensive message for any require. Such that the user has enough
information to know the reason of failure:

### Code instances:

        Solidity file: PCVGuardian.sol, In line 81 with Require message: empty
        Solidity file: PCVGuardian.sol, In line 56 with Require message: empty
        Solidity file: PCVGuardian.sol, In line 181 with Require message: set
        Solidity file: AutoRewardsDistributorV2.sol, In line 108 with Require message: init
        Solidity file: AutoRewardsDistributorV2.sol, In line 67 with Require message: pid
        Solidity file: TribalChiefSyncV2.sol, In line 69 with Require message: rewards
        Solidity file: TribalChiefSyncV2.sol, In line 70 with Require message: timestamp
        Solidity file: AutoRewardsDistributorV2.sol, In line 70 with Require message: ctoken
        Solidity file: MergerGate.sol, In line 23 with Require message: rip
        Solidity file: PCVGuardian.sol, In line 186 with Require message: unset



## Not verified input


external / public functions parameters should be validated to make sure the address is not 0.
Otherwise if not given the right input it can mistakenly lead to loss of user funds.

### Code instances:

        PegStabilityModule.sol.withdraw to
        RatioPCVControllerV2.sol.transferFromRatio to
        GlobalRateLimitedMinter.sol.mint to



## Treasury may be address(0)


    Make sure the treasury is not address(0).


### Code instance:

        TribeMinter.sol.setTribeTreasury newTribeTreasury



## Solidity compiler versions mismatch


The project is compiled with different versions of solidity, which is not recommended because it can lead to undefined behaviors.

### Code instance:





## Hardcoded WETH

WETH address is hardcoded but it may differ on other chains, e.g. Polygon,
so make sure to check this before deploying and update if necessary
You should consider injecting WETH address via the constructor.
(previous issue: https://github.com/code-423n4/2021-10-ambire-findings/issues/54)
### Code instances:

        Hardcoded weth address in Constants.sol
        Hardcoded weth address in VeBalDelegatorPCVDeposit.sol



## Use safe math for solidity version <8

You should use safe math for solidity version <8 since there is no default over/under flow check it suchversions of solidity.
### Code instance:

        The contract WETH9.sol doesn't use safe math and is of solidity version < 8



## Using safeERC20 for non ERC20

You can't use SafeERC20 for non ERC20 protocols (as for example ERC721) due to undefined behavior of it. The following files does that:
### Code instance:

SimpleFeiDaiPSM.sol, 15 :     using SafeERC20 for Fei;




## Not verified owner


        owner param should be validated to make sure the owner address is not address(0).
        Otherwise if not given the right input all only owner accessible functions will be unaccessible.


### Code instances:

        Tribe.sol.permit owner
        Fei.sol.permit owner



## Init frontrun

Most contracts use an init pattern (instead of a constructor) to initialize contract parameters. Unless these are enforced to be atomic with contact deployment via deployment script or factory contracts, they are susceptible to front-running race conditions where an attacker/griefer can front-run (cannot access control because admin roles are not initialized) to initially with their own (malicious) parameters upon detecting (if an event is emitted) which the contract deployer has to redeploy wasting gas and risking other transactions from interacting with the attacker-initialized contract.

Many init functions do not have an explicit event emission which makes monitoring such scenarios harder. All of them have re-init checks; while many are explicit some (those in auction contracts) have implicit reinit checks in initAccessControls() which is better if converted to an explicit check in the main init function itself.
(details credit to: https://github.com/code-423n4/2021-09-sushimiso-findings/issues/64)
The vulnerable initialization functions in the codebase are:

### Code instances:

        TribalChief.sol, initialize, 140
        StakingTokenWrapper.sol, init, 30
        Core.sol, init, 20



## Named return issue

Users can mistakenly think that the return value is the named return, but it is actually the actualreturn statement that comes after. To know that the user needs to read the code and is confusing.
Furthermore, removing either the actual return or the named return will save gas.

### Code instances:

        UintArrayOps.sol, positiveDifference
        ConvexPCVDeposit.sol, resistantBalanceAndFei
        MaxFeiWithdrawalGuard.sol, getProtecActions
        CurvePCVDepositPlainPool.sol, resistantBalanceAndFei
        BalancerLBPSwapper.sol, getTokensIn



## Two Steps Verification before Transferring Ownership

The following contracts have a function that allows them an admin to change it to a different address. If the admin accidentally uses an invalid address for which they do not have the private key, then the system gets locked.
It is important to have two steps admin change where the first is announcing a pending new admin and the new address should then claim its ownership.
A similar issue was reported in a previous contest and was assigned a severity of medium: [code-423n4/2021-06-realitycards-findings#105](https://github.com/code-423n4/2021-06-realitycards-findings/issues/105)

### Code instances:

        IBaseBalancerPoolManager.sol
        PodAdminGateway.sol
        CoreRef.sol
        CollateralizationOracleWrapper.sol
        AutoRewardsDistributorV2.sol
        ICollateralizationOracleWrapper.sol
        AngleUniswapPCVDeposit.sol
        IBasePool.sol
        IAaveGovernanceV2.sol
        Permissions.sol
        ICoreRef.sol
        BaseBalancerPoolManager.sol
        AutoRewardsDistributor.sol
        ICurvePool.sol



## Missing non reentrancy modifier

The following functions are missing reentrancy modifier although some other pulbic/external functions does use reentrancy modifer.
Even though I did not find a way to exploit it, it seems like those functions should have the nonReentrant modifier as the other functions have it as well..

### Code instances:

        NonCustodialPSM.sol, setGlobalRateLimitedMinter is missing a reentrancy modifier
        TribalChief.sol, governorWithdrawTribe is missing a reentrancy modifier
        TribalChief.sol, resetRewards is missing a reentrancy modifier
        NonCustodialPSM.sol, setPCVDeposit is missing a reentrancy modifier
        NonCustodialPSM.sol, withdrawERC20 is missing a reentrancy modifier




## In the following public update functions no value is returned

In the following functions no value is returned, due to which by default value of return will be 0.
We assumed that after the update you return the latest new value.
(similar issue here: https://github.com/code-423n4/2021-10-badgerdao-findings/issues/85).

### Code instances:

        TribalChief.sol, updatePool
        CollateralizationOracleWrapper.sol, update
        WeightedBalancerPoolManager.sol, updateWeightsGradually
        MultiRateLimited.sol, updateMaxBufferCap
        MultiRateLimited.sol, updateMaxRateLimitPerSecond
        OracleRef.sol, updateOracle
        ChainlinkOracleWrapper.sol, update
        PodFactory.sol, updateDefaultPodController
        CollateralizationOracle.sol, update
        CompositeOracle.sol, update
        MultiRateLimited.sol, updateAddress
        TribalChief.sol, updateBlockReward
        CollateralizationOracleWrapper.sol, updateIfOutdated
        ConstantOracle.sol, update
        TribalChief.sol, massUpdatePools
        GOhmEthOracle.sol, update




## Open TODOs

Open TODOs can hint at programming or architectural errors that still need to be fixed.
These files has open TODOs:

### Code instance:

    Open TODO in MockRariMerkleRedeemerNoSigs.sol line 24 :         // @todo - do we want to use this, which supports ERC1271, or *just* EOA signatures?




## Missing commenting


        The following functions are missing commenting as describe below:

### Code instances:

        TribeRagequit.sol, getCirculatingTribe (public), @return is missing
        RatioPCVControllerV2.sol, _withdrawRatio (internal), parameters pcvDeposit, to, basisPoints not commented
        FuseFixer.sol, _repayERC20 (internal), parameter underlying not commented
        RatioPCVControllerV2.sol, _withdrawRatio (internal), @return is missing
        FuseFixer.sol, withdraw (external), parameters to, amount not commented
        TribeTimelockedDelegatorBurner.sol, undelegate (external), parameter _delegate not commented
        RatioPCVControllerV2.sol, _transferETHAsWETH (internal), parameters to, amount not commented
        ConstantOracle.sol, isOutdated (external), @return is missing
        RatioPCVControllerV2.sol, _transferWETHAsETH (internal), parameters to, amount not commented
        Permissions.sol, _setupGovernor (internal), parameter governor not commented
        NonCustodialPSM.sol, _validatePriceRange (internal), parameter price not commented
        BalancerPCVDepositBase.sol, claimRewards (external), parameters distributionId, amount, merkleProof not commented
        FuseFixer.sol, balanceReportedIn (public), @return is missing
        FuseFixer.sol, balance (public), @return is missing
        RestrictedPermissions.sol, isGovernor (external), @return is missing



## Add a timelock

To give more trust to users: functions that set key/critical variables should be put behind a timelock.

### Code instances:

        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/core/Core.sol#L32
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/PriceBoundPSM.sol#L59
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/refs/OracleRef.sol#L68
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/tribe/TribeMinter.sol#L86


## Must approve 0 first


Some tokens (like USDT) do not work when changing the allowance from an existing non-zero allowance value.
They must first be approved by zero and then the actual allowance must be approved.

### Code instances:


    approve without approving 0 first BalancerLBPSwapper.sol, 176,         IERC20(tokenReceived).approve(address(_vault), type(uint256).max);

    approve without approving 0 first BalancerPCVDepositWeightedPool.sol, 202,                 IERC20(address(poolAssets[i])).approve(address(vault), balances[i]);

    approve without approving 0 first LiquidityGaugeManager.sol, 128,         IERC20(token).approve(gaugeAddress, amount);

    approve without approving 0 first PSMRouter.sol, 25,         _fei.approve(address(_psm), type(uint256).max);

    approve without approving 0 first ERC20CompoundPCVDeposit.sol, 28,         token.approve(address(cToken), amount);

    approve without approving 0 first PSMRouter.sol, 24,         IERC20(address(Constants.WETH)).approve(address(_psm), type(uint256).max);

    approve without approving 0 first LiquidityGaugeManager.sol, 116,         IERC20(token).approve(gaugeAddress, amount);

    approve without approving 0 first VotiumBriber.sol, 55,         token.approve(address(votiumBribe), tokenAmount);

    approve without approving 0 first CurvePCVDepositPlainPool.sol, 87,                 tokens[i].approve(address(curvePool), balances[i]);

    approve without approving 0 first UniswapPCVDeposit.sol, 218,         IERC20(_token).approve(address(router), maxTokens);

    approve without approving 0 first ConvexPCVDeposit.sol, 73,         curvePool.approve(address(convexBooster), lpTokenBalance);

    approve without approving 0 first ExchangerTimelock.sol, 36,         rgt.approve(address(exchanger), rgtBalance);

    approve without approving 0 first BalancerLBPSwapper.sol, 175,         IERC20(tokenSpent).approve(address(_vault), type(uint256).max);

    approve without approving 0 first ERC20TokemakPCVDeposit.sol, 29,         token.approve(pool, amount);

    approve without approving 0 first BalancerLBPSwapper.sol, 174,         _pool.approve(address(_vault), type(uint256).max);


    approve without approving 0 first BAMMDeposit.sol, 31,         lusd.safeApprove(address(BAMM), amount);

    approve without approving 0 first UniswapLiquidityRemover.sol, 48,         FEI_TRIBE_PAIR.approve(address(UNISWAP_ROUTER), amountLP);

    approve without approving 0 first VlAuraDelegatorPCVDeposit.sol, 116,         IERC20(aura).safeApprove(auraLocker, amount);

    approve without approving 0 first AavePCVDeposit.sol, 86,         token.approve(address(lendingPool), pendingBalance);

    approve without approving 0 first VoteEscrowTokenManager.sol, 84,             liquidToken.approve(address(veToken), tokenBalance);

    approve without approving 0 first VoteEscrowTokenManager.sol, 79,             liquidToken.approve(address(veToken), tokenBalance);

    approve without approving 0 first AngleEuroRedeemer.sol, 86,         USDC.approve(MAKER_DAI_USDC_PSM_AUTH, usdcBalance);




## Unsafe Cast

use openzeppilin's safeCast in (although its a mock I think it might be good to address):

### Code instances:

        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/mock/MockRouter.sol#L62 : unsafe cast uint112(amountToken0Desired)
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/mock/MockRouter.sol#L62 : unsafe cast uint112(amountToken1Desired)



## Two arrays length mismatch


The functions below fail to perform input validation on arrays to verify the lengths match.
A mismatch could lead to an exception or undefined behavior.
Consider making this a medium risk please.


        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/MaxFeiWithdrawalGuard.sol#L31 constructor ['deposits', 'destinations', 'liquiditySources']



## Unbounded loop on array that can only grow can lead to DoS

A malicious attacker that is also a protocol owner can push unlimitedly to an array, that some function loop over this array.
If increasing the array size enough, calling the function that does a loop over the array will always revert since there is a gas limit.
This is a Med Risk issue since it can lead to DoS with a reasonable chance of having untrusted owner or even an owner that did a mistake in good faith.

### Code instance:

        TribalChief.sol (L254): Unbounded loop on the array poolInfo that can be publicly pushed by ['add'] and can't be pulled



## Potential DoS

the balance of outputToken is checked to be exactly a specified value that is not declared in this specific function.
 Therefore, a malicious user can transfer to the contract address tiny amount of tokens and the user transactions will always revert.

### Code instance:

        Potential DoS in ERC20CompoundPCVDeposit.sol, 32



## Div by 0


Division by 0 can lead to accidentally revert,
(An example of a similar issue - https://github.com/code-423n4/2021-10-defiprotocol-findings/issues/84)

### Code instances:

        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/TribalChief.sol#L543 to might be 0
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/timelocks/LinearTokenTimelock.sol#L25 duration might be 0



## Tokens with fee on transfer are not supported


There are ERC20 tokens that charge fee for every transfer() / transferFrom().

Vault.sol#addValue() assumes that the received amount is the same as the transfer amount,
and uses it to calculate attributions, balance amounts, etc.
But, the actual transferred amount can be lower for those tokens.
Therefore it's recommended to use the balance change before and after the transfer instead of the amount.
This way you also support the tokens with transfer fee - that are popular.


### Code instances:

        https://github.com/code-423n4/2022-09-tribe/tree/main/scripts/shutdown/data/prod/RariMerkleRedeemer_flattened.sol#L2342
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L246
        https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/tribe/TribeMinter.sol#L179




