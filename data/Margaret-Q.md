# QA REPORT

## [QA 00] Add Timelock for the following functions:


### Proof of concept:
- [CollateralizationOracleGuardian.sol#L88](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/oracle/collateralization/CollateralizationOracleGuardian.sol#L88)
- [FuseWithdrawalGuard.sol#L58](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/FuseWithdrawalGuard.sol#L58)
- [NopeDAO.sol#L46](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/dao/nopeDAO/NopeDAO.sol#L46)
- [TribeTimelockedDelegatorBurner.t.sol#L22](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/shutdown/timelocks/TribeTimelockedDelegatorBurner.t.sol#L22)
- [WeightedBalancerPoolManager.sol#L13](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/manager/WeightedBalancerPoolManager.sol#L13)

## [QA 01] For solidity version less than 8, use safe math.


### Proof of concept:
- [DSTest.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/utils/DSTest.sol)

## [QA 02] Two steps verification process is missing.
The process of transferring ownership is risky because typing the wrong address can have serious consequences.

### Proof of concept:
- [CoreRef.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/refs/CoreRef.sol)
- [BaseBalancerPoolManager.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/manager/BaseBalancerPoolManager.sol)
- [AutoRewardsDistributor.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fuse/rewards/AutoRewardsDistributor.sol)
- [AutoRewardsDistributorV2.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fuse/rewards/AutoRewardsDistributorV2.sol)
