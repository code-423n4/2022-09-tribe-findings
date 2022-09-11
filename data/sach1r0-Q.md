# Lack of zero-address check in the constructor

## Details
 Lack of zero-address checks may lead to infunctional protocol especially in the case wherein variable is immutable like the  `redeemedToken` .
 
## Mitigation
Consider adding zero-address check such as: `require(_redeemedToken != address(0));`

## Line of code
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L27-L35
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L35-L44
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L10-L16

___
# Non-Library/Interface files should use fixed compiler versions, not floating ones

## Details
Locking the pragma helps to ensure that contracts do not accidentally get deployed using an outdated compiler version that might introduce bugs that affect the contract system negatively.
see reference: https://github.com/code-423n4/2021-11-unlock-findings/issues/15, https://code4rena.com/reports/2022-03-paladin/ and https://swcregistry.io/docs/SWC-103

## Mitigation
I suggest removing ^ in `pragma solidity ^0.8.4` and change it to `pragma solidity 0.8.10` to be consistent with the rest of the contracts.

## Line of code
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L2
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L2
___
# Lack of indexed parameters in events

## Details
Some of the events in the codebase are not indexed. Indexing event parameters enable off-chain services to search and filter for specific events.
see reference: Low severity finding from OpenZeppelin Audit of HoldeFi
[L09] Lack of indexed parameters in events
https://blog.openzeppelin.com/holdefi-audit/#low

## Mitigation
Add the `indexed` keyword to the events.

## Line of code
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L27
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L29

___
# Some functions does not check for zero address

## Details
It's important to check for zero-address to avoid redeploying of the contract when the address is accidentally set to zero-address. And in the case of the mint function, minted tokens might be lost if `to` parameter is accidentaly set to `address(0)`.

## Mitigation
Add a require statement, for example: `require(to != address(0));`

## Line of code
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L33-L43
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L48-L68
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L64-L76