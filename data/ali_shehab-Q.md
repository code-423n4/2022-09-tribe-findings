Hi there is 2 function that do not do what is mentioned in the comment.

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
The first one mention it calculates the amount of FEI out for a given amountIn however it returns the amountIn directly.
The second is the same.