## Typo Mistakes
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L164

The comment should be rewritten as:

/// Should set the user's claim amount in the claims mapping for the provided cToken

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L246

The comment should be rewritten as:

// We give the interactions (the safeTransferFroms) their own for loop, just to be safe

## Zero Address Check

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L170
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L202

A zero address check for _cToken should be inserted as the first require statement for `_claim()` before line 170 and `_redeem()` before line 202 just in case of user's input error when writing directly at etherscan instead of interacting from the UI:

require(_cToken != address(0), "Invalid cToken address");

