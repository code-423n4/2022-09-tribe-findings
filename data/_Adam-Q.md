### [L01] Missing hasNotSigned Modifier
**Description:**
The function signAndClaim() is missing the hasNotSigned modifier.

**LOC:**
[RariMerkleRedeemer.sol#L93](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L93) 

**Recommendation:**
Add hasNotSigned modifier to the signAndClaim() function.

### [L02] Unlocked Pragma
**Description:**
Non Library/Interface contracts should be deployed with a locked pragma version. This prevents the contract being deployed with a version that wasn't thoroughly tested against in development.

**LOC:**
[SimpleFeiDaiPSM.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L2)
[TribeRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L2) 

**Recommendation:**
Lock the pragma the version that was used in testing.

### [N01] Unnecessary Named Returns Value
**Description:**
The previewRedeem() function uses both named returns and return statements.

**LOC:**
[RariMerkleRedeemer.sol#L81](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81)  

**Recommendation:**
Remove named returns or assign line 85 to baseTokenAmount.