# GAS REPORT

## [GAS 00] Declare as payable If the onlyOwner modifier is present
You can save gas by adding a payable modifier to functions that can be only called by protocol owners.

### Proof of concept:
- [FeiTimedMinter.sol#L75](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/fei/minter/FeiTimedMinter.sol#L75)
- [TribalChief.sol#L316](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/staking/TribalChief.sol#L316)
- [BalancerLBPSwapper.sol#L101](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pcv/balancer/BalancerLBPSwapper.sol#L101)
- [Core.sol#L38](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/core/Core.sol#L38)
- [MultiRateLimited.sol#L98](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/utils/MultiRateLimited.sol#L98)

--