## TribeRedeemer can't redeem if a token has zero balance

It should skip that token instead of revert the entire transaction.

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L44-L61

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

Change to

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
            // require(balance != 0, "ZERO_BALANCE");
            // @dev, this assumes all of `tokensReceived` and `redeemedToken`
            // have the same number of decimals
            uint256 redeemedAmount = (amountIn * balance) / base;
            amountsOut[i] = redeemedAmount;
        }
    }
```