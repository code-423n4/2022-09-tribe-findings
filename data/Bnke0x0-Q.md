

### [L01] Unbounded loops with external calls

#### Impact
The interface and the function should require a start index and a 
lenght, so that the index composition can be fetched in batches without 
running out of gas. If there are thousands of index components (e.g. 
like the Wilshire 5000 index), the function may revert
#### Findings:
```

2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::137-145 => function _configureMerkleRoots(address[] memory _cTokens, bytes32[] memory _roots) internal {
     require(_cTokens.length == 27, "Must provide exactly 27 merkle roots");
        require(_cTokens.length == _roots.length, "Merkle roots must be provided for each cToken");

        for (uint256 i = 0; i < _cTokens.length; i++) {
            require(_roots[i] != bytes32(0), "Merkle root must be non-zero");
            merkleRoots[_cTokens[i]] = _roots[i];
        }
    }

```






### [L02] Missing checks for `address(0x0)` when assigning values to `address` state variables

#### Impact

#### Findings:
```

2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::133 => cTokenExchangeRates[_cTokens[i]] = _exchangeRates[i];
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::138 => require(_cTokens.length == 27, "Must provide exactly 27 merkle roots");
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::139 => require(_cTokens.length == _roots.length, "Merkle roots must be provided for each cToken");
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::143 => merkleRoots[_cTokens[i]] = _roots[i];
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::149 => baseToken = _baseToken;
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::158 => userSignatures[msg.sender] = _signature;
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::178 => claims[msg.sender][_cToken] = _amount;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::32 => redeemedToken = _redeemedToken;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::33 => tokensReceived = _tokensReceived;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::34 => redeemBase = _redeemBase;
```










### [L03] Unspecific Compiler Version Pragma

#### Impact
Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.
#### Findings:
```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::2 => pragma solidity ^0.8.4;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::2 => pragma solidity ^0.8.4;
```




### Non-Critical Issues



### [N01] Adding a return statement when the function defines a named return variable, is redundant

#### Impact
Issue Information: (https://github.com/Bnke0x0/c4-common-issues/blob/main/2-Low-Risk.md#n001---Adding-a-return-statement-when-the-function-defines-a-named-return-variable,-is-redundant)

#### Findings:
```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::67 => return amountIn;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::39 => return tokensReceived;
```


### [N02] constants should be defined rather than using magic numbers


#### Findings:
```
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::85 => return (cTokenExchangeRates[cToken] * amount) / 1e18;
```




### [N03] Use a more recent version of solidity


#### Findings:
```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::2 => pragma solidity ^0.8.4;
2022-09-tribe/contracts/shutdown/fuse/MerkleRedeemerDripper.sol::2 => pragma solidity =0.8.10;
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::2 => pragma solidity =0.8.10;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::2 => pragma solidity ^0.8.4;
```






### [N04] Use a more recent version of solidity

#### Impact
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions
#### Findings:
```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::16 => using SafeERC20 for Fei;
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::17 => using SafeERC20 for IERC20;
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::16 => using SafeERC20 for IERC20;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::11 => using SafeERC20 for IERC20;
```




### [N05] Event is missing `indexed` fields

#### Impact
Each `event` should use three `indexed` fields if there are three or more fields
#### Findings:
```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::27 => event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::29 => event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::14 => event Redeemed(address indexed owner, address indexed receiver, uint256 amount, uint256 base);
```












###  [N06] `public` functions not called by the contract should be declared `external` instead

#### Impact
Contracts are allowed to override their parents’ functions and change the visibility from external to public.
#### Findings:
```
2022-09-tribe/contracts/shutdown/fuse/MerkleRedeemerDripper.sol::21 => function drip() public override {
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::81 => function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::38 => function tokensReceivedOnRedeem() public view returns (address[] memory) {
```



### [N07] Constant redefined elsewhere

#### Impact
Consider defining in only one contract so that values cannot become out of sync when only one location is updated
#### Findings:
```
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::174 => bytes32 leafHash = keccak256(abi.encodePacked(msg.sender, _amount));
```




### [N08] NC-library/interface files should use fixed compiler versions, not floating ones


#### Findings:
```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::2 => pragma solidity ^0.8.4;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::2 => pragma solidity ^0.8.4;
```



### [N09] File does not contain an SPDX Identifier


#### Findings:
```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::1 => // SPDX-License-Identifier: GPL-3.0-or-later
2022-09-tribe/contracts/shutdown/fuse/MerkleRedeemerDripper.sol::1 => // SPDX-License-Identifier: GPL-3.0-or-later
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::1 => // SPDX-License-Identifier: GPL-3.0-or-later
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::1 => // SPDX-License-Identifier: GPL-3.0-or-later


