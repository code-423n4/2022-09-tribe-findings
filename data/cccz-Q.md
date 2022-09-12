## [Low-01] Hardcoded token address

Hardcoded DAI and FEI token addresses are used in SimpleFeiDaiPSM contract, this is not a good practice, if the token address changes make the contract unusable, variables should be used as the token address

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L15-L20



## [Low-02] RariMerkleRedeemer: signAndClaim should use hasNotSigned modifier

In the RariMerkleRedeemer contract, the hasNotSigned modifier should be used when the function requires the user to provide a signature, like the sign and signAndClaimAndRedeem functions, so signAndClaim should also use the hasNotSigned modifier.

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L88-L97