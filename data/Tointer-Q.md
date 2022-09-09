## Can sign multiple times
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L93

In other functions with sign functionality there is `hasNotSigned` modifier, but not in this one. This can lead to multiple Signed events and possible disruptions of offchain services or frontend interfaces. Solution: add `hasNotSigned` modifier