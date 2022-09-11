
Table Of Content
================

* [GAS REPORT](#gas-report)
        * [Not Efficient Struct Packing](#not-efficient-struct-packing)
        * [Caching array size](#caching-array-size)
        * [Split require statement with & operator](#split-require-statement-with--operator)
        * [Use bytes32 instead string whenever possible](#use-bytes32-instead-string-whenever-possible)
        * [The following assignments are not in use](#the-following-assignments-are-not-in-use)

# GAS REPORT

## Not Efficient Struct Packing
By reordering the struct variables you can decrease the number of slots in use and therefore reduce the gas cost of using the struct.

### Code Instances:
- [PegStabilityModule.sol#L56](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/PegStabilityModule.sol#L56)
- [GovernorAlpha.sol#L49](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/dao/governor/GovernorAlpha.sol#L49)

## Caching array size
In the following for loops consider caching the array size instead of loading it every iteration.

### Code Instances:
- [RariMerkleRedeemer.sol#L125](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125)
- [RariMerkleRedeemer_flattened.sol#L2288](https://github.com/code-423n4/2022-09-tribe/tree/main/scripts/shutdown/data/prod/RariMerkleRedeemer_flattened.sol#L2288)
- [BalancerPCVDepositWeightedPool.sol#L64](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerPCVDepositWeightedPool.sol#L64)
- [RariMerkleRedeemer_flattened.sol#L2342](https://github.com/code-423n4/2022-09-tribe/tree/main/scripts/shutdown/data/prod/RariMerkleRedeemer_flattened.sol#L2342)

## Split require statement with & operator
instead of require(A & B, ...) consider having two require statements require(A, ...) and require(B, ...) for better code quality and improved gas usage.

### Code Instances:
- [CollateralizationOracleWrapper.sol#L91](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/oracle/collateralization/CollateralizationOracleWrapper.sol#L91)
- [BPTLens.sol#L95](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BPTLens.sol#L95)
- [BalancerPool2Lens.sol#L103](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerPool2Lens.sol#L103)
- [Fei.sol#L130](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fei/Fei.sol#L130)

## Use bytes32 instead string whenever possible


### Code Instances:
- [GovernanceMetadataRegistry.t.sol#L26](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/governance/GovernanceMetadataRegistry.t.sol#L26)
- [NopeDAO.t.sol#L122](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/governance/NopeDAO.t.sol#L122)
- [GovernanceMetadataRegistry.t.sol#L39](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/governance/GovernanceMetadataRegistry.t.sol#L39)

## The following assignments are not in use


For instance, [FuseFixer.sol#L120](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/utils/FuseFixer.sol#L120)
