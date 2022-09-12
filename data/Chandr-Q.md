# Floating pragma

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
Proof of Concept
https://swcregistry.io/docs/SWC-103
Contracts:
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L2
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2

Recommended Mitigation Steps
Lock the pragma version.



# Local variables shadowing


Impact
If two variables with the same name exist in a function, but one is imported from another contract while the other is created locally, it is unclear which value is being used or should be used. Avoiding variable name collisions avoids confusion and the risks of using the wrong variable.
https://swcregistry.io/docs/SWC-119
Proof of Concept
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L11 (_core ) shadows 
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/refs/CoreRef.sol#L11

Recommended Mitigation Steps
Rename local variables to avoid shadowing. For instance, add an underscore in front of the name of local variables.

# Missing zero-address validations


While the codebase does a great job of input validation  for parameters of all kinds and especially addresses, there are a few  places where zero-address validations are missing. None of them are  catastrophic, will result in obvious reverts and can be reset given the  permissioned/controlled interactions with the contracts.
Nevertheless, it is helpful to add zero-address  validations to be consistent and ensure high availability of the  protocol with resistance to accidental misconfigurations.

POC

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L27-L35

