# QA Report for FEI and TRIBE Redemption contest

## Overview
During the audit, 2 low and 5 non-critical issues were found.

№ | Title | Risk Rating  
--- | --- | --- 
L-1 | [Misleading comments](#l-1-misleading-comments) | Low
L-2 | [Functions return input](#l-2-functions-return-input) | Low
NC-1 | [Constants at the end of the contract](#nc-1-constants-at-the-end-of-the-contract) | Non-Critical
NC-2 | [Constants may be used](#nc-2-constants-may-be-used) | Non-Critical
NC-3 | [Missing NatSpec](#nc-3-missing-natspec) | Non-Critical
NC-4 | [Floating pragma](#nc-4-floating-pragma) | Non-Critical
NC-5 | [Incorrect comment](#nc-5-incorrect-comment) | Non-Critical

## Low Risk Findings (2)
### L-1. Misleading comments
##### Instances
- [SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L87-L90)

##### Recommendation
Put comments before functions, not after.

#
### L-2. Functions return input
##### Instances
[Link:](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L60-L68)
```
    /// @notice calculate the amount of FEI out for a given `amountIn` of underlying
    function getMintAmountOut(uint256 amountIn) external pure returns (uint256) {
        return amountIn;
    }


    /// @notice calculate the amount of underlying out for a given `amountFeiIn` of FEI
    function getRedeemAmountOut(uint256 amountIn) external pure returns (uint256) {
        return amountIn;
    }
```

## Non-Critical Risk Findings (5)
### NC-1. Constants at the end of the contract
##### Instances
- [SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98)

##### Recommendation
[Inside each contract, library or interface, use the following order](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout):
- Type declarations
- State variables
- Events
- Modifiers
- Functions

#
### NC-2. Constants may be used
##### Description
Constants may be used instead of  literal values.

##### Instances
- [```return (cTokenExchangeRates[cToken] * amount) / 1e18;```](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L85) (for 1e18)
- [```require(_cTokens.length == 27, "Must provide exactly 27 exchange rates.");```](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125) (for 27)
- [```_exchangeRates[i] > 1e10,```](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L130) (for 1e10)
- [```require(_cTokens.length == 27, "Must provide exactly 27 merkle roots");```](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138) (for 27)

##### Recommendation
Define constant variables, especially, for repeated values.

#
### NC-3. Missing NatSpec
##### Description
NatSpec is missing in 1 contract.

##### Instances
- [RariMerkleRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol)

##### Recommendation
Add NatSpec for all functions.

#
### NC-4. Floating pragma
##### Description
Contracts should be deployed with the same compiler version. It helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

##### Instances
- [TribeRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol)
- [SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol)

##### Recommendation
According to [SWC-103](https://swcregistry.io/docs/SWC-103), pragma version should be locked.

#
### NC-5. Incorrect comment
##### Description
Not only public but also external.

##### Instances
[```/** ---------- Public State-Changing Funcs ----------------- **/``` ](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L46)