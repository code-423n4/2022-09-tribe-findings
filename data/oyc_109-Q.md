## [L-01] cToken assumed to have 18 decimals

previewRedeem calculates the amount by dividing by 18 decimal places, the amount will be incorrect if cToken had different decimals amount

```
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol
85: return (cTokenExchangeRates[cToken] * amount) / 1e18;
```

## [L-02] Unspecific Compiler Version Pragma

Avoid floating pragmas for non-library contracts.

While floating pragmas make sense for libraries to allow them to be included with multiple different versions of applications, it may be a security risk for application implementations.

A known vulnerable compiler version may accidentally be selected or security tools might fall-back to an older compiler version ending up checking a different EVM compilation that is ultimately deployed on the blockchain.

It is recommended to pin to a concrete compiler version.

```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::2 => pragma solidity ^0.8.4;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::2 => pragma solidity ^0.8.4;
```

## [L-03] abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()

Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). Unless there is a compelling reason, abi.encode should be preferred. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.

```
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::174 => bytes32 leafHash = keccak256(abi.encodePacked(msg.sender, _amount));
```

## [L-04] Missing Zero address checks

Zero-address checks are a best practice for input validation of critical address parameters. While the codebase applies this to most cases, there are many places where this is missing in constructors and setters.
Impact: Accidental use of zero-addresses may result in exceptions, burn fees/tokens, or force redeployment of contracts.

```
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::32 => redeemedToken = _redeemedToken;
```

## [N-01] Use a more recent version of solidity

Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::2 => pragma solidity ^0.8.4;
2022-09-tribe/contracts/shutdown/fuse/MerkleRedeemerDripper.sol::2 => pragma solidity =0.8.10;
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::2 => pragma solidity =0.8.10;
2022-09-tribe/contracts/shutdown/redeem/TribeRedeemer.sol::2 => pragma solidity ^0.8.4;
```

## [N-02] Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

```
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::27 => event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
2022-09-tribe/contracts/peg/SimpleFeiDaiPSM.sol::29 => event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```

## [N-03] Adding a return statement when the function defines a named return variable, is redundant

It is not necessary to have both a named return and a return statement.

```
2022-09-tribe/contracts/shutdown/fuse/RariMerkleRedeemer.sol::81 => function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
```
