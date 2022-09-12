##  1 -Inconsistent Solidity Versions 


**Description:**
Different Solidity compiler versions are used throughout Src repositories. The following 
contracts mix versions: 
pragma ^0.8.4   (contracts/peg/SimpleFeiDaiPSM.sol)
pragma =0.8.10 (contracts/shutdown/fuse/RariMerkleRedeemer.sol)
pragma =0.8.10 (contracts/shutdown/fuse/MerkleRedeemerDripper.sol)
pragma ^0.8.4   (contracts/shutdown/redeem/TribeRedeemer.sol)


**Recommendation:**
Versions must be consistent with each other


**Full List:**

```js
contracts/peg/SimpleFeiDaiPSM.sol:
pragma solidity ^0.8.4;
 
contracts/shutdown/fuse/RariMerkleRedeemer.sol :
pragma solidity =0.8.10;
 
contracts/shutdown/fuse/MerkleRedeemerDripper.sol :
pragma solidity =0.8.10;
 
contracts/shutdown/redeem/TribeRedeemer.sol :
pragma solidity ^0.8.4;
```


## 2 - Add to indexed parameter for countable Events

**Context:**
[SimpleFeiDaiPSM.sol#L27-L29](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L27-L29)


**Recommendation:**
Add to indexed parameter for countable Events


## 3 -Floating Pragma


**Description:**
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.
https://swcregistry.io/docs/SWC-103

Floating Pragma List: 
pragma ^0.8.4.   (contracts/peg/SimpleFeiDaiPSM.sol)
		(contracts/shutdown/redeem/TribeRedeemer.sol)


**Recommendation:**
Lock the pragma version and also consider known bugs (https://github.com/ethereum/solidity/releases) for the compiler version that is chosen.


## 4 - For modern and more readable code; Update import usages

**Proof of Concept**

```js
RariMerkleRedeemer.sol
import "../../refs/CoreRef.sol";
import "./MultiMerkleRedeemer.sol";

SimpleFeiDaiPSM.sol
import "../fei/Fei.sol";

```


**Recommendation:**
Style should be like this
```import {contract1 , contract2} from "filename.sol";```


##  5 - Use a More Recent Version of Solidity


**Description:**
Old version of Solidity is used (```^0.8.4```), newer version should be used

pragma ^0.8.4   (contracts/peg/SimpleFeiDaiPSM.sol)
pragma ^0.8.4   (contracts/shutdown/redeem/TribeRedeemer.sol)


## 6-Function writing that does not comply with the 'Solidity Style Guide’

**Context:**
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol


**Description:**
Order of Functions ; Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this
https://docs.soliditylang.org/en/v0.8.15/style-guide.html

Functions should be grouped according to their visibility and ordered:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private
Within a grouping, place the view and pure functions last.


## 7-   ```0``` address Check

**Context:**
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L10-L16
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L36-L37
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L90
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L166


**Description:**
Also check of the address to protect the code from 0x0 address  problem just in case.This is best practice
Or instead of suggesting that they verify address != 0x0 , you could add some good natspec comments explaining what is valid and what is invalid and what are the implications of accidentally using an invalid address

**Recommendation:**
like this ;
``` if (_treasury == address(0)) revert ADDRESS_ZERO();```


## 8- Missing NatSpec comment

**Context:**
(contracts/peg/SimpleFeiDaiPSM.sol)
(contracts/shutdown/fuse/RariMerkleRedeemer.sol)
(contracts/shutdown/fuse/MerkleRedeemerDripper.sol)
(contracts/shutdown/redeem/TribeRedeemer.sol)


**Description:**
Solidity contracts can use a special form of comments to provide rich documentation for functions, return variables and more. This special form is named the Ethereum Natural Language Specification Format (NatSpec).
https://docs.soliditylang.org/en/v0.8.15/natspec-format.html

NatSpec comments are very few on all contracts of the audited project,

**Recommendation:**
NatSpec comments should be added as below

```js

/// @title A simulator for trees
/// @author Larry A. Gardner
/// @notice You can use this contract for only the most basic simulation
/// @dev All function calls are currently implemented without side effects
/// @custom:experimental This is an experimental contract.
contract Tree {
    /// @notice Calculate tree age in years, rounded up, for live trees
    /// @dev The Alexandr N. Tetearing algorithm could increase precision
    /// @param rings The number of rings from dendrochronological sample
    /// @return Age in years, rounded up for partial years
    function age(uint256 rings) external virtual pure returns (uint256) {
        return rings + 1;
    }

```


## 9- Missing Event for Critical Parameters Change

**Context:**
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L68
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L72
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L105


**Description:**
Events help non-contract tools to track changes, and events prevent users from being surprised by changes

**Recommendation:**
Add Event-Emit


## 10 - Long lines are not suitable for the Solidity Style Guide

**Context:**
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L19
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L124
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L225


**Description:**
Usually lines in source code are limited to 80 characters. Today’s screens are much larger so it’s reasonable to stretch this in some cases. Since the files will most likely reside in GitHub, and GitHub starts using a scroll bar in all cases when the length is over 164 characters, the lines below should be split when they reach that length

(why-is-80-characters-the-standard-limit-for-code-width)[https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width]



**Recommendation:**
It is recommended to write proper test for all possible code flows and specially edge cases


## 11 – Use abi.encode() to avoid collision 

**Context:**
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L174

**Description:**
If you use keccak256(abi.encodePacked(a, b)) and both a and b are dynamic types, it is easy to craft collisions in the hash value by moving parts of a into b and vice-versa. More specifically, abi.encodePacked("a", "bc") == abi.encodePacked("ab", "c"). If you use abi.encodePacked for signatures, authentication or data integrity, make sure to always use the same types and check that at most one of them is dynamic. Unless there is a compelling reason, abi.encode should be preferred.


When are these used?

```abi.encode:```
abi.encode encode its parameters using the ABI specs. The ABI was designed to make calls to contracts.
Parameters are padded to 32 bytes. If you are making calls to a contract you likely have to use abi.encode
If you are dealing with more than one dynamic data type as it prevents collision.

```abi.encodePacked:```
abi.encodePacked encode its parameters using the minimal space required by the type. Encoding an uint8 it will use 1 byte. It is used when you want to save some space, and not calling a contract.Takes all types of data and any amount of input.


**Proof of Concept**

```js

contract Contract0 {

    // abi.encode
    // (AAA,BBB) keccak = 0xd6da8b03238087e6936e7b951b58424ff2783accb08af423b2b9a71cb02bd18b
    // (AA,ABBB) keccak = 0x54bc5894818c61a0ab427d51905bc936ae11a8111a8b373e53c8a34720957abb
    function collision(string memory _text, string memory _anotherText)
        public pure returns (bytes32)
    {
        return keccak256(abi.encode(_text, _anotherText));
    }
}



contract Contract1 {

    // abi.encodePacked
    // (AAA,BBB) keccak = 0xf6568e65381c5fc6a447ddf0dcb848248282b798b2121d9944a87599b7921358
    // (AA,ABBB) keccak = 0xf6568e65381c5fc6a447ddf0dcb848248282b798b2121d9944a87599b7921358

    function hard(string memory _text, string memory _anotherText)
        public pure returns (bytes32)
    {
        return keccak256(abi.encodePacked(_text, _anotherText));
    }
}

```



