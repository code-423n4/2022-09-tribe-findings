
---

## Summary

L-01 Missing checks for address(0x0) when assigning values to address state variables (4 instances)
L-02 abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256() (1 instance)
L-03 Unbounded loops with external calls (2 instances)
L-04 Unspecific Compiler Version Pragma (2 instances)

N-01 Unused library (1 instance)
N-02 constants should be defined rather than using magic numbers (1 instance)
N-03 Use a more recent version of solidity (4 instances)
N-04 Incomplete NatSpec (1 instance)
N-05 Event is missing indexed fields (3 instances)
N-06 Unnecessary default-value (5 instances)
 
Total: 24 instances in 15 issues

---

## L-01 Missing checks for address(0x0) when assigning values to address state variables 

4 instances in 2 files:

contracts/shutdown/fuse/MerkleRedeemerDripper.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L16 (3)

contracts/shutdown/redeem/TribeRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L32
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L33


## L-02 abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). “Unless there is a compelling reason, abi.encode should be preferred”. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

1 instance:

contracts/shutdown/fuse/RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L174


## L-03 Unbounded loops with external calls

The interface and the function should require a start index and a lenght, so that the index composition can be fetched in batches without running out of gas. 
If there are thousands of index components (e.g. like the Wilshire 5000 index), the function may revert

2 instances in 1 file:

contracts/shutdown/fuse/RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L124-L135
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L137-L145


## L-04 Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts. While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.
A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain. It is recommended to pin to a concrete compiler version.

2 instances:

contracts/peg/SimpleFeiDaiPSM.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2

contracts/shutdown/redeem/TribeRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2



## N-01 Unused library

The imported library is never used in the contract

1 instance:

contracts/shutdown/fuse/RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L7


## N-02 constants should be defined rather than using magic numbers

1 instance:

contracts/shutdown/fuse/RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L130


## N-03 Use a more recent version of solidity

Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>). 
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions. 
Use a solidity version of at least 0.8.14 to get bytes.concat() instead of abi.encodePacked(<bytes>,<bytes>) 

4 instances:

contracts/peg/SimpleFeiDaiPSM.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2

contracts/shutdown/fuse/MerkleRedeemerDripper.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2

contracts/shutdown/fuse/RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2

contracts/shutdown/redeem/TribeRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2


## N-04 NatSpec is incomplete

most functions in contract RariMerkleRedeemer do not have NatSpec

1 instance:

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol


## N-05 Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

3 instances in 2 files:

contracts/peg/SimpleFeiDaiPSM.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L27
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L29

contracts/shutdown/redeem/TribeRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L14


## N-06 Unnecessary default-value

More info: https://docs.soliditylang.org/en/v0.8.13/control-structures.html#default-value

5 instances in 1 file:

contracts/peg/SimpleFeiDaiPSM.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L93
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L96
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L97
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L98
