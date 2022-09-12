## Issue 1: Use of Magic Values
In order to increase readability, it is recommended to replace magic values with constants.
e.g.

Change
```solidity
        require(_cTokens.length == 27, "Must provide exactly 27 merkle roots");
```
to
```solidity
uint256 public constant CTOKENS_LENGTH = 27;
        require(_cTokens.length == CTOKENS_LENGTH, "Must provide exactly 27 merkle roots");
```

Occurrences:
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L130
## Issue 2: Dev comment about decimals
In `previewRedeem` from `TribeRedeemer` it is commented that:
>            // @dev, this assumes all of `tokensReceived` and `redeemedToken`
>            // have the same number of decimals

This is not true because `amountIn` and `base` have the same number of decimals, so they cancel out and the resulting `redeemedAmount` has the same number of decimals as the ERC20 token.

## Issue 3: Update contract versions
Not all contracts are up-to-date, consider updating the solidity versions to 0.8.17

## Issue 4: No address verification when claiming `cToken`
In `RariMerkleRedeemer`, when a user runs `claim()` with an arbitrary address, the merkle root defaults to `bytes32(0)` and incorrectly reports an error that the merkle proof is not valid when in fact the real error is that the `cToken` address is incorrect

Add a check to make sure that the cToken address is valid
e.g. 
```solidity
require(merkleRoots[_cToken] != bytes32(0), "Incorrect cToken address");
```