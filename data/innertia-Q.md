
# List of Issues
* 01: The denominator of the division may be zero.
* 02: Zero check on argument is missing.
* 03: Floating Pragma
* 04: The event argument names are seemingly incorrect. 
* 05: Snake case is not used for `constant`.
* 06: `DAI.balanceOf(address(this))` is written in multiple places.
* 07: The same `require` statement is described in multiple places.
* 08: Lack of NatSpec comment
* 09: The `event` is not `emit` when the critical value is set.
# Issues
## 01: The denominator of the division may be zero.
### Impact - Low risk
`redeemBase` is the denominator for calculating the `redeemedAmount`, which may be zero as it gets smaller in the `redeem` function.
### Proof of Concept
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L58
### Recommended Mitigation Steps
Add a `require` statement to guarantee that `redeemBase` is not zero.
## 02: Zero check on argument is missing. 
### Impact - Low risk
To prevent unintended behavior, the arguments of these functions should be zero-checked.
### Proof of Concept
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L34

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L49

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L36

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L166

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L201

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L44

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L64

### Recommended Mitigation Steps
Add `require` statement for zero-checking.
## 03: Floating Pragma
### Impact - Non-critical
Contracts should be deployed with the same compiler version and flags that have been thoroughly tested.
### Proof of Concept
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L2

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L2

### Recommended Mitigation Steps
Lock the pragma version.
## 04: The event argument names are seemingly incorrect. 
### Impact - Non-critical
The value to emit is correct, but the argument names are seemingly wrong.
### Proof of Concept
```
/// @notice event emitted when fei gets minted
    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```
```
emit Mint(to, amountIn, amountIn);
```
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L42

```
/// @notice event emitted upon a redemption
    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
```
```
emit Redeem(to, amountFeiIn, amountFeiIn);
```
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L57

### Recommended Mitigation Steps
Appropriate argument names should be used.  
`amountFeiOut == amountIn`, `amountFeiIn == amountOut` in these functions, so the actual value is correct.
## 05: Snake case is not used for `constant`.
### Impact - Non-critical
Constants should be named with all capital letters with underscores separating words. 

https://docs.soliditylang.org/en/latest/style-guide.html
### Proof of Concept
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L75

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L92 (L92 ~ 98)

### Recommended Mitigation Steps
 All capital letters with underscores separating words should be used.
## 06: `DAI.balanceOf(address(this))` is written in multiple places.
### Impact - Non-critical
Codes writing the same content(`DAI.balanceOf(address(this))` in this case) should be unified as much as possible.
### Proof of Concept
```
/// @notice gets the effective balance of "balanceReportedIn" token if the deposit were fully withdrawn
    function balance() external view returns (uint256) {
        return DAI.balanceOf(address(this));
    }
```
and
```
return (DAI.balanceOf(address(this)), FEI.balanceOf(address(this)));
```
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L84
### Recommended Mitigation Steps
Change the `balance()` function to `public` and write it like this.
```
return (balance(), FEI.balanceOf(address(this)));
```
Alternatively, `getDaiBalance()` and `getFeiBalance() `can be created separately and incorporated here.
```
return (getDaiBalance(), getFeiBalance());
```
In this way, the following code can also be modified.
```
uint256 feiBalance = FEI.balanceOf(address(this));
```
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L106

## 07: The same `require` statement is described in multiple places.
### Impact - Non-critical
Codes writing the same content(`require(_cTokens.length == 27, "Must provide exactly 27 merkle roots");` in this case) should be unified as much as possible.

### Proof of Concept
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138

### Recommended Mitigation Steps
It is conceivable to define a `modifier`.
## 08: Lack of NatSpec comment
### Impact - Non-critical
For completeness and readability, NatSpec for all functions within the contract should be documented whenever possible.
### Proof of Concept
In this contract.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol

### Recommended Mitigation Steps
Add NatSpec comment.

## 09: The `event` is not `emit` when the critical value is set.
### Impact - Non-critical
Allow Dapps to detect important changes in storage ,
`event` should be `emit` so that Dapps can detect important changes in storage.  
This is even more necessary since these are `internal` functions, not `private` functions, and can also be called by derived contracts in the future.

### Proof of Concept
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L133

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L143

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L149

### Recommended Mitigation Steps
Define `event` and `emit`.