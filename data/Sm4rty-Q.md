
## 1. Avoid using Floating Pragma:

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Instances:
[TribeRedeemer.sol:L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2)
[SimpleFeiDaiPSM.sol:L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2)
```
contracts/shutdown/redeem/TribeRedeemer.sol:2:pragma solidity ^0.8.4;
contracts/peg/SimpleFeiDaiPSM.sol:2:pragma solidity ^0.8.4;
``` 
Recommend using fixed solidity version

### References:

[https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity](https://code4rena.com/reports/2022-04-phuture#g-20-use-a-more-recent-version-of-solidity)

-----

## 2. Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

### Instances:
[TribeRedeemer.sol:L14](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L14)
[SimpleFeiDaiPSM.sol:L27](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L27)
[SimpleFeiDaiPSM.sol:L29](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L29)
```
contracts/shutdown/redeem/TribeRedeemer.sol:14:    event Redeemed(address indexed owner, address indexed receiver, uint256 amount, uint256 base);
contracts/peg/SimpleFeiDaiPSM.sol:27:    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
contracts/peg/SimpleFeiDaiPSM.sol:29:    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);

```

### References:

[https://code4rena.com/reports/2022-05-sturdy#n-10-event-is-missing-indexed-fields](https://code4rena.com/reports/2022-05-sturdy#n-10-event-is-missing-indexed-fields)

-----

## 3. Variable names that consist of all capital letters should be reserved for const/immutable variables

Variable names that consist of all capital letters should be reserved for const/immutable variables

If the variable needs to be different based on which class it comes from, a view/pure function should be used instead (e.g. like this).

### Instance:
[SimpleFeiDaiPSM.sol:L19](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L19)
[SimpleFeiDaiPSM.sol:L20](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L20)
[SimpleFeiDaiPSM.sol:L75](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L75)
[SimpleFeiDaiPSM.sol:L92-98](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98)
[TribeRedeemer.sol:L17](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L17)
```
contracts/peg/SimpleFeiDaiPSM.sol:19:    IERC20 private constant DAI = IERC20(0x6B175474E89094C44Da98b954EedeAC495271d0F);
contracts/peg/SimpleFeiDaiPSM.sol:20:    Fei private constant FEI = Fei(0x956F47F50A910163D8BF957Cf5846D573E7f87CA);
contracts/peg/SimpleFeiDaiPSM.sol:75:    address public constant balanceReportedIn = address(DAI);
contracts/peg/SimpleFeiDaiPSM.sol:92:    uint256 public constant mintFeeBasisPoints = 0;
contracts/peg/SimpleFeiDaiPSM.sol:93:    uint256 public constant redeemFeeBasisPoints = 0;
contracts/peg/SimpleFeiDaiPSM.sol:94:    address public constant underlyingToken = address(DAI);
contracts/peg/SimpleFeiDaiPSM.sol:95:    uint256 public constant getMaxMintAmountOut = type(uint256).max;
contracts/peg/SimpleFeiDaiPSM.sol:96:    bool public constant paused = false;
contracts/peg/SimpleFeiDaiPSM.sol:97:    bool public constant redeemPaused = false;
contracts/peg/SimpleFeiDaiPSM.sol:98:    bool public constant mintPaused = false;

contracts/shutdown/redeem/TribeRedeemer.sol:17:    address public immutable redeemedToken;

```

### References:
https://code4rena.com/reports/2022-05-sturdy#n-08-variable-names-that-consist-of-all-capital-letters-should-be-reserved-for-constimmutable-variables

-----

## 4. Lack of Input validation of an `ARRAY`

### Impact
if the array length of and amountsToClaim, amountsToRedeem is not equal it can lead to an error.
Improper Input Validation/ Lack of Input validation of an `ARRAY`

### Instances:
[RariMerkleRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L107-L118)
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