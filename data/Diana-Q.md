## L-01  NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Proof of Concept

There were 2 instances of this issue.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2

```
File: contracts/peg/SimpleFeiDaiPSM.sol    #1

pragma solidity ^0.8.4;
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2

```
File: contracts/shutdown/redeem/TribeRedeemer.sol    #2

pragma solidity ^0.8.4;
```


### Recommended Mitigation Steps

Lock the pragma version



## L-02  ADDING A RETURN STATEMENT WHEN THE FUNCTION DEFINES A NAMED RETURN VARIABLE IS REDUNDANT AND ERRONEOUS

### Proof of Concept

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81-L86

The `previewRedeem` function already defines a named return variable `baseTokenAmount` .

The function should be altered as follows:
```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol

function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
    // Each ctoken exchange rate is stored as how much you should get for 1e18 of the particular cToken
    // Thus, we divide by 1e18 when returning the amount that a person should get when they provide
    // the amount of cTokens they're turning into the contract
 + baseTokenAmount = ((cTokenExchangeRates[cToken] * amount) / 1e18);
 - return (cTokenExchangeRates[cToken] * amount) / 1e18;
}
```

### Recommended Mitigation Steps

 `return (cTokenExchangeRates[cToken] * amount) / 1e18;`  should be removed as it is redundant and also does not use proper brackets which can result in confusion



## N-01  USE A MORE RECENT VERSION OF SOLIDITY

When deploying contracts, you should use the latest released version of Solidity. Apart from exceptional cases, only the latest version receives security fixes. Furthermore, breaking changes as well as new features are introduced regularly.

### Proof of Concept

There were 4 instances of this issue.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2

```
File: contracts/peg/SimpleFeiDaiPSM.sol    #1

pragma solidity ^0.8.4;
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #2

pragma solidity =0.8.10;
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2

```
File: contracts/shutdown/fuse/MerkleRedeemerDripper.sol    #3

pragma solidity =0.8.10;
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2

```
File: contracts/shutdown/redeem/TribeRedeemer.sol    #4

pragma solidity ^0.8.4;
```


### Recommended Mitigation Steps

Update to the latest released version of Solidity



## N-02  NATSPEC IS INCOMPLETE

It is recommended that _Solidity_ contracts are fully annotated using _NatSpec_

### Proof of Concept

There were 16 instances of this issue.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L26-L27

```
File: contracts/peg/SimpleFeiDaiPSM.sol    #1

/// @notice event emitted upon a redemption
event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
```
Missing: `@param to` , `@param amountFeiIn`, `@param amountAssetOut`


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L28-L29

```
File: contracts/peg/SimpleFeiDaiPSM.sol    #2

@notice event emitted when fei gets minted
event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```
Missing: `@param amountIn`, `@param amountFeiOut`


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L31-L37

```
File: contracts/peg/SimpleFeiDaiPSM.sol    #3

/// @notice mint `amountFeiOut` FEI to address `to` for `amountIn` underlying tokens
/// @dev see getMintAmountOut() to pre-calculate amount out
function mint(
    address to,
    uint256 amountIn,
    uint256 minAmountOut
) external returns (uint256 amountFeiOut) {
```
Missing:  `@param minAmountOut`, `@return amountFeiOut`


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L48-L52

```
File: contracts/peg/SimpleFeiDaiPSM.sol    #4

function redeem(
    address to,
    uint256 amountFeiIn,
    uint256 minAmountOut
) external returns (uint256 amountOut) {
```
Missing:  `@return amountOut`


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L52-L56

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #5

function claim(
    address _cToken,
    uint256 _amount,
    bytes32[] calldata _merkleProof
) external override hasSigned nonReentrant {
```
Missing: `@param _cToken`, `@param _amount`, `@param _merkleProof`


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L60-L64

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #6

function multiClaim(
    address[] calldata _cTokens,
    uint256[] calldata _amounts,
    bytes32[][] calldata _merkleProofs
) external override hasSigned nonReentrant {
````
Missing: `@param _cTokens`, `@param _amounts`, `@param _merkleProofs`


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L68

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #7

function redeem(address cToken, uint256 cTokenAmount) external override hasSigned nonReentrant {
````
Missing: `@param cTokenAmount`


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L72

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #8

function multiRedeem(address[] calldata cTokens, uint256[] calldata cTokenAmounts)
```
Missing: `@param cTokenAmounts


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #9

function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
```
Missing: `@return baseTokenAmount


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L88-L93

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #10

function signAndClaim(
bytes calldata signature,
address[] calldata cTokens,
uint256[] calldata amounts,
bytes32[][] calldata merkleProofs
) external override nonReentrant {
```
Missing: `@param signature


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L108-L114

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #11

function signAndClaimAndRedeem(
bytes calldata signature,
address[] calldata cTokens,
uint256[] calldata amountsToClaim,
uint256[] calldata amountsToRedeem,
bytes32[][] calldata merkleProofs
) external override hasNotSigned nonReentrant {
```
Missing: `@param amountsToClaim`, `@param amountsToRedeem`


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L124

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #12

function _configureExchangeRates(address[] memory _cTokens, uint256[] memory _exchangeRates) internal {
```
Missing: `@param _exchangeRates`


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L137

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #13

function _configureMerkleRoots(address[] memory _cTokens, bytes32[] memory _roots) internal {
```
Missing: `@param _roots


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L147

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #14

function _configureBaseToken(address _baseToken) internal {
```
Missing: `@param _baseToken


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L7-L16

```
File: contracts/shutdown/fuse/MerkleRedeemerDripper.sol    #15

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
Missing: `@param _core`, `@param _target`, `@param _dripPeriod`, `@param _amountToDrip`, `@param _token


https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L27-L31

```
File: contracts/shutdown/redeem/TribeRedeemer.sol    #16
    
constructor(
    address _redeemedToken,
    address[] memory _tokensReceived,
    uint256 _redeemBase
)
```
Missing: `@param _redeemedToken`, `@param _tokensReceived`, `@param _redeemBase`


## N-03  VARIABLE NAMES THAT CONSIST OF ALL CAPITAL LETTERS ARE RESERVED AND SHOULD BE USED FOR CONST/IMMUTABLE VARIABLES

Constants and immutable variables should be named in all capital letters

### Proof of Concept

There were 2 instances of this issue.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L75

```
File: contracts/peg/SimpleFeiDaiPSM.sol    #1

address public constant balanceReportedIn = address(DAI);
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98

```
File: contracts/peg/SimpleFeiDaiPSM.sol    #2

uint256 public constant mintFeeBasisPoints = 0;
uint256 public constant redeemFeeBasisPoints = 0;
address public constant underlyingToken = address(DAI);
uint256 public constant getMaxMintAmountOut = type(uint256).max;
bool public constant paused = false;
bool public constant redeemPaused = false;
bool public constant mintPaused = false;
```

### Recommended Mitigation Steps

Use all capital letters when naming constants/immutable variables



## N-04  UNNECESSARY ASSIGNMENT TO 0

uint256 is assigned to zero by default, additional reassignment to zero is unnecessary

### Proof of Concept

There were 8 instances of this issue.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92-L93

```
File: contracts/peg/SimpleFeiDaiPSM.sol    #1

- uint256 public constant mintFeeBasisPoints = 0;
+ uint256 public constant mintFeeBasisPoints;
- uint256 public constant redeemFeeBasisPoints = 0;
+ uint256 public constant redeemFeeBasisPoints;
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L128

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #2

- for (uint256 i = 0; i < _cTokens.length; i++) {
+ for (uint256 i; i < _cTokens.length; i++) {
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L141

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #3

- for (uint256 i = 0; i < _cTokens.length; i++) {
+ for (uint256 i; i < _cTokens.length; i++) {
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L193

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #4

- for (uint256 i = 0; i < _cTokens.length; i++) {
+ for (uint256 i; i < _cTokens.length; i++) {
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L229

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #5

- for (uint256 i = 0; i < cTokens.length; i++) {
+ for (uint256 i; i < cTokens.length; i++) {
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L247

```
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol    #6

- for (uint256 i = 0; i < cTokens.length; i++) {
+ for (uint256 i; i < cTokens.length; i++) {
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L53

```
File: contracts/shutdown/redeem/TribeRedeemer.sol    #7

- for (uint256 i = 0; i < tokensReceived.length; i++) {
+ for (uint256 i; i < tokensReceived.length; i++) {
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L71

```
File: contracts/shutdown/redeem/TribeRedeemer.sol    #8

- for (uint256 i = 0; i < tokens.length; i++) {
+ for (uint256 i; i < tokens.length; i++) {
```

