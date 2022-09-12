# Summary
## Low
[L01] A floating pragma is set.
[L02] Outdated compiler version
[L03] Missing checks for address(0x0) when assigning values to address state variables

## Non Critical
[NC01] Constants should be defined rather than using magic numbers
[NC02] Event is missing indexed fields
[NC03] Duplicated require()/revert() checks should be refactored to a modifier or function

# Low
## [L01] A floating pragma is set.
### Description
Some contracts have the pragma solidity directive ^0.8.4. It is recommended to specify a fixed compiler version to ensure that the bytecode produced 
does not vary between builds. 
This is especially important if you rely on bytecode-level verification of the code.

### Mitigation
Lock the pragma.

### Lines in the code
[TribeRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L2)
[SimpleFeiDaiPSM.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L2)

## [L02] Outdated compiler version
It's a best practice to use the latest compiler version.
The specified minimum compiler version is quite old. Older compilers might be susceptible to some bugs. 
It's recommended changing the solidity version pragma to the latest version to enforce the use of an up-to-date compiler.

A list of known compiler bugs and their severity can be found here: https://etherscan.io/solcbuginfo

To check the bugfixed and improvements of latest versions see the following [link](https://github.com/ethereum/solidity/releases)

### Mitigation
Update the pragma to 0.8.16

### Lines in the code
[RariMerkleRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2)
[MerkleRedeemerDripper.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2)
[TribeRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L2)
[SimpleFeiDaiPSM.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L2)

## [L03] Missing checks for address(0x0) when assigning values to address state variables

### Lines in the code
[TribeRedeemer.sol#L32-L33](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L32-L33)

# Non Critical
## [NC01] Constants should be defined rather than using magic numbers
Even assembly can benefit from using readable constants instead of hex/numeric literals

### Lines in the code
[RariMerkleRedeemer.sol#L125](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125)
[RariMerkleRedeemer.sol#L138](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138)


## [NC02] Event is missing indexed fields
### Description
Index event fields make the field more quickly accessible to off-chain tools that parse events. 
However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). 
Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. 
If there are fewer than three fields, all of the fields should be indexed.

### Lines in the code
[TribeRedeemer.sol#L14](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L14)
[SimpleFeiDaiPSM.sol#L27](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L27)
[SimpleFeiDaiPSM.sol#L29](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L29)

## [NC03] Duplicated require()/revert() checks should be refactored to a modifier or function
### Description
The compiler will inline the function, which will avoid JUMP instructions usually associated with functions

### Lines in the code
#### require(_cTokens.length == 27, "Must provide exactly 27 exchange rates.");
[RariMerkleRedeemer.sol#L125](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125)
[RariMerkleRedeemer.sol#L138](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138)

#### require(_cTokens.length == _exchangeRates.length, "Exchange rates must be provided for each cToken");
[RariMerkleRedeemer.sol#L126](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L126)
[RariMerkleRedeemer.sol#L139](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L139)
