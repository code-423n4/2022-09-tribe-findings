## L-01 FLOATING PRAGMA VERSION SHOULD NOT BE USED
In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

[SimpleFeiDaiPSM.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2)
```
// SPDX-License-Identifier: GPL-3.0-or-later

pragma solidity ^0.8.4;
```

[TribeRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2)

```
// SPDX-License-Identifier: GPL-3.0-or-later

pragma solidity ^0.8.4;
```

### Proof of Concept

[SWC-103](https://swcregistry.io/docs/SWC-103)


## N-01 NATSPEC IS INCOMPLETE

Missing `@params`

[TribeRedeemer.sol#L22-L35](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L22-L35)

```
/// @notice base used to compute the redemption amounts.

/// For instance, if the base is 100, and a user provides 100 `redeemedToken`,

/// they will receive all the balances of each `tokensReceived` held on this contract.

uint256 public redeemBase;

constructor(

address _redeemedToken,

address[] memory _tokensReceived,

uint256 _redeemBase

) {

redeemedToken = _redeemedToken;

tokensReceived = _tokensReceived;

redeemBase = _redeemBase;

}
```

[TribeRedeemer.sol#L42-L47](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L42-L47)

```
/// @notice Return the balances of `tokensReceived` that would be

/// transferred if redeeming `amountIn` of `redeemedToken`.

function previewRedeem(uint256 amountIn)

public

view

returns (address[] memory tokens, uint256[] memory amountsOut)
```

[TribeRedeemer.sol#L63-L65](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L63-L65)

```
/// @notice Redeem `redeemedToken` for a pro-rata basket of `tokensReceived`

function redeem(address to, uint256 amountIn) external nonReentrant {

IERC20(redeemedToken).safeTransferFrom(msg.sender, address(this), amountIn);
```

[MerkleRedeemerDripper.sol#L7-L16](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L7-L16)

```
/// @notice Drips ERC20 tokens to the RariMerkleRedeemer contract

/// @author kryptoklob

contract MerkleRedeemerDripper is ERC20Dripper {

constructor(

address _core,

address _target,

uint256 _dripPeriod,

uint256 _amountToDrip,

address _token

) ERC20Dripper(_core, _target, _dripPeriod, _amountToDrip, _token) {}
```

All the Functions in [RariMerkleRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol) have missing `@param`

All the Functions in [RariMerkleRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol) have missing `@notice`



## N-02 NAMING CONVENTION FOR CONSTANTS SHOULD BE FOLLOWED

Constants should be named with all capital letters with underscores separating words. 

[SimpleFeiDaiPSM.sol#L75](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L75)

```
- address public constant balanceReportedIn = address(DAI);
+ address public constant BALANCE_REPORTED_IN = address(DAI);

```

[SimpleFeiDaiPSM.sol#L92-L98](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98)

```
- uint256 public constant mintFeeBasisPoints = 0;
- uint256 public constant redeemFeeBasisPoints = 0;
- address public constant underlyingToken = address(DAI);
- uint256 public constant getMaxMintAmountOut = type(uint256).max;
- bool public constant paused = false;
- bool public constant redeemPaused = false;
- bool public constant mintPaused = false;

+ uint256 public constant MINT_FEE_BASIS_POINTS = 0;
+ uint256 public constant REDEEM_FEE_BASIS_POINTS = 0;
+ address public constant UNDERLYING_TOKEN = address(DAI);
+ uint256 public constant GET_MAX_MINT_AMOUNT_OUT = type(uint256).max;
+ bool public constant PAUSED = false;
+ bool public constant REDEEM_PAUSED = false;
+ bool public constant MINT_PAUSED = false;

```


## N-03 USE LATEST SOLIDITY VERSION

When deploying contracts, you should use the latest released version of Solidity. Apart from exceptional cases, only the latest version receives [security fixes](https://github.com/ethereum/solidity/security/policy#supported-versions). Furthermore, breaking changes as well as new features are introduced regularly.
Latest Version is 0.8.17

[SimpleFeiDaiPSM.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2)

```
pragma solidity ^0.8.4;
```

[TribeRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2)

```
pragma solidity ^0.8.4;
```

[MerkleRedeemerDripper.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2)

```
pragma solidity =0.8.10;
```

[RariMerkleRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2)
`
```
pragma solidity =0.8.10;
```


## N-04 INCORRECT ORDERING OF FUNCTIONS

Functions should be grouped according to their visibility and ordered.


In [TribeRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol) , the correct order of functions according visibility should be


```
function redeem(address to, uint256 amountIn) external nonReentrant

function tokensReceivedOnRedeem() public view returns (address[] memory)

function previewRedeem(uint256 amountIn) public view returns (address[] memory tokens, uint256[] memory amountsOut)

```

## N-05 UNNECESSARY ASSIGNMENT TO 0

[SimpleFeiDaiPSM.sol#L92-L93](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92-L93)

```
- uint256 public constant mintFeeBasisPoints = 0;
+ uint256 public constant mintFeeBasisPoints;

- uint256 public constant redeemFeeBasisPoints = 0;
+ uint256 public constant redeemFeeBasisPoints;
```

[TribeRedeemer.sol#L53](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L53) 

```
for (uint256 i = 0; i < tokensReceived.length; i++)
```

[TribeRedeemer.sol#L71](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L71)

```
for (uint256 i = 0; i < tokens.length; i++)
```

[RariMerkleRedeemer.sol#L128](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L128)

```
for (uint256 i = 0; i < _cTokens.length; i++)
```

[RariMerkleRedeemer.sol#L141](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L141)

```
for (uint256 i = 0; i < _cTokens.length; i++)
```

[RariMerkleRedeemer.sol#L193]()

```
for (uint256 i = 0; i < _cTokens.length; i++)
```

[RariMerkleRedeemer.sol#L229](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L229)

```
for (uint256 i = 0; i < cTokens.length; i++)
```

[RariMerkleRedeemer.sol#L247](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L247)

```
for (uint256 i = 0; i < cTokens.length; i++)
```

