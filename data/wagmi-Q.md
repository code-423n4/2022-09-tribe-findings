# Summary

| Id | Title |
| -- | ----- |
| 1 | Users cannot claim if they have at least 2 valid leaves in merkle tree |
| 2 | Users can sign again even when they have already signed |

## 1. Users cannot claim if they have at least 2 valid leaves in merkle tree

For each (user, cToken) pair, contract `RariMerkleRedeemer` kept track of `claims[][]` amount. When users claim with any valid data, value of `claims` is updated only once. 

So if an user has 2 valid data with the same token in merkle tree, the second one cannot be claimed, since in [line 171](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L171), it checked that
```
require(claims[msg.sender][_cToken] == 0, "User has already claimed for this cToken.");
```

### Affected Codes
- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L171


## 2. Users can sign again even when they have already signed

In `RariMerkleRedeemer`, users have to sign a message before they can claim and redeem. And in every sign function, there is a `hasNotSigned` modifier to not allow users to sign again. Except in `signAndClaim()` function, it's lacking `hasNotSigned` modifier. 

Even though this does not affect the logic, it is inconsistent and should be fixed for better code quality.

### Affected Codes
- https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L93

### Recommendation
- Add `hasNotSigned` modifier to `signAndClaim` function.