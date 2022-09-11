## Typo Mistakes
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L164

The comment should be rewritten as:
```
/// Should set the user's claim amount in the claims mapping for the provided cToken
```
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L246

The comment should be rewritten as:
```
// We give the interactions (the safeTransferFroms) their own for loop, just to be safe
```
## Zero Address Check

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L170
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L202

Just as it has been done for `_multiRedeem()`, the following zero address check for _cToken should be inserted as the first require statement for `_claim()` before line 170 and `_redeem()` before line 202 just in case of user's input error when writing directly at etherscan instead of interacting from the UI:
```
require(_cToken != address(0), "Invalid cToken address");
```
## Zero and Low Amounts Checks When Redeeming
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L217-L218
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L249-L250

A cToken exchange rate could be as small as 1e10 +1 according to line 130 inside `_configureExchangeRates()`. When dealing with very small amount of cToken at very low exchange rate, this could possibly render a zero returned value from `previewRedeem()' since the numerator would be smaller than 1e18. Similarly, contract balance of baseToken could run lower than baseTokenAmountReceived.  As such, the following require statement should be inserted prior to doing the token transfers:
```
require(baseTokenAmountReceived != 0 && baseTokenAmountReceived < IERC20(baseToken).balanceOf(address(this)),
        "Zero and/or insufficient base token payout!"); 
```
## Chainlink Keeper for MerkleRedeemerDripper.sol
Instead of inheriting ERC20Dripper and overriding `drip()`, consider implementing Chainlink keeper that would auto-fund RariMerkleRedeemer when the FEI balance dropped below a preset level.