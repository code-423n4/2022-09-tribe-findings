# QA Report

## Summary

|               | Issue         | Instances     |
| :-------------: |:-------------|:-------------:|
| 1      | Immutable addresses lack zero address `address(0)` checks | 1|
| 2      | Missing `hasNotSigned` modifier check in the `signAndClaim` function  |  1 |
| 3      | Setters should check the input value and revert if it's the zero address or zero  |  1 |
| 4      | Related data should be grouped in a `struct` |  1 |


## Findings

### 1- Missing zero address `address(0)` checks  :

Constructors should check the address written in an immutable address variable is not the zero address

#### Impact - Low Risk

#### Proof of Concept

Instances include:

File: contracts/shutdown/redeem/TribeRedeemer.sol 

[address public immutable redeemedToken;](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L17)

#### Mitigation
Add non-zero address checks in the constructors for the instances aforementioned.

### 2- Missing `hasNotSigned` modifier check in the `signAndClaim` function :

The function `signAndClaim` should have a `hasNotSigned` modifier check just like the `signAndClaimAndRedeem` function to prevent already signed users from signing again.

#### Impact - Low Risk

#### Proof of Concept

Instances include:

File: contracts/shutdown/fuse/RariMerkleRedeemer.sol 

[function signAndClaim(bytes calldata signature, address[] calldata cTokens, uint256[] calldata amounts,
bytes32[][] calldata merkleProofs) external override nonReentrant](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L88-L97)

#### Mitigation

Add the `hasNotSigned` modifier check in the function `signAndClaim` as follow :

```
    function signAndClaim(
        bytes calldata signature,
        address[] calldata cTokens,
        uint256[] calldata amounts,
        bytes32[][] calldata merkleProofs
    ) external override hasNotSigned nonReentrant
```

### 3- Adding a `return` statement when the function defines a named return variable is redundant:

#### Impact - NON CRITICAL

#### Proof of Concept

Instances include:

File: contracts/shutdown/fuse/RariMerkleRedeemer.sol

[function previewRedeem(address cToken, uint256 amount)](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81-L86)

#### Mitigation

The named return varibale should either be removed or used instead of the final return statement as follow :

```
function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
        // Each ctoken exchange rate is stored as how much you should get for 1e18 of the particular cToken
        // Thus, we divide by 1e18 when returning the amount that a person should get when they provide
        // the amount of cTokens they're turning into the contract
        baseTokenAmount = (cTokenExchangeRates[cToken] * amount) / 1e18;
    }
```

### 4- Related data should be grouped in a struct :

When there are mappings that use the same key value, having separate fields is error prone, for instance in case of deletion or with future new fields.

#### Impact - NON CRITICAL

#### Proof of Concept

Instances include:

File: contracts/shutdown/fuse/MultiMerkleRedeemer.sol

```
42      mapping(address => bytes) public userSignatures;
46      mapping(address => mapping(address => uint256)) public redemptions;
50      mapping(address => mapping(address => uint256)) public claims;
```
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L42-L50

#### Mitigation

Those mappings should be refactored into the following `struct` and `mapping` for example :

```
struct UserData {
        bytes userSignature;
        mapping(address => uint256) redemptions;
        mapping(address => uint256) claims;
    }
    
mapping(address => UserData) public usersData;
```