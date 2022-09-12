# QA REPORT 
## Number of Issues: 2

# 1.   NON-LIBRARY/INTERFACE FILES SHOULD USE FIXED COMPILER VERSIONS, NOT FLOATING ONES 

## Impact
Avoid floating pragmas for non-library contracts.
While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.
A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.
It is recommended to pin to a concrete compiler version.

## Mitigation
Used a fixed compiler version

## Proof of Concept
```
  2022-09-tribe\contracts\peg\SimpleFeiDaiPSM.sol::2 => pragma solidity ^0.8.4;
  2022-09-tribe\contracts\shutdown\fuse\MultiMerkleRedeemer.sol::2 => pragma solidity ^0.8.4;
  2022-09-tribe\contracts\shutdown\redeem\TribeRedeemer.sol::2 => pragma solidity ^0.8.4;
```

## References 
https://code4rena.com/reports/2022-06-nibbl/#n-16-non-libraryinterface-files-should-use-fixed-compiler-versions-not-floating-ones

# 2. Do not use Deprecated Library Functions

## Impact
The usage of deprecated library functions should be discouraged.
This issue is mostly related to OpenZeppelin libraries.

## Proof of Concept

```
  2022-09-tribe\contracts\utils\FuseFixer.sol::151 => SafeERC20.safeApprove(IERC20(underlying), ctoken, type(uint256).max);
  2022-09-tribe\contracts\core\Permissions.sol::152 => _setupRole(GOVERN_ROLE, governor);
  2022-09-tribe\contracts\pcv\liquity\BAMMDeposit.sol::32 => lusd.safeApprove(address(BAMM), amount)
```