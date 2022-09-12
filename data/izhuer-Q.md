# Locked Funds in `RariMerkleRedeemer` Contract

Without a migration function,  funds in `RariMerkleRedeemer` may be locked. It is possible  if a multiple of `amountToDrip` (in https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L18-L29) exceeds the total amount of redemption. 

# Locked Funds in `TribeRedeemer` Contract
Without a migration function,  funds in `TribeRedeemer` may be locked. It is almost impossible to happen, but when someone invokes `redeem(address to, uint256 amountIn)` and specifies `to` as the address of `TribeRedeemer`, the funds will get locked.
