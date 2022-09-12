L-01 Floating Pragmas
In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Proof of concept
https://swcregistry.io/docs/SWC-103

Instances:
1. https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2
2. https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2

Recommendation:
Lock the pragma version into recent version of Solidity at least 0.8.15

L-02 Use recent version of Solidity
Using an outdated compiler version can be problematic especially if there are publicly disclosed bugs and issues that affect the current compiler version.

Proof of concept
https://swcregistry.io/docs/SWC-102

Instances:
1. https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2
2. https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2
3. https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2
4.https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2

Recommendation:
Change the version to at least 0.8.15