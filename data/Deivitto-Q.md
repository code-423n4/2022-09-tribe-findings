# QA
# Low

## Missing checks for address(0x0) when assigning values to `redeemedToken`
### Summary
Zero address should be checked for immutable variables. A zero address can lead into problems and redeployments.
### Github Permalinks
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L32

### Mitigation
Check zero address before assigning it



# Informational
##  Use of magic numbers is confusing and risky
### Summary:
Magic numbers are hardcoded numbers used in the code which are ambiguous to their intended purpose. These should be replaced with constants to make code more readable and maintainable.
### Details:
Values are hardcoded and would be more readable and maintainable if declared as a constant

### Github Permalinks
27, 1e10, 1e18 appear hardcoded in
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol


### Mitigation
Replace magic hardcoded numbers with declared constants

## Missing indexed event parameters
### Summary:
Events without indexed event parameters make it harder and
inefficient for off-chain tools to analyze them.
### Details:
Indexed parameters (“topics”) are searchable event parameters.
They are stored separately from unindexed event parameters in an efficient manner to allow for faster access. This is useful for efficient off-chain-analysis, but it is also more costly gas-wise.
 
### Github Permalinks
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/peg/SimpleFeiDaiPSM.sol#L26-L29

### Mitigation
Consider which event parameters could be particularly useful to off-chain tools and should be indexed.

## Naming convention of constants
### Summary:
 Constant naming convention is all upper case. 
### Details:
Some constants are not using proper style.
Constant should be in UPPER_CASE_WITH_UNDERSCORES as per Solidity Style Guide. 
### Github Permalinks
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/peg/SimpleFeiDaiPSM.sol#L75
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98

### Mitigation
 Rename the constant to uppercase style: CONSTANTS_WITH_UNDERSCORES. 




### Mitigation
Consider changing to pragma 0.8.12

## Missing Natspec 
 ### Summary: 
 Missing Natspec and regular comments affect readability and maintainability of a codebase. 
 ### Details: 
 Contracts has partial or full lack of comments
 ### Github Permalinks 
- Natspec @param and @return value
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L47-L254
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L26-L76
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/peg/SimpleFeiDaiPSM.sol#L30-L110
 
 ### mitigation
 Add @param descriptors
 Add @return descriptors


## Bad order of code
### Summary
Clearness of the code is important for the readability and maintainability.
As Solidity guidelines says about declaration order:
1.Type declarations
2.State variables
3.Events
4.Modifiers
5.Functions
Also, state variables order affects to gas in the same way as ordering structs for saving storage slots
### Details
Variables are defined between functions rather than at the top of the contract.
### github permalink
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/peg/SimpleFeiDaiPSM.sol#L33-L98


### Mitigation
Follow solidity style guidelines https://docs.soliditylang.org/en/v0.8.15/style-guide.html




## Different versions of pragma
### Summary
Some of the contracts include an unlocked pragma, e.g., pragma solidity ^0.8.4. 

Locking the pragma helps ensure that contracts are not accidentally deployed using an old compiler version with unfixed bugs.

### Github Permalinks
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/peg/SimpleFeiDaiPSM.sol#L2
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L2
### Mitigation 
Lock pragmas to a specific Solidity version. 
Consider converting ^ 0.8.4 into 0.8.10

## Maximum line length exceeded
### Summary:
 Long lines should be wrapped to conform with Solidity Style guidelines. 
### Details: 
Lines that exceed the 79 (or 99) character length suggested by the Solidity Style guidelines. Reference: https://docs.soliditylang.org/en/v0.8.10/style-guide.html#maximum-line-length
### Github Permalinks: 
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/peg/SimpleFeiDaiPSM.sol#L77

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L19

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L68

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L82

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L83

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L124

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L126

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L152

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L154

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L163

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L175

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L183

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L191

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L200

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L205

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L224

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L225

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L227

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L236

https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L238

### Mitigation
Reduce line length to less than 99 at least to improve maintainability and readability of the code 


## Unused named returns
### Summary
Using both named returns and a return statement isn’t necessary. Removing one of those can improve code clarity 

### Github Permalinks
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81
### Mitigation
Remove return or returns when both used

