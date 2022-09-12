# 1. Improper Input Validation/ Lack of Input validation of an `ARRAY`

if the array length of and amountsToClaim, amountsToRedeem is not equal it can lead to an error.

## Impact
//Links to github file
[RariMerkleRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L107-L118)


```
//actual codes used

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

### Tools Used

manual review

### Recommended Mitigation Steps

check the input array length

------

# 2.`EVENT` IS MISSING INDEXED FIELDS

Each event should use three indexed fields if there are three or more fields
### Instances
[TribeRedeemer](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L14)
[SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L27)
[SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L27)


```
//actual codes used

contracts/shutdown/redeem/TribeRedeemer.sol:14:    event Redeemed(address indexed owner, address indexed receiver, uint256 amount, uint256 base);
contracts/peg/SimpleFeiDaiPSM.sol:27:    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
contracts/peg/SimpleFeiDaiPSM.sol:29:    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);

```

-----
# 3. Use Of floating Pragma

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Instances
[TribeReedemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2)
[SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2)
[MerkleRedeemerDripper.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2)
[RariMerkleRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2)

```
//actual codes used

ontracts/shutdown/fuse/RariMerkleRedeemer.sol:2:pragma solidity =0.8.10;
contracts/shutdown/fuse/MerkleRedeemerDripper.sol:2:pragma solidity =0.8.10;
contracts/shutdown/redeem/TribeRedeemer.sol:2:pragma solidity ^0.8.4;
contracts/peg/SimpleFeiDaiPSM.sol:2:pragma solidity ^0.8.4;

```
### Mitigation step

Avoid Using Floating Pragma instead lock the solidity version

-----

## 4.Variable names that consist of all capital letters should be reserved for const/immutable variables

If the variable needs to be different based on which class it comes from, a view/pure function should be used instead (e.g. like this).
### Instances
[TribeReedemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L17)
[SimpleFeiDaiPSM.solL75](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L75)
[SimpleFeiDaiPSM.solL92-L98](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98)

```
// actual code used

contracts/shutdown/redeem/TribeRedeemer.sol:17:    address public immutable redeemedToken;
contracts/peg/SimpleFeiDaiPSM.sol:75:    address public constant balanceReportedIn = address(DAI);
contracts/peg/SimpleFeiDaiPSM.sol:92:    uint256 public constant mintFeeBasisPoints = 0;
contracts/peg/SimpleFeiDaiPSM.sol:93:    uint256 public constant redeemFeeBasisPoints = 0;
contracts/peg/SimpleFeiDaiPSM.sol:94:    address public constant underlyingToken = address(DAI);
contracts/peg/SimpleFeiDaiPSM.sol:95:    uint256 public constant getMaxMintAmountOut = type(uint256).max;
contracts/peg/SimpleFeiDaiPSM.sol:96:    bool public constant paused = false;
contracts/peg/SimpleFeiDaiPSM.sol:97:    bool public constant redeemPaused = false;
contracts/peg/SimpleFeiDaiPSM.sol:98:    bool public constant mintPaused = false;
```

-----








