# Unnecessary code

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L60-L68
```
    /// @notice calculate the amount of FEI out for a given `amountIn` of underlying
    function getMintAmountOut(uint256 amountIn) external pure returns (uint256) {
        return amountIn;
    }

    /// @notice calculate the amount of underlying out for a given `amountFeiIn` of FEI
    function getRedeemAmountOut(uint256 amountIn) external pure returns (uint256) {
        return amountIn;
    }
```

Not sure why this part of code being written in the contract, because the function just returns the parameter. The contract is also not a derived contract.
 
