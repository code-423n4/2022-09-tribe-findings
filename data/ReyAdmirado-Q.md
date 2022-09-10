## 1. use of floating pragma

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

- [SimpleFeiDaiPSM.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2)

- [TribeRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2)


## 2. event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

- [SimpleFeiDaiPSM.sol#L27](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L27)
- [SimpleFeiDaiPSM.sol#L29](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L29)

- [TribeRedeemer.sol#L14](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L14)

## 3. constants should be defined rather than using magic numbers 

Even assembly can benefit from using readable constants instead of hex/numeric literals

- [RariMerkleRedeemer.sol#L85](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L85)
- [RariMerkleRedeemer.sol#L125](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125)
- [RariMerkleRedeemer.sol#L130](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L130)
- [RariMerkleRedeemer.sol#L138](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138)


## 4. Outdated compiler version

Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs


## 5. inconsistent use of named return variables

there is an inconsistent use of named return variables in the contract some functions return named variables, others return explicit values. consider adopting a consistent approach.
this would improve both the explicitness and readability of the code, and it may also help reduce regressions during future code refactors.

- [SimpleFeiDaiPSM.sol#L37](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L37)
- [SimpleFeiDaiPSM.sol#L52](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L52)

- [RariMerkleRedeemer.sol#L81](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81)

- [TribeRedeemer.sol#L47](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L47)


## 6. inconsistency in comments

this comment should have 2 slashes instead of 3

- [RariMerkleRedeemer.sol#L164](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L164)

remove comma after @dev

- [TribeRedeemer.sol#L56](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L56)

this comments should have 3 slashes instead of 2

- [TribeRedeemer.sol#L56](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L56)
- [TribeRedeemer.sol#L57](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L57)
