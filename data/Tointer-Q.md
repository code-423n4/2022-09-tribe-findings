## RariMerkleRedeemer: Can sign multiple times
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L93

In other functions with sign functionality there is `hasNotSigned` modifier, but not in this one. This can lead to multiple Signed events and possible disruptions of offchain services or frontend interfaces. Solution: add `hasNotSigned` modifier

## TribeRedeemer: Can redeem 0 amount
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L64
It's a good practice to add `require(amountIn > 0)`, so users won't get confused and there would not be excess `Redeemed` events

## TribeRedeemer: All or nothing redeem
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L71
If some of the transfers fail, user wouldn't be able to redeem all other tokens. But, out of the tokens that was mentioned, only stETH has such risk (they can upgrade contract with blacklist functionality, for example). stETH is pretty reputable so risk is non-existant.