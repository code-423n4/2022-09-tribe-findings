## Low level issues:

1. require() check can be bypassed
File: `SimpleFeiDaiPSM.sol`
In function `mint()` and `redeem()`, the checks:
`require(amountFeiOut >= minAmountOut, "SimpleFeiDaiPSM: Mint not enough out");`
`require(amountOut >= minAmountOut, "SimpleFeiDaiPSM: Redeem not enough out");` 
can be bypassed, as the parameter `minAmountOut` is given as argument.

2. Typo in emit statements
File: `SimpleFeiDaiPSM.sol`
In function `mint()`, the last parameter of `emit` statement should be `amountOut` instead of `amountIn`. Though their values may be same, but the correction increases readability of the code.
Similarly for `redeem()` , the last parameter of `emit` statement should be `amountOut` instead of `amountFeiIn`

3. Two functions can be combined into 1, as their logic is same
File: `SimpleFeiDaiPSM.sol`
The function `getMintAmountOut` and `getRedeemAmountOut` should be merged because both of them just returns the value of the parameter passed, i.e their logic is same.

4. Unnecessary constants defined.
File: `SimpleFeiDaiPSM.sol`
Constants like `paused` , `redeemPaused`, `mintPaused` are defined, but the `mint` and `redeem` function doesn't use them. Ex: `mint` will not stop, even if `paused=true`. Furthermore there is no function to modify them.

5. Loop on unbounded arrays can lead to DOS
File: `TribeRedeemer.sol`
In `previewRedeem` function, `tokensReceived` array is unbounded. So if this array grows quite large, the transaction’s gas cost could exceed the block gas limit and make it impossible to call this function at all.
     `for (uint256 i = 0; i < tokensReceived.length; i++) {`
Also in `redeem` function, `tokens` array is unbounded, which may lead to DOS, if it has a very large length.
     `for (uint256 i = 0; i < tokens.length; i++) {`

6. Immutable address should be 0-checked
File: TribeRedeemer.sol : `address _redeemedToken` in constructor function

## Non-critical issues:

1. Avoid floating pragma. The version should be locked to a particular compiler version

File: SimpleFeiDaiPSM.sol : `pragma solidity ^0.8.4;`
File: TribeRedeemer.sol : `pragma solidity ^0.8.4;`

2. Update to the latest compiler version

The compiler of all the 4 in-scope contracts should be upgraded to the latest version, at least above the compiler version 0.8.10

3. Custom error messages should be used instead of revert strings, to save deployment cost
File: SimpleFeiDaiPSM.sol
mint(): `require(amountFeiOut >= minAmountOut, "SimpleFeiDaiPSM: Mint not enough out");`
redeem(): `require(amountOut >= minAmountOut, "SimpleFeiDaiPSM: Redeem not enough out");`