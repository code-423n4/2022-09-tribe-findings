## Zero address check missing

Contract:
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L64

Issue:
In redeem function, It is not checked whether "to" is address(0)

Recommendation:
Add a check

```
require(to!=address(0), "Invalid address");
```

## Missing modifier

Contract:
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L88

Issue:
In signAndClaim function, hasNotSigned modifier is missing

Recommendation:
Add modifier hasNotSigned in the signAndClaim function

## Check missing on redeemedAmount

Contract:
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L58

Issue:
In previewRedeem function, redeemedAmount should always be lesser than total token balance