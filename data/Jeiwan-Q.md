# [L-01] Missing `hasNotSigned` modifier
## Targets
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L93 
## Description
The `signAndClaim` function is missing the `hasNotSigned` modifier. Since the function calls `_sign`, the missing
modifier won't allow to bypass the signature check. However, a signature can be overwritten due to the missing
modifier.

# [L-02] FEI token inflation
## Targets
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L41
## Description
FEI token supply can be inflated using a series for `mint` + `redeem` calls, e.g.:
```solidity
function testFEIInflation() public {
    uint256 initialSupply = FEI.totalSupply();

    for (uint256 i; i < 10; ++i) {
        PSM.mint(address(this), 1000e18, 1000e18);
        PSM.redeem(address(this), 1000e18, 1000e18);
    }

    assertEq(FEI.totalSupply(), initialSupply + 10_000e18);
}
```
While this won't break the DAI backing, increased supply might be seemed suspicious by token holders.

Consider burning FEI in the [redeem function](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L55)
to avoid the extra supply.

# [L-03] Unnecessary function arguments
## Targets
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L36
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L51 
## Description
Since `minAmountOut` always equals to `amountIn` ([proof](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L62)),
it'll be better to comment out the arguments and remove the `require` checks so users don't need to figure out
the values:
```diff
diff --git a/contracts/peg/SimpleFeiDaiPSM.sol b/contracts/peg/SimpleFeiDaiPSM.sol
index 6e103a34..15b6ed2a 100644
--- a/contracts/peg/SimpleFeiDaiPSM.sol
+++ b/contracts/peg/SimpleFeiDaiPSM.sol
@@ -33,10 +33,9 @@ contract SimpleFeiDaiPSM {
     function mint(
         address to,
         uint256 amountIn,
-        uint256 minAmountOut
+        uint256 /* minAmountOut */
     ) external returns (uint256 amountFeiOut) {
         amountFeiOut = amountIn;
-        require(amountFeiOut >= minAmountOut, "SimpleFeiDaiPSM: Mint not enough out");
         DAI.safeTransferFrom(msg.sender, address(this), amountIn);
         FEI.mint(to, amountFeiOut);
         emit Mint(to, amountIn, amountIn);
``` 

# [L-04] Missing check for zero value
## Targets:
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L32
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L33
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L34
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L64 (`amountIn`)

# [N-01] Event is missing `indexed` fields
## Targets:
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L27
- https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L29