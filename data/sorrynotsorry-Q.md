# QA (LOW & NON-CRITICAL)

## [L-01] Missing modifier in RariMerkleRedeemer.sol
While the functions `signAndClaimAndRedeem`, `sign` have `hasNotSigned` modifier, the function `signAndClaim` does not have this modifier.

## [L-02] Redeeming in TribeRedeemer contract will throw panic when the redeemBase token amount is zero
The `redeem` function utilizes `previewRedeem` in order to calculate the `redeemedToken` amounts. However, it will panic if the `redeemBase` is zero.

```solidity
        uint256 base = redeemBase;
        for (uint256 i = 0; i < tokensReceived.length; i++) {
            uint256 balance = IERC20(tokensReceived[i]).balanceOf(address(this));
            require(balance != 0, "ZERO_BALANCE");
            // @dev, this assumes all of `tokensReceived` and `redeemedToken`
            // have the same number of decimals
            uint256 redeemedAmount = (amountIn * balance) / base;
```
There can be a require statement implemented to avoid the panic: `require(redeemBase != 0)`