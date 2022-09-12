# [L-01] Lack of checks-effects-interactions for `redeem()`

The `redeem`() function inside the `TribeRedeemer.sol` contract will execute the external transfer call for the `redeemedToken` before updating the state variable `redeemBase`. 

```
function redeem(address to, uint256 amountIn) external nonReentrant {
    IERC20(redeemedToken).safeTransferFrom(msg.sender, address(this), amountIn);

    (address[] memory tokens, uint256[] memory amountsOut) = previewRedeem(amountIn);

    uint256 base = redeemBase;
    redeemBase = base - amountIn; // decrement the base for future redemptions
    for (uint256 i = 0; i < tokens.length; i++) {
        IERC20(tokens[i]).safeTransfer(to, amountsOut[i]);
    }

    emit Redeemed(msg.sender, to, amountIn, base);
}
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L64-L76

The function is not at risk of reentrancy due to the `nonReentrancy` modifier, but the state update should be executed before the external to act in conformance with the `checks-effects-interactions` pattern, similarly to what's done in the `_redeem()` function for the `RariMerkleRedeemer.sol` contract.

# [L-02] Add a check to ensure `redeemBase` is bigger than zero

For the `TribeRedeemer.sol` constructor, if `redeemBase` accidentaly receives the value 0, the function `previewRedeem()` will always generate a divide-by-zero error. 

```
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

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L27-L35

```
    function previewRedeem(uint256 amountIn)
        public
        view
        returns (address[] memory tokens, uint256[] memory amountsOut)
    {
        tokens = tokensReceivedOnRedeem();
        amountsOut = new uint256[](tokens.length);

        uint256 base = redeemBase;
        for (uint256 i = 0; i < tokensReceived.length; i++) {
            uint256 balance = IERC20(tokensReceived[i]).balanceOf(address(this));
            require(balance != 0, "ZERO_BALANCE");
            // @dev, this assumes all of `tokensReceived` and `redeemedToken`
            // have the same number of decimals
            uint256 redeemedAmount = (amountIn * balance) / base;
            amountsOut[i] = redeemedAmount;
        }
    }
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L45-L61

## Recommended Mitigation Steps

Add a check to ensure `redeemBase` is bigger than zero to avoid the need for redeployments in case of user-setting accidental mistakes.

# [L-03] Use a more recent version of solidity and avoid rounding the pragma version where possible

Avoid using floating pragmas, e.g.`TribeRedeemer.sol` is using ^0.8.4.

Furthermore, all the contracts in scope are using OpenZeppelin, which makes use of inline assembly. Solidity version prior to 0.8.15 has a bug related to inline assemby: [Optimizer Bug Regarding Memory Side Effects of Inline Assembly](https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/).

## Recommened Mitigation Steps

Use the latest version of solidity where possible to ensure the project is in conformance with the latest bugfixes in solidity.

# [NC-01] Add an event if the `burnFeiHeld()` function is called to facilidate offchain monitoring

```
function burnFeiHeld() external {
    uint256 feiBalance = FEI.balanceOf(address(this));
    if (feiBalance != 0) {
        FEI.burn(feiBalance);
    }
}
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L105-L110

# [NC-02] Missing NATSPEC

```
138: function _configureMerkleRoots(address[] memory _cTokens, bytes32[] memory _roots) internal {
```

```
148: function _configureBaseToken(address _baseToken) internal {
```

# [NC-03] `previewRedeem()` is declaring a return named variable and manually returning a value

```
function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
    // Each ctoken exchange rate is stored as how much you should get for 1e18 of the particular cToken
    // Thus, we divide by 1e18 when returning the amount that a person should get when they provide
    // the amount of cTokens they're turning into the contract
    return (cTokenExchangeRates[cToken] * amount) / 1e18;
}
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81-L86

The function `previewRedeem()` can be refactored to use the return named variable or maintain the current return and removed the named variable in the function header.

Option 1:

```
function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
    baseTokenAmount = (cTokenExchangeRates[cToken] * amount) / 1e18;
}
```

Option 2:

```
function previewRedeem(address cToken, uint256 amount) public view override returns (uint256) {
    return (cTokenExchangeRates[cToken] * amount) / 1e18;
}
```

# [NC-04] Repeated required statements can be refactored into a single function modifier to improve code reusability

```
function _configureExchangeRates(address[] memory _cTokens, uint256[] memory _exchangeRates) internal {
    require(_cTokens.length == 27, "Must provide exactly 27 exchange rates.");
    require(_cTokens.length == _exchangeRates.length, "Exchange rates must be provided for each cToken");
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L124-L126


```
function _configureMerkleRoots(address[] memory _cTokens, bytes32[] memory _roots) internal {
    require(_cTokens.length == 27, "Must provide exactly 27 merkle roots");
    require(_cTokens.length == _roots.length, "Merkle roots must be provided for each cToken");
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L137-L139

# [NC-05] `tokenReceived` can be converted into a public state variable which will remove the need for the `tokensReceivedOnRedeem()` function

```
address[] private tokensReceived;
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L20

```
function tokensReceivedOnRedeem() public view returns (address[] memory) {
    return tokensReceived;
}
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L38-L40

By refactoring `tokensReceived` to public, solidity will create a getter function automatically.
