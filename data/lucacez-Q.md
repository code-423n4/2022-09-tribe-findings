### MISSING CHECKS FOR `ADDRESS(0X0)` WHEN ASSIGNING VALUES TO `ADDRESS` STATE VARIABLES

Zero-address checks are a best practice for input validation of critical address parameters. While the codebase applies this to most cases, there are many places where this is missing in constructors and setters.

Impact: Accidental use of zero-addresses may result in exceptions, burn fees/tokens, or force redeployment of contracts.

**Findings**

[SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol)
[L34](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L34) - `to` can be address(0)
[L49](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L49) - `to` can be address(0)

[RariMerkleRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol)
[L124](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L124) - `_cTokens` can be address(0)
[L137](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L137) - `_cTokens` can be address(0)
[L166](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L166) - `_cTokens` can be address(0)
[L186](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L186) - `_cTokens` can be address(0)
[L201](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L201) - `cToken` can be address(0)

[TribeRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol)
[L32](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L32) - `_redeemedToken` can be address(0)
[L33](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L33) - `_tokensReceived` can be address(0)
[L64](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L64) - `to` can be address(0)

#### Recommended Mitigation Steps

Add zero-address checks, e.g.:

```
require(`address` != address(0), "Zero-address");
``` 
### THESE ADDRESSES CAN BE REPEATED

The same token addresses can be entered by mistake, it does not revert if this happens.

[RariMerkleRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol)
[L124](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L124) - `_cTokens`
[L137](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L137) - `_cTokens` 
[L186](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L186) - `_cTokens` 

### **USE OF FLOATING PRAGMA**

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

[https://swcregistry.io/docs/SWC-103](https://swcregistry.io/docs/SWC-103)

**Findings**
[SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L2) `pragma solidity ^0.8.4;`
[TribeRedeemer.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L2) `pragma solidity ^0.8.4;`

**Recommended Mitigation Steps**

Lock the pragma version to the same version as used in the other contracts and also consider known bugs ([https://github.com/ethereum/solidity/releases](https://github.com/ethereum/solidity/releases)) for the compiler version that is chosen.

Pragma statements can be allowed to float when a contract is intended for consumption by other developers, as in the case with contracts in a library or EthPM package. Otherwise, the developer would need to manually update the pragma in order to compile it locally.

### **LACK OF SETTER FUNCTION FOR THE `tokensReceived`**

On the `TribeRedeemer` contract, there is not setter function on the `tokensReceived` address. This can cause misfunctionality on the `TribeRedeemer` contract. 

#### Proof of Concept

Navigate to contract: 
`tokensReceived` address is set on the constructor.
Setter function is missing on the contract. Misdeployed contract can cause failure of `tokensReceived` integration.

#### Recommended Mitigation Steps

Consider adding setter function for `tokensReceived`