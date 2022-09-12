# QA report

## Non-critical

### [N-01] The contracts use unlocked pragma

In **contracts/peg/SimpleFeiDaiPSM.sol** and **contracts/shutdown/redeem/TribeRedeemer.sol** lock the pragma to an specific version


### [N-02] Burn receive tokens

The **contracts/shutdown/redeem/TribeRedeemer.sol** could burn the tokens received by `redeem` function

```diff
    function redeem(address to, uint256 amountIn) external nonReentrant {
+       IERC20 _redeemedToken = IERC20(redeemedToken);
-       IERC20(redeemedToken).safeTransferFrom(msg.sender, address(this), amountIn);
+       _redeemedToken.safeTransferFrom(msg.sender, address(this), amountIn);
+       _redeemedToken.burn(amountIn);

        (address[] memory tokens, uint256[] memory amountsOut) = previewRedeem(amountIn);

        uint256 base = redeemBase;
        redeemBase = base - amountIn; // decrement the base for future redemptions
        for (uint256 i = 0; i < tokens.length; i++) {
            IERC20(tokens[i]).safeTransfer(to, amountsOut[i]);
        }

        emit Redeemed(msg.sender, to, amountIn, base);
    }
```

### [N-03] Typo

In **contracts/shutdown/fuse/RariMerkleRedeemer.sol#L164:** 
From: `/// Should set the user's claim amount int he claims mapping for the provided cToken`
To: `// Should set the user's claim amount in the claims mapping for the provided cToken`

## Low Risk

### [L-01] Events are not indexed

The emitted events are not indexed, making off-chain scripts such as front-ends of dApps difficult to filter the events efficiently.

**Recommended Mitigation Steps:** Add the `indexed` keyword in _filter_ parameters of the events

In contracts/peg/SimpleFeiDaiPSM.sol:
From:
```solidity
L27: event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
L29: event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```
To:
```solidity
L27: event Redeem(address indexed to, uint256 amountFeiIn, uint256 amountAssetOut);
L29: event Mint(address indexed to, uint256 amountIn, uint256 amountFeiOut);
```

### [L-02] In the function `previewRedeem` in **contracts/shutdown/redeem/TribeRedeemer.sol** the `amountIn` should be lower or equal than `base` by definition

```diff
  function previewRedeem(uint256 amountIn)
      public
      view
      returns (address[] memory tokens, uint256[] memory amountsOut)
  {
      tokens = tokensReceivedOnRedeem();
      amountsOut = new uint256[](tokens.length);

      uint256 base = redeemBase;
+     require(base >= amountsIn, "High amountsIn");
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

### [L-03] Validate constructor parameters(TribeRedeemer.sol)

In the `constructor` of **contracts/shutdown/redeem/TribeRedeemer.sol**:
 - The `_redeemedToken` parameter should not be the `address(0)`
 - All tokens inside `_tokensReceived` array should not be `address(0)`
 - The `_redeemBase` parameter should not be `0`

```diff
    constructor(
        address _redeemedToken,
        address[] memory _tokensReceived,
        uint256 _redeemBase
    ) {
+       require(_redeemedToken != address(0), "_redeemedToken should not be address(0)");
        redeemedToken = _redeemedToken;
+       uint256 _tokensReceivedLength;
+       for (uint256 i; i < _tokensReceivedLength;) {
+           require(_tokensReceived[i] != address(0), "All tokensReceived should not be address(0)");
+           unchecked{ ++i; }
+       }
        tokensReceived = _tokensReceived;
+       require(_redeemBase != 0, "_redeemBase should not be 0");
        redeemBase = _redeemBase;
    }
```

### [L-04] Validate constructor parameters(RariMerkleRedeemer.sol)

In the `constructor` of **contracts/shutdown/fuse/RariMerkleRedeemer.sol** the `cTokens` should not contains any `address(0)`

```diff
    constructor(
        address token,
        address[] memory cTokens,
        uint256[] memory rates,
        bytes32[] memory roots
    ) {
+       uint256 cTokensLength;
+       for (uint256 i; i < cTokensLength;) {
+           require(cTokens[i] != address(0), "All cTokens should not be address(0)");
+           unchecked{ ++i; }
+       }
        _configureExchangeRates(cTokens, rates);
        _configureMerkleRoots(cTokens, roots);
        _configureBaseToken(token);
    }
```

### [L-05] Poor signature message

The `MESSAGE_HASH` used in `_sign` in [RariMerkleRedeemer.sol#L155](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L155) defined in [MultiMerkleRedeemer.sol#L56](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L56) use a poor message ["Sample message, please update."](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L53)
This message don't represent anything, also says update it

### [L-06] The rates on **RariMerkleRedeemer.sol** could expire

The rates setter in the `constructor` of **contracts/shutdown/fuse/RariMerkleRedeemer.sol** can expire
add an expiry date and update the rate in a window time

### [L-07] Miss `hasNotSigned`

In the `signAndClaim` function miss `hasNotSigned` modifier