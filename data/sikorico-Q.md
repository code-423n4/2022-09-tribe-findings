# QA REPORT

## [QA 00] Use SafeMath in the following contracts to avoid unexpected overflow/underflow


### Proof of concept:
- [DSTest.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/utils/DSTest.sol)

## [QA 01] The following require statements are missing an error message


### Proof of concept:
- [BalancerPool2Lens.sol#L72](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerPool2Lens.sol#L72)
- [BPTLens.sol#L61](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BPTLens.sol#L61)

## [QA 02] Some tokens needs approve 0 before any approve above 0
Consider using increase/decrease approve notation instead approve to deal with that.

### Proof of concept:
- [rariMerkleRedeemer.t.sol#L663](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/shutdown/fuse/rariMerkleRedeemer.t.sol#L663)
- [NonCustodialPSM.t.sol#L148](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/unit/peg/NonCustodialPSM.t.sol#L148)
- [CurvePCVDepositPlainPool.sol#L87](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/curve/CurvePCVDepositPlainPool.sol#L87)
- [BalancerLBPSwapper.sol#L176](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerLBPSwapper.sol#L176)
- [TribeRedeemer.t.sol#L80](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/unit/shutdown/redeemer/TribeRedeemer.t.sol#L80)

## [QA 03] Missing MIT licence for the following contracts


### Proof of concept:
- [BalanceGuard.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/examples/BalanceGuard.sol)
- [MultiActionGuard.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/examples/MultiActionGuard.sol)
- [GOhmEthOracle.t.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/unit/oracle/GOhmEthOracle.t.sol)
- [SmartYieldRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/barnbridge/SmartYieldRedeemer.sol)
- [OwnableTimedMinter.sol](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fei/minter/OwnableTimedMinter.sol)

## [QA 04] Remove the name from the following unused function parameters


### Proof of concept:
- [PegStabilityModule.sol#L386](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/PegStabilityModule.sol#L386)
- [AutoRewardsDistributorV2.sol#L54](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fuse/rewards/AutoRewardsDistributorV2.sol#L54)
- [NopeDAO.sol#L37](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/dao/nopeDAO/NopeDAO.sol#L37)
- [VotiumBriber.sol#L34](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/convex/VotiumBriber.sol#L34)
- [TribeRagequit.sol#L51](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/merger/TribeRagequit.sol#L51)

## [QA 05] Not emitted event for state changes


### Proof of concept:
- [NopeDAO.t.sol#L38](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/integration/governance/NopeDAO.t.sol#L38)
- [NopeDAO.t.sol#L52](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/test/unit/governance/NopeDAO.t.sol#L52)
- [AutoRewardsDistributorV2.sol#L54](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fuse/rewards/AutoRewardsDistributorV2.sol#L54)
- [VotiumBriber.sol#L34](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/convex/VotiumBriber.sol#L34)
- [RestrictedPermissions.sol#L23](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/core/RestrictedPermissions.sol#L23)

## [QA 06] Both return statement and named return
For readability purposes consider having one of the two return options (for the following functions)

### Proof of concept:
- [ConvexPCVDeposit.sol#L113](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/convex/ConvexPCVDeposit.sol#L113)
- [BalanceGuard.sol#L23](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/examples/BalanceGuard.sol#L23)
- [MaxFeiWithdrawalGuard.sol#L97](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/MaxFeiWithdrawalGuard.sol#L97)
- [RecoverEthGuard.sol#L20](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/examples/RecoverEthGuard.sol#L20)
- [BalancerPCVDepositWeightedPool.sol#L123](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerPCVDepositWeightedPool.sol#L123)
