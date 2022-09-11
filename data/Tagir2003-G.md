# GAS REPORT

## [GAS] Consider using custom errors
You can utilize customized errors in require statements to save gas.

### Proof of concept:
- [RoleBastion.sol#L22](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pods/RoleBastion.sol#L22)
- [RariMerkleRedeemer_flattened.sol#L2285](https://github.com/code-423n4/2022-09-tribe/tree/main/scripts/shutdown/data/prod/RariMerkleRedeemer_flattened.sol#L2285)

## [GAS] The following require statements could be split into multiple require statements to save gas


### Proof of concept:
- [FuseWithdrawalGuard.sol#L39](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/sentinel/guards/FuseWithdrawalGuard.sol#L39)
- [TribeRagequit.sol#L69](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/merger/TribeRagequit.sol#L69)

## [GAS] Cache the following arrays size before the loop


### Proof of concept:
- [RariMerkleRedeemer.sol#L127](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L127)
- [FuseFixer.sol#L131](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/utils/FuseFixer.sol#L131)

## [GAS] Caching msg.sender is unnecessary


### Proof of concept:
- [PodFactory.sol#L216](https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/pods/PodFactory.sol#L216)
- [RariMerkleRedeemer_flattened.sol#L2222](https://github.com/code-423n4/2022-09-tribe/tree/main/scripts/shutdown/data/prod/RariMerkleRedeemer_flattened.sol#L2222)

--