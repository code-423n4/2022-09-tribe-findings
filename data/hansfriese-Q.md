## [L-01] Wrong Comments
- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L19-L20

It checks if the balance is less than `amountToDrip` [here](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L23), but the above comment explains differently.

- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L56-L57

That calculation will work properly with different decimals because we calculate `redeemedAmount` according to the ratio between `amountIn` and `base`.

So `amountIn` and `base` are for `redeemedToken`, `redeemedAmount` and `balance` are for `tokensReceived[i]`.


## [L-02] There is no validation `_cTokens[i] != address(0)`
- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L133

- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L143

It's designed to configure 27 cTokens exactly so each cToken shouldn't be address(0).


## [L-03] Inconsistent validation of `cToken` for `RariMerkleRedeemer._redeem()` and `RariMerkleRedeemer._multiRedeem()`.
- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L201

- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L231

Inside the `_multiRedeem()`, it checks if `cTokens[i] != address(0)` but there is no such requirement for `_redeem()`.


## [L-04] Check address(0) before transfer funds
- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L72

Currently it doesn't check `to != address(0)` so all tokens might be lost when users input zero address by fault.


## [N-01] Event is missing indexed fields
Each event should use three indexed fields if there are three or more fields
- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L27
- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L29
- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L14
