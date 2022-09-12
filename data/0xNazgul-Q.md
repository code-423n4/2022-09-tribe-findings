## [NAZ-L1] Missing `hasNotSigned` Modifier
**Severity**: Low
**Context**: [`RariMerkleRedeemer.sol#L93`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L93)

**Description**:
The `signAndClaim()` function is missing the `hasNotSigned` modifier. All other functions that use the internal function `_sign()` have the `hasNotSigned` modifier to check if the user has already signed.

**Recommendation**:
Consider adding the `hasNotSigned` modifier to the function `signAndClaim()`.


## [NAZ-L2] Missing Zero-address Validation
**Severity**: Low
**Context**: [`SimpleFeiDaiPSM.sol#L34`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L34), [`SimpleFeiDaiPSM.sol#L49`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L49), [`TribeRedeemer.sol#L32-L33`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L32-L33)

**Description**:
Lack of zero-address validation on address parameters may lead to transaction reverts, waste gas, require resubmission of transactions and may even force contract redeployments in certain cases within the protocol.

**Recommendation**:
Consider adding explicit zero-address validation on input parameters of address type.


## [NAZ-N1] Function States Named Return But Doesn't Use It
**Severity** Informational
**Context**: [`RariMerkleRedeemer.sol#L81`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81)

**Description**:
The linked function(s) have a named return that is not used. Using both named returns and a return statement isn't necessary. 


**Recommendation**:
Removing one of those can improve code clarity.

## [NAZ-N2] Incorrect Amounts Emitted
**Severity** Informational
**Context**: [`SimpleFeiDaiPSM.sol#L42`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L42), [`SimpleFeiDaiPSM.sol#L57`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L57)

**Description**:
Both `Mint() && Redeem()` events in `mint() && redeem()` respectively are emitted when a user mints `FEI` or redeems it for `DAI`. In the `mint()` function its parameters `address to and uint256 amountIn` are correctly emitted. It also has a return parameter of `uint256 amountFeiOut` which indicates the intended FEI amount to be sent to the `address to`, is used in `FEI.mint()` and is meant to be emitted in the event `Mint()`. However, in the function `mint()` it emits the `amountIn` instead of the `amountFeiOut` for the third event argument. This can also be seen being done with the function `redeem()`, where the event is emitting `amountFeiIn` instead of `amountAssetOut`. Although they are set equal to each other at the start of each function it would help readability in the future.

**Recommendation**:
Change emit `emit Mint(to, amountIn, amountIn); && emit Redeem(to, amountFeiIn, amountFeiIn);` To `emit Mint(to, amountIn, amountFeiOut); && emit Redeem(to, amountFeiIn, amountOut);`. 


## [NAZ-N3] Line Length
**Severity**: Informational
**Context**: [`RariMerkleRedeemer.sol#L163`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L163), [`RariMerkleRedeemer.sol#L183`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L183), [`RariMerkleRedeemer.sol#L200`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L200), [`RariMerkleRedeemer.sol#L205`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L205)

**Description**:
Max line length must be no more than 120 but many lines are extended past this length.

**Recommendation**:
Consider cutting down the line length below 120.


## [NAZ-N4] Code Structure Deviates From Best-Practice
**Severity**: Informational
**Context**: [`SimpleFeiDaiPSM.sol#L75`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L75), [`RariMerkleRedeemer.sol#L88`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L88)

**Description**:
The best-practice layout for a contract should follow the following order: state variables, events, modifiers, constructor and functions. Function ordering helps readers identify which functions they can call and find constructor and fallback functions easier.  Functions should be grouped according to their visibility and ordered as: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Some constructs deviate from this recommended best-practice: Modifiers are in the middle of contracts. External/public functions are mixed with internal/private ones. Few events are declared in contracts while most others are in corresponding interfaces.

**Recommendation**:
Consider adopting recommended best-practice for code structure and layout.


## [NAZ-N5] Function && Variable Naming Convention
**Severity** Informational
**Context**: [`SimpleFeiDaiPSM.sol#L19-L20`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L19-L20), [`SimpleFeiDaiPSM.sol#L75`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L75), [`SimpleFeiDaiPSM.sol#L92-L98`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98), [`TribeRedeemer.sol#L20`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L20) 

**Description**:
The linked variables do not conform to the standard naming convention of Solidity whereby functions and variable names(local and state) utilize the `mixedCase` format unless variables are declared as `constant` in which case they utilize the `UPPER_CASE_WITH_UNDERSCORES` format. Private variables and functions should lead with an `underscore`.

**Recommendation**:
Consider naming conventions utilized by the linked statements are adjusted to reflect the correct type of declaration according to the [Solidity style guide](https://docs.soliditylang.org/en/latest/style-guide.html). 


## [NAZ-N6] Unindexed Event Parameters
**Severity** Informational
**Context**: [`SimpleFeiDaiPSM.sol#L27-L29`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L27-L29)

**Description**:
Parameters of certain events are expected to be indexed so that they’re included in the block’s bloom filter for faster access. Failure to do so might confuse off-chain tooling looking for such indexed events.

**Recommendation**:
Consider adding the indexed keyword to event parameters that should include it.


## [NAZ-N7] Floating Pragma
**Severity**: Informational
**Context**: [`SimpleFeiDaiPSM.sol`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol), [`TribeRedeemer.sol`](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol)

**Description**:
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

**Recommendation**: 
Consider locking the pragma version.


## [NAZ-N8] Missing or Incomplete NatSpec
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-09-tribe)

**Description**:
Some functions are missing @notice/@dev NatSpec comments for the function, @param for all/some of their parameters and @return for return values. Given that NatSpec is an important part of code documentation, this affects code comprehension, auditability and usability.

**Recommendation**:
Consider adding in full NatSpec comments for all functions to have complete code documentation for future use.


## [NAZ-N9] Multiple Solidity Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-09-tribe)

**Description**:
It is better to use one Solidity compiler version across all contracts instead of different versions with different bugs and security checks. 

**Recommendation**: 
Consider ensuring all pragma versions are the same one.


## [NAZ-N10] Older Version Pragma
**Severity**: Informational
**Context**: [`All Contracts`](https://github.com/code-423n4/2022-09-tribe)

**Description**:
Using very old versions of Solidity prevents benefits of bug fixes and newer security checks. Using the latest versions might make contracts susceptible to undiscovered compiler bugs. 

**Recommendation**:
Consider using the most recent version.