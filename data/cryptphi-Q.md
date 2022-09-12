1. Floating pragma
The contracts should be deployed with the same solidity version with which they have been tested. Locking the pragma ensures that the contracts don't get deployed with an older version which might introduce bugs.

**Occurences**:
SimpleFeiDaiPSM.sol
TribeRedeemer.sol


2. Use of different compiler versions
Was observed that two different versions of solidity pragma are used. It is recommended to be consistent in the use of solidity pragma versions

**Occurrences**
SimpleFeiDaiPSM.sol
TribeRedeemer.sol
RariMerkleRedeemer.sol
MerkleRedeemerDripper.sol


3. Missing zero address check
The following functions are missing zero address checks which may cause either transfer of ERC20 tokens to address(0), or inability of redemption of tokens

**Occurrences**:
SimpleFeiDaiPSM.redeem() - may cause transfer of ERC20 tokens to address(0)
TribeRedeemer.redeem() - may cause transfer of ERC20 tokens to address(0)
TribeRedeemer.constructor() - may cause redeemToken to be set to address zero and users are unable to call TribeRedeemer.redeem() successfully due to wrong address of redeemToken in line 65.