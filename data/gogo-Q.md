# 2022-09-TRIBE
## Low Risk and Non-Critical Issues

### Non-library/interface files should use fixed compiler versions, not floating ones


_There are **2** instances of this issue:_

```solidity
File: contracts/peg/SimpleFeiDaiPSM.sol

2:    pragma solidity ^0.8.4;
```
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/SimpleFeiDaiPSM.sol

```solidity
File: contracts/shutdown/redeem/TribeRedeemer.sol

2:    pragma solidity ^0.8.4;
```
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/redeem/TribeRedeemer.sol

### Event is missing `indexed` fields


_There are **3** instances of this issue:_

```solidity
File: contracts/peg/SimpleFeiDaiPSM.sol

event      Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut)

event      Mint(address to, uint256 amountIn, uint256 amountFeiOut)
```
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/SimpleFeiDaiPSM.sol

```solidity
File: contracts/shutdown/redeem/TribeRedeemer.sol

event      Redeemed(address indexed owner, address indexed receiver, uint256 amount, uint256 base)
```
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/redeem/TribeRedeemer.sol

### Not used import


_There are **2** instances of this issue:_

```solidity
File: contracts/shutdown/fuse/RariMerkleRedeemer.sol

4:    import "../../refs/CoreRef.sol";

7:    import "@openzeppelin/contracts/utils/cryptography/SignatureChecker.sol";
```
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol
