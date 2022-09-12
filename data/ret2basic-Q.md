# FEI and TRIBE Redemption Contest QA Report

## Summary

The following QA issues were found during the code audit:

1. Outdated compiler version (1 instance)
2. Inconsistent natspec (1 instance)
3. Consider adding `require()` statement or emitting an event (1 instance)

Total 3 instances of 3 issues.

## 1. Outdated compiler version (1 instance)

Some contracts use `pragma solidity ^0.8.4`, which was released on Apr 21, 2021. This compiler version is 18-month old by now. The latest version is 0.8.17 and it was released 4 days ago.

## 2. Inconsistent natspec (1 instance)

In `contracts/shutdown/fuse/MerkleRedeemerDripper.sol`, the natspec does not match the implementation:

```solidity
/// @notice Overrides drip() in the ERC20Dripper contract to add a balance check on the target
/// @dev This will revert if there are < amountToDrip tokens in this contract, so make sure
/// that it is funded with a multiple of amountToDrip to avoid this case.
function drip() public override {
    require(
        IERC20(token).balanceOf(target) < amountToDrip,
        "MerkleRedeemerDripper: dripper target already has enough tokens."
    );

    super.drip();
}
```

The `require()` statement will revert if `IERC20(token).balanceOf(target) >= amountToDrip`, so either the implementation is wrong or the natspec is wrong.

## 3. Consider adding `require()` statement or emitting an event (1 instance)

In `contracts/peg/SimpleFeiDaiPSM.sol`:

```solidity
function burnFeiHeld() external {
    uint256 feiBalance = FEI.balanceOf(address(this));
    if (feiBalance != 0) {
        FEI.burn(feiBalance);
    }
}
```

This function will do nothing if the balance of this contract is 0. This behavior could be confusing and it is hard for debugging. Consider adding `require(feiBalance != 0)` or emitting an event saying the burn operation failed.
