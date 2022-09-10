## signAndClaim function missing HasNotSign modifier in RariMerkleReemder.sol

we recommand the protocol add HasNotSign modifier to the function
sign and claim.

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L88

```

    function signAndClaim(
        bytes calldata signature,
        address[] calldata cTokens,
        uint256[] calldata amounts,
        bytes32[][] calldata merkleProofs
    ) external override nonReentrant {
        // both sign and claim/multiclaim will revert on invalid signatures/proofs
        _sign(signature);
        _multiClaim(cTokens, amounts, merkleProofs);
    }
```

## Implement unit test using consitent test framework.

Currently the project use both forge and hardhat to implement test, 
we recommand the protocol to use one consistent test framework to
avoid the code maintance workload and integration test workload.

## Natspace comment style can be improved in TribeRedeemer.sol

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol

https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

## USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

the TribeRedeemer.sol, simpleFeiDaiPSM.sol and MultiMerkleRedeemer.sol

use the solidity version

```
pragma solidity ^0.8.4;
```

## Use the same solidty version

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol

MerkleRedeemerDripper use solidty version 

```
pragma solidity =0.8.10;
```

while other contract use

```
pragma solidity ^0.8.4;
```

we recommand the protocol to use the same solidity version.

## USE OF FLOATING PRAGMA

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

https://swcregistry.io/docs/SWC-103

the TribeRedeemer.sol, simpleFeiDaiPSM.sol and MultiMerkleRedeemer.sol

use the solidity version

```
pragma solidity ^0.8.4;
```

## Missing event emit in important state configuration

in RariMerkleReemer.sol

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L133

```
    function _configureExchangeRates(address[] memory _cTokens, uint256[] memory _exchangeRates) internal {
```

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L137

```
 function _configureMerkleRoots(address[] memory _cTokens, bytes32[] memory _roots) internal {
```

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L147

```
    function _configureBaseToken(address _baseToken) internal {
```

is missing event emit.