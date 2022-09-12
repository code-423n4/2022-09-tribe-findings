## 1. Avoid using Floating Pragma
### Description
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
The pragma declared across the solution is ^0.8.4
As the compiler introduces a several interesting upgrades in newer  versions of Solidity
consider locking at this version or a more recent one.
### Instances
//Links to github Files:
[SimpleFeiDaiPSM.sol:L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2)
[TribeRedeemer.sol:L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2)
[RariMerkleRedeemer.sol:L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2)
[MerkleRedeemerDripper.sol:L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2)

### Actual Codes Used
```
contracts/peg/SimpleFeiDaiPSM.sol:2:pragma solidity ^0.8.4;
contracts/shutdown/redeem/TribeRedeemer.sol:2:pragma solidity ^0.8.4;
contracts/shutdown/fuse/RariMerkleRedeemer.sol:2:pragma solidity =0.8.10;
contracts/shutdown/fuse/MerkleRedeemerDripper.sol:2:pragma solidity =0.8.10;
```
----
## 2. The nonReentrant modifier should occur before all other modifiers
### Description
The nonReentrant modifier should occur before all other modifiers
This is a best-practice to protect against re-entrancy in other modifiers
### Instances 
// Links To Github Files:
[RariMerkleRedeemer.sol:L48](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L48)
[RariMerkleRedeemer.sol:L56](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L56)
[RariMerkleRedeemer.sol:L64](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L64)
[RariMerkleRedeemer.sol:L68](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L68)
[RariMerkleRedeemer.sol:L76](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L76)
[RariMerkleRedeemer.sol:L103](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L103)
[RariMerkleRedeemer.sol:L114](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L114)

### Actual Codes Used

```
contracts/shutdown/fuse/RariMerkleRedeemer.sol:48:    function sign(bytes calldata signature) external override hasNotSigned nonReentrant {
contracts/shutdown/fuse/RariMerkleRedeemer.sol:56:    ) external override hasSigned nonReentrant {
contracts/shutdown/fuse/RariMerkleRedeemer.sol:64:    ) external override hasSigned nonReentrant {
contracts/shutdown/fuse/RariMerkleRedeemer.sol:68:    function redeem(address cToken, uint256 cTokenAmount) external override hasSigned nonReentrant {
contracts/shutdown/fuse/RariMerkleRedeemer.sol:76:        nonReentrant
contracts/shutdown/fuse/RariMerkleRedeemer.sol:103:    ) external hasSigned nonReentrant {
contracts/shutdown/fuse/RariMerkleRedeemer.sol:114:    ) external override hasNotSigned nonReentrant {
```
### Recommended Mitigation Steps
using the nonReentrant modifier before hasSigned and hasNotSigned modifier

----
# 3.`EVENT` is missing indexed feilds
### Description
Each event should use three indexed fields if there are three or more fields
### Instances
//Links to Githubfile
[TribeRedeemer.sol:L14](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L14)
[SimpleFeiDaiPSM.sol:L27](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L27)
[SimpleFeiDaiPSM.sol:L29](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L29)

### Actual codes used:
```
contracts/shutdown/redeem/TribeRedeemer.sol:14:    event Redeemed(address indexed owner, address indexed receiver, uint256 amount, uint256 base);
contracts/peg/SimpleFeiDaiPSM.sol:27:    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
contracts/peg/SimpleFeiDaiPSM.sol:29:    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```
----
# 4. Improper Input Validation/ Lack of Input validation of an `ARRAY`
### Description
If the array length of amountsToRedeem and amountsToClaim is not equal it can lead to an error.

### Instances
//Links to Github file
[RariMerkleRedeemer.sol:L108-L118](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L108-L118)

### Actual codes used
```
    function signAndClaimAndRedeem(
        bytes calldata signature,
        address[] calldata cTokens,
        uint256[] calldata amountsToClaim,
        uint256[] calldata amountsToRedeem,
        bytes32[][] calldata merkleProofs
    ) external override hasNotSigned nonReentrant {
        _sign(signature);
        _multiClaim(cTokens, amountsToClaim, merkleProofs);
        _multiRedeem(cTokens, amountsToRedeem);
    }
```
----
# 5. Missing zero-address check in constructors and the setter functions
### Description
Missing checks for zero-addresses may lead to infunctional protocol, if the variable addresses are updated incorrectly.
### Instances
//Links to Githubfile
[TribeRedeemer.sol:L27](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L27)
[RariMerkleRedeemer.sol:L35](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L35)
[MerkleRedeemerDripper.sol:L10](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L10)

### Actual codes used:
```
contracts/shutdown/redeem/TribeRedeemer.sol:27:    constructor(
contracts/shutdown/fuse/RariMerkleRedeemer.sol:35:    constructor(
contracts/shutdown/fuse/MerkleRedeemerDripper.sol:10:    constructor(
```
### Recommended Mitigation Steps
Consider adding zero-address checks in the discussed constructors:
require(newAddr != address(0));

----
# 6. Incosistent return type within contracts
### Description
The functions uses an unamed return, while the rest of the contract and functions uses named returns. Please keep it consistent within contracts and accross the project if possible to improve readibility.
### Instances
//Links to Githubfile
[SimpleFeiDaiPSM.sol:L61](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L61)
[SimpleFeiDaiPSM.sol:L66](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L66)
[SimpleFeiDaiPSM.sol:L78](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L78)
[SimpleFeiDaiPSM.sol:L83](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L83)
[TribeRedeemer.sol:L38](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L38)


### Actual codes used:
```
contracts/peg/SimpleFeiDaiPSM.sol:61:    function getMintAmountOut(uint256 amountIn) external pure returns (uint256) {
contracts/peg/SimpleFeiDaiPSM.sol:66:    function getRedeemAmountOut(uint256 amountIn) external pure returns (uint256) {
contracts/peg/SimpleFeiDaiPSM.sol:78:    function balance() external view returns (uint256) {
contracts/peg/SimpleFeiDaiPSM.sol:83:    function resistantBalanceAndFei() external view returns (uint256, uint256) {
contracts/shutdown/redeem/TribeRedeemer.sol:38:    function tokensReceivedOnRedeem() public view returns (address[] memory) {
```
### Recommended Mitigation Steps
use named return for every function

----
# 7. Same operation done by two different function
 Non-Critical
 If two fuction are doing same operation there is no point of declaring two functions
 ### Instances
 //Links to Githubfile
 [SimpleFeiDaiPSM.sol:L61-L63](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L61-L63)
[SimpleFeiDaiPSM.sol:L66-L68](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L66-L68)
 
 ### Actual codes used:
```
contracts/peg/SimpleFeiDaiPSM.sol:61:    function getMintAmountOut(uint256 amountIn) external pure returns (uint256) {
contracts/peg/SimpleFeiDaiPSM.sol:66:    function getRedeemAmountOut(uint256 amountIn) external pure returns (uint256) {
```
----
### Recommended Mitigation Steps
use a single function for a particular job