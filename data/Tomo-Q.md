## ✅ N-1: Non-library/interface files should use fixed compiler versions, not floating ones

### 📝 Description
Non-library/interface files should use fixed compiler versions, not floating ones

### 💡 Recommendation
Delete the floating keyword `^`.

### 🔍 Findings:
```2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2``` [pragma solidity ^0.8.4;](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2 )

```2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2``` [pragma solidity ^0.8.4;](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2 )


## ✅ N-2: Use a more recent version of solidity

### 📝 Description
Use a solidity version of at least 0.8.4 to get bytes.concat() instead of abi.encodePacked (<bytes>, <bytes>)
Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked (<str>, <str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

### 💡 Recommendation
Use more recent version of solidity.

### 🔍 Findings:
```2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2``` [pragma solidity ^0.8.4;](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2 )

```2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2``` [pragma solidity =0.8.10;](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2 )

```2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2``` [pragma solidity =0.8.10;](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2 )

```2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2``` [pragma solidity ^0.8.4;](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2 )


## ✅ N-3: Use `string.concat()` or`bytes.concat()`

### 📝 Description
Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)Solidity version 0.8.12 introduces `string.concat()`(vs `abi.encodePacked(<str>,<str>)`)

### 💡 Recommendation
Use concat instead of abi.encodePacked

### 🔍 Findings:
```2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L174``` [bytes32 leafHash = keccak256(abi.encodePacked(msg.sender, _amount));](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L174 )

## ✅ N-4: Variable names that consist of all capital letters should be reserved for constant/immutable variables

### 📝 Description
Variable names that consist of all capital letters should be reserved for constant/immutable variables.

### 💡 Recommendation
Variables that are not constant/immutable should be declared in the lower case also, and the name of constant/immutable variables should be declared in capital letters

### 🔍 Findings:
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L75
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98