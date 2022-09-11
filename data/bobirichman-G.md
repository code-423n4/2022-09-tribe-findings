
# GAS REPORT

## [GAS] Mark as payable If has onlyOwner modifier
In order to save gas you can put a payable modifier for functions that are called by protocol owners.

### Proof of concept:
- [BalancerGaugeStaker.sol#L26](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/metagov/BalancerGaugeStaker.sol#L26)
- [NamedStaticPCVDepositWrapper.sol#L162](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/utils/NamedStaticPCVDepositWrapper.sol#L162)

## [GAS] Cache array size
You can cache the array size to improve gas usage in the following locations

### Proof of concept:
- [FuseFixer.sol#L172](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/utils/FuseFixer.sol#L172)
- [RariMerkleRedeemer_flattened.sol#L2234](https://github.com/code-423n4/2022-09-tribe/tree/main/scripts/shutdown/data/prod/RariMerkleRedeemer_flattened.sol#L2234)

## [GAS] Use > instead != to compare uint with 0


### Proof of concept:
- [PegStabilityModule.sol#L175](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/PegStabilityModule.sol#L175)
- [TribalChief.sol#L302](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/TribalChief.sol#L302)

## [GAS] Use bytes32 in the following locations


### Proof of concept:
- [GovernanceMetadataRegistry.t.sol#L48](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/governance/GovernanceMetadataRegistry.t.sol#L48)
- [GovernanceMetadataRegistry.t.sol#L39](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/governance/GovernanceMetadataRegistry.t.sol#L39)

## [GAS] Use abiEncodePacked()


### Proof of concept:
- [PodAdminGateway.sol#L48](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pods/PodAdminGateway.sol#L48)
- [Tribe.sol#L269](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/tribe/Tribe.sol#L269)

## [GAS] In the following revert statements consider using custom error instead a message


### Proof of concept:
- [CollateralizationOracle.sol#L202](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/oracle/collateralization/CollateralizationOracle.sol#L202)
- [Timelock.sol#L67](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/dao/timelock/Timelock.sol#L67)

## [GAS] Use assembly opcodes iszero in the following locations


### Proof of concept:
- [AutoRewardsDistributor.sol#L56](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fuse/rewards/AutoRewardsDistributor.sol#L56)
- [Timelock.sol#L148](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/dao/timelock/Timelock.sol#L148)
