## Table of Contents
### Low Risk Issues
- Missing Zero Address Check 

### Non-critical Issues
- Wrong Error Message
- Unnecessary Use of Named Returns
- Constant Naming Convention
- Define Magic Numbers to Constant
- Event is Missing Indexed Fields
- Use fixed compiler versions instead of floating version

## Low Risk Issues
### Missing Zero Address Check 

#### Issue
I recommend adding check of 0-address for input validation of critical address parameters.
Not doing so might lead to non-functional contract and have to redeploy the contract, when it is updated to 0-address accidentally.

#### PoC
Total of 4 instances found.
1. cTokens addresses of constructor() of RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L41-42
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L124-L135
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L137-L145

2. _core address of constructor() of MerkleRedeemerDripper.sol
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L16
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/refs/CoreRef.sol#L19

3. redeemedToken address of constructor() of TribeRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L32

4. tokensReceived addresses of constructor() of TribeRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L33

#### Mitigation
Add 0-address check for above addresses.

&ensp;
## Non-critical Issues
### Wrong Error Message

#### Issue
There is a wrong error message in the following.

#### PoC
1. _configureExchangeRates() of RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125
```solidity
        require(_cTokens.length == 27, "Must provide exactly 27 exchange rates.");
```
Error message should be
```solidity
        require(_cTokens.length == 27, "Must provide exactly 27 cToken");
```

2. _configureMerkleRoots() of RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138
```solidity
        require(_cTokens.length == 27, "Must provide exactly 27 merkle roots");
```
Error message should be
```solidity
        require(_cTokens.length == 27, "Must provide exactly 27 cToken");
```
#### Mitigation
Change to correct error message as mentioned in above PoC

&ensp;
### Unnecessary Use of Named Returns

#### Issue
Several function adds return statement even thought named returns variable are used.
Remove unnecessary named returns variable to improve code readability.
Also keeping the use of named returns or return statement consistent through out the whole project 
if possible is recommended.

#### PoC
Total of 1 instance found.
1. previewRedeem() of RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L85

#### Mitigation
Remove unused named returns variable and keep the use of named returns or return statement consistent
through out the whole project if possible.

&ensp;
### Constant Naming Convention

#### Issue
Constants should be named with all capital letters with underscores separating words.
Reference: https://docs.soliditylang.org/en/v0.8.15/style-guide.html?highlight=naming#constants

#### PoC
Total of 8 instances found.
```solidity
./SimpleFeiDaiPSM.sol:75:    address public constant balanceReportedIn = address(DAI);
./SimpleFeiDaiPSM.sol:92:    uint256 public constant mintFeeBasisPoints = 0;
./SimpleFeiDaiPSM.sol:93:    uint256 public constant redeemFeeBasisPoints = 0;
./SimpleFeiDaiPSM.sol:94:    address public constant underlyingToken = address(DAI);
./SimpleFeiDaiPSM.sol:95:    uint256 public constant getMaxMintAmountOut = type(uint256).max;
./SimpleFeiDaiPSM.sol:96:    bool public constant paused = false;
./SimpleFeiDaiPSM.sol:97:    bool public constant redeemPaused = false;
./SimpleFeiDaiPSM.sol:98:    bool public constant mintPaused = false;
```

#### Mitigation
Name the constants with all capital letters with underscores separating words.

&ensp;
### Define Magic Numbers to Constant

#### Issue
It is best practice to define magic numbers to constant rather than just using as a magic number.
This improves code readability and maintainability. 

#### PoC
1. 1e10
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L130
```solidity
./RariMerkleRedeemer.sol:130:                _exchangeRates[i] > 1e10,
```

2. 1e18
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L85
```solidity
./RariMerkleRedeemer.sol:85:        return (cTokenExchangeRates[cToken] * amount) / 1e18;
```

#### Mitigation
Define magic numbers to constant.

&ensp;
### Event is Missing Indexed Fields

#### Issue
Each event should have 3 indexed fields if there are 3 or more fields.

#### PoC
Total of 3 instances found.
```solidity
./SimpleFeiDaiPSM.sol:27:    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
./SimpleFeiDaiPSM.sol:29:    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
./TribeRedeemer.sol:14:    event Redeemed(address indexed owner, address indexed receiver, uint256 amount, uint256 base);
```

#### Mitigation
Use all 3 index fields when possible.

&ensp;
### Use fixed compiler versions instead of floating version

#### Issue
It is best practice to lock your pragma instead of using floating pragma.
The use of floating pragma has a risk of accidentally get deployed using latest complier
which may have higher risk of undiscovered bugs.
Reference: https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/locking-pragmas/

#### PoC
Total of 2 instances found.
```solidity
./SimpleFeiDaiPSM.sol:2:pragma solidity ^0.8.4;
./TribeRedeemer.sol:2:pragma solidity ^0.8.4;
```

#### Mitigation
I suggest to lock your pragma and aviod using floating pragma.
```
// bad
pragma solidity ^0.8.10;

// good
pragma solidity 0.8.10;
```

&ensp;