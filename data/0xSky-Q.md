# cToken validation is missing

In `_redeem` of `RariMerkleRedeemer.sol`, `cToken` validation is missing. But `_multiRedeem` correctly validates ctoken addresses [here](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L231). The impact is not high, but I recommend to use the same validation in `_redeem`.

# validation is missing in the constructor of TribeRedeemer
In the constructor of TribeRedeemer, there are three parameters and there is no validation about them.

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L27-L35

If `_redeemBase = 0`, `previewRedeem` and `redeem` will revert.
And if `_redeemedToken = address(0)`, `previewRedeem` is okay, but `redeem` will revert. So I recommend to validate those inputs.


