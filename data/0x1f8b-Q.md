- [Low](#low)
    - [**1. Packages with vulnerabilities**](#1-packages-with-vulnerabilities)
    - [**2. Mixing and Outdated compiler**](#2-mixing-and-outdated-compiler)
    - [**3. Denial of service during redeem**](#3-denial-of-service-during-redeem)
    - [**4. Unsafe math**](#4-unsafe-math)
- [Non critical](#non-critical)
    - [**5. Use same data sources**](#5-use-same-data-sources)
    - [**6. Improve previewRedeem logic**](#6-improve-previewredeem-logic)
    - [**7. Use hardcoded values**](#7-use-hardcoded-values)
    - [**8. Use the right value**](#8-use-the-right-value)
    - [**9. Unify coding style**](#9-unify-coding-style)

# Low

## **1. Packages with vulnerabilities**

The project contains packages that urgently need to be updated because they contain important vulnerabilities.

**Proof of concept:**

To get the complete list of issues you have to run:

```
> npm audit
```

**Summary:**

```
83 vulnerabilities (2 low, 26 moderate, 52 high, 3 critical)
```

**npm audit report:**

```
@openzeppelin/contracts-upgradeable  <=4.7.2
Severity: high
OpenZeppelin Contracts's SignatureChecker may revert on invalid EIP-1271 signers - https://github.com/advisories/GHSA-4g63-c64m-25w9
OpenZeppelin Contracts's ERC165Checker may revert instead of returning false - https://github.com/advisories/GHSA-qh9x-gcfh-pcrw
OpenZeppelin Contracts ERC165Checker unbounded gas consumption - https://github.com/advisories/GHSA-7grf-83vw-6f5x
OpenZeppelin Contracts's GovernorVotesQuorumFraction updates to quorum may affect past defeated proposals - https://github.com/advisories/GHSA-xrc4-737v-9q75
OpenZeppelin Contracts vulnerable to ECDSA signature malleability - https://github.com/advisories/GHSA-4h98-2769-gh6h
fix available via `npm audit fix`
```

```
ajv  <6.12.3
Severity: moderate
Prototype Pollution in Ajv - https://github.com/advisories/GHSA-v88g-cgmw-v5xw
fix available via `npm audit fix --force`
```

```
async  2.0.0 - 2.6.3
Severity: high
Prototype Pollution in async - https://github.com/advisories/GHSA-fwr7-v2mv-hh25
No fix available
```

```
braces  <=2.3.0
Regular Expression Denial of Service in braces - https://github.com/advisories/GHSA-g95f-p29q-9xw4
Regular Expression Denial of Service (ReDoS) in braces - https://github.com/advisories/GHSA-cwfw-4gq5-mrqx
fix available via `npm audit fix --force`
```

```
cross-fetch  <=2.2.5 || 3.0.0 - 3.1.4 || >=3.2.0-alpha.0
Severity: high
Incorrect Authorization in cross-fetch - https://github.com/advisories/GHSA-7gc6-qh9x-w6h8
Depends on vulnerable versions of node-fetch
fix available via `npm audit fix`
```

```
diff  <3.5.0
Severity: high
Regular Expression Denial of Service (ReDoS) - https://github.com/advisories/GHSA-h6ch-v84p-w6p9
fix available via `npm audit fix --force`
```

```
elliptic  <6.5.4
Severity: moderate
Use of a Broken or Risky Cryptographic Algorithm - https://github.com/advisories/GHSA-r9p9-mrjm-926w
fix available via `npm audit fix`
```

```
glob-parent  <5.1.2
Severity: high
glob-parent before 5.1.2 vulnerable to Regular Expression Denial of Service in enclosure regex - https://github.com/advisories/GHSA-ww39-953v-wcq6
fix available via `npm audit fix --force`
```

```
got  <11.8.5
Severity: moderate
Got allows a redirect to a UNIX socket - https://github.com/advisories/GHSA-pfrx-2q88-qq97
fix available via `npm audit fix --force`
```

```
highlight.js  9.0.0 - 10.4.0
Severity: moderate
ReDOS vulnerabities: multiple grammars - https://github.com/advisories/GHSA-7wwv-vh3v-89cq
fix available via `npm audit fix`
```

```
json-schema  <0.4.0
Severity: critical
json-schema is vulnerable to Prototype Pollution - https://github.com/advisories/GHSA-896r-f27r-55mw
fix available via `npm audit fix`
```

```
lodash  <=4.17.20
Severity: high
Command Injection in lodash - https://github.com/advisories/GHSA-35jh-r3h4-6jhm
Regular Expression Denial of Service (ReDoS) in lodash - https://github.com/advisories/GHSA-29mw-wpgm-hmr9
fix available via `npm audit fix`
```

```
mem  <4.0.0
Severity: moderate
Denial of Service in mem - https://github.com/advisories/GHSA-4xcv-9jjx-gfj3
No fix available
```

```
minimist  <=1.2.5
Severity: critical
Prototype Pollution in minimist - https://github.com/advisories/GHSA-xvch-5gv4-984h
Prototype Pollution in minimist - https://github.com/advisories/GHSA-vh95-rmgr-6w4m
fix available via `npm audit fix --force`
```

```
node-fetch  <=2.6.6
Severity: high
node-fetch is vulnerable to Exposure of Sensitive Information to an Unauthorized Actor - https://github.com/advisories/GHSA-r683-j2x4-v87g
The `size` option isn't honored after following a redirect in node-fetch - https://github.com/advisories/GHSA-w7rc-rwvf-8q5r
No fix available
```

```
normalize-url  4.3.0 - 4.5.0
Severity: high
ReDoS in normalize-url - https://github.com/advisories/GHSA-px4h-xg32-q955
fix available via `npm audit fix`
```

```
path-parse  <1.0.7
Severity: moderate
Regular Expression Denial of Service in path-parse - https://github.com/advisories/GHSA-hj48-42vr-x3v9
fix available via `npm audit fix`
```

```
simple-get  <2.8.2
Severity: high
Exposure of Sensitive Information in simple-get - https://github.com/advisories/GHSA-wpg7-2c88-r8xv
fix available via `npm audit fix`
```

```
tar  <=4.4.17
Severity: high
Arbitrary File Creation/Overwrite on Windows via insufficient relative path sanitization - https://github.com/advisories/GHSA-5955-9wpr-37jh
Arbitrary File Creation/Overwrite via insufficient symlink protection due to directory cache poisoning using symbolic links - https://github.com/advisories/GHSA-qq89-hq3f-393p
Arbitrary File Creation/Overwrite via insufficient symlink protection due to directory cache poisoning using symbolic links - https://github.com/advisories/GHSA-9r2w-394v-53qc
Arbitrary File Creation/Overwrite due to insufficient absolute path sanitization - https://github.com/advisories/GHSA-3jfq-g458-7qm9
Arbitrary File Creation/Overwrite via insufficient symlink protection due to directory cache poisoning - https://github.com/advisories/GHSA-r628-mhmh-qjhw
fix available via `npm audit fix`
```

```
underscore  1.3.2 - 1.12.0
Severity: high
Arbitrary Code Execution in underscore - https://github.com/advisories/GHSA-cf4h-3jhx-xvhq
fix available via `npm audit fix --force`
```

```
undici  <=5.8.1
Severity: high
undici before v5.8.0 vulnerable to CRLF injection in request headers - https://github.com/advisories/GHSA-3cvr-822r-rqcc
ProxyAgent vulnerable to MITM - https://github.com/advisories/GHSA-pgw7-wx7w-2w33
`undici.request` vulnerable to SSRF using absolute URL on `pathname` - https://github.com/advisories/GHSA-8qr4-xgw6-wmr3
Nodejs ‘undici’ Vulnerable to CRLF Injection via Content-Type - https://github.com/advisories/GHSA-f772-66g8-q5h3
undici before v5.8.0 vulnerable to uncleared cookies on cross-host / cross-origin redirect - https://github.com/advisories/GHSA-q768-x9m6-m9qp
fix available via `npm audit fix`
```

```
ws  5.0.0 - 5.2.2
Severity: moderate
ReDoS in Sec-Websocket-Protocol header - https://github.com/advisories/GHSA-6fc8-4gx4-v693
fix available via `npm audit fix`
```

```
yargs-parser  <=5.0.0 || 6.0.0 - 13.1.1
Severity: moderate
yargs-parser Vulnerable to Prototype Pollution - https://github.com/advisories/GHSA-p9pc-299p-vxgp
yargs-parser Vulnerable to Prototype Pollution - https://github.com/advisories/GHSA-p9pc-299p-vxgp
No fix available
```

**Recommended changes:**

```
To address issues that do not require attention, run:
  npm audit fix

To address all issues possible (including breaking changes), run:
  npm audit fix --force

Some issues need review, and may require choosing
a different dependency.
```

## **2. Mixing and Outdated compiler**

The pragma version used are:

```
pragma solidity =0.8.10;
pragma solidity ^0.8.4;
```

*Note that mixing pragma is not recommended. Because different compiler versions have different meanings and behaviors, it also significantly raises maintenance costs. As a result, depending on the compiler version selected for any given file, deployed contracts may have security issues.*

The minimum required version must be [0.8.16](https://github.com/ethereum/solidity/releases/tag/v0.8.16); otherwise, contracts will be affected by the following **important bug fixes**:

[0.8.9](https://blog.soliditylang.org/2021/09/29/solidity-0.8.9-release-announcement/):

- Immutables: Properly perform sign extension on signed immutables.
- User Defined Value Type: Fix storage layout of user defined value types for underlying types shorter than 32 bytes.

[0.8.13](https://blog.soliditylang.org/2022/03/16/solidity-0.8.13-release-announcement/):
- Code Generator: Correctly encode literals used in `abi.encodeCall` in place of fixed bytes arguments.

[0.8.14](https://blog.soliditylang.org/2022/05/18/solidity-0.8.14-release-announcement/):

- ABI Encoder: When ABI-encoding values from calldata that contain nested arrays, correctly validate the nested array length against `calldatasize()` in all cases.
- Override Checker: Allow changing data location for parameters only when overriding external functions.

[0.8.15](https://blog.soliditylang.org/2022/06/15/solidity-0.8.15-release-announcement/)

- Code Generation: Avoid writing dirty bytes to storage when copying `bytes` arrays.
- Yul Optimizer: Keep all memory side-effects of inline assembly blocks.

[0.8.16](https://blog.soliditylang.org/2022/08/08/solidity-0.8.16-release-announcement/)

- Code Generation: Fix data corruption that affected ABI-encoding of calldata values represented by tuples: structs at any nesting level; argument lists of external functions, events and errors; return value lists of external functions. The 32 leading bytes of the first dynamically-encoded value in the tuple would get zeroed when the last component contained a statically-encoded array.

Apart from these, there are several minor bug fixes and improvements.

## **3. Denial of service during `redeem`**

The `TribeRedeemer` constructor does not check that the same token appears repeated in the `_tokensReceived` array.

If the `tokensReceivedOnRedeem` method returns the same token twice, the amount that can be redeemed will be returned twice, since the balance of the contract is used and in both iterations it will be the same, this will cause that when the method is called `redeem`, this fails due to not having enough balance.

```javascript
    function previewRedeem(uint256 amountIn) public view
        returns (address[] memory tokens, uint256[] memory amountsOut)
    {
        tokens = tokensReceivedOnRedeem();
        amountsOut = new uint256[](tokens.length);
        uint256 base = redeemBase;
        for (uint256 i = 0; i < tokensReceived.length; i++) {
            uint256 balance = IERC20(tokensReceived[i]).balanceOf(address(this));
            require(balance != 0, "ZERO_BALANCE");
            // @dev, this assumes all of `tokensReceived` and `redeemedToken`
            // have the same number of decimals
            uint256 redeemedAmount = (amountIn * balance) / base;
            amountsOut[i] = redeemedAmount;
        }
    }
    function redeem(address to, uint256 amountIn) external nonReentrant {
        IERC20(redeemedToken).safeTransferFrom(msg.sender, address(this), amountIn);
        (address[] memory tokens, uint256[] memory amountsOut) = previewRedeem(amountIn);
        uint256 base = redeemBase;
        redeemBase = base - amountIn; // decrement the base for future redemptions
        for (uint256 i = 0; i < tokens.length; i++) {
            IERC20(tokens[i]).safeTransfer(to, amountsOut[i]);
        }
        emit Redeemed(msg.sender, to, amountIn, base);
    }
```
**Recommended change:**

- It's recommended to ensure that tokens always will be different.

**Affected source code:**

- [TribeRedeemer.sol:44-L61](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L44-L61)

## **4. Unsafe math**

A user supplied argument can be passed as zero to multiplication operation.

**Reference:**

- https://slowmist.medium.com/analysis-of-the-treasuredao-zero-fee-exploit-73791f4b9c14

**Affected source code:**

- [RariMerkleRedeemer.sol:85](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L85)
- [TribeRedeemer.sol:58](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L58)
- [TribeRedeemer.sol:67](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L67)

---

# Non critical

## **5. Use same data sources**

The `previewRedeem` method calls the `tokensReceivedOnRedeem` method to get the list of tokens, however to iterate them it calls the state variable directly, if this method becomes `virtual` and is overridden in the future, a denial of service could occur when using different variables.

```diff
    function tokensReceivedOnRedeem() public view returns (address[] memory) {
        return tokensReceived;
    }

    function previewRedeem(uint256 amountIn)
        public
        view
        returns (address[] memory tokens, uint256[] memory amountsOut)
    {
        tokens = tokensReceivedOnRedeem();
        amountsOut = new uint256[](tokens.length);

        uint256 base = redeemBase;
-       for (uint256 i = 0; i < tokensReceived.length; i++) {
+       for (uint256 i = 0; i < tokens.length; i++) {
-           uint256 balance = IERC20(tokensReceived[i]).balanceOf(address(this));
+           uint256 balance = IERC20(tokens[i]).balanceOf(address(this));
            require(balance != 0, "ZERO_BALANCE");
            // @dev, this assumes all of `tokensReceived` and `redeemedToken`
            // have the same number of decimals
            uint256 redeemedAmount = (amountIn * balance) / base;
            amountsOut[i] = redeemedAmount;
        }
    }
```

**Affected source code:**

- [TribeRedeemer.sol:54-59](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L54-L59)

## **6. Improve `previewRedeem` logic**

The `previewRedeem` method of the `TribeRedeemer` contract performs a check that the balance of the token is different from 0, otherwise this execution will fail, I think that this check should not be done in this method since it is a `view` method, its result should be 0 instead of failing, and if you want to check this value, you should do it in the `redeem` method.

**Affected source code:**

- [TribeRedeemer.sol:55](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L55)

## **7. Use hardcoded values**

It is not good practice to hardcode values, but if you are dealing with addresses much less, these can change between implementations, networks or projects, so it is convenient to remove these addresses from the source code.

**Affected source code:**

- [SimpleFeiDaiPSM.sol:19](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L19)
- [SimpleFeiDaiPSM.sol:20](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L20)

## **8. Use the right value**

Although the change is 1 to 1 and does not affect the logic, it is better to use the in and out variables instead of the same one, since if the conversion factor changes, the rest of the logic will not be affected and will be changed, reducing the change of human errors.

**Recommended change:**

```diff
    /// @notice mint `amountFeiOut` FEI to address `to` for `amountIn` underlying tokens
    /// @dev see getMintAmountOut() to pre-calculate amount out
    function mint(
        address to,
        uint256 amountIn,
        uint256 minAmountOut
    ) external returns (uint256 amountFeiOut) {
        amountFeiOut = amountIn;
        require(amountFeiOut >= minAmountOut, "SimpleFeiDaiPSM: Mint not enough out");
        DAI.safeTransferFrom(msg.sender, address(this), amountIn);
        FEI.mint(to, amountFeiOut);
-       emit Mint(to, amountIn, amountIn);
+       emit Mint(to, amountIn, amountFeiOut);
    }

    /// @notice redeem `amountFeiIn` FEI for `amountOut` underlying tokens and send to address `to`
    /// @dev see getRedeemAmountOut() to pre-calculate amount out
    /// @dev FEI received is not burned, see `burnFeiHeld()` below to batch-burn the FEI redeemed
    function redeem(
        address to,
        uint256 amountFeiIn,
        uint256 minAmountOut
    ) external returns (uint256 amountOut) {
        amountOut = amountFeiIn;
        require(amountOut >= minAmountOut, "SimpleFeiDaiPSM: Redeem not enough out");
        FEI.safeTransferFrom(msg.sender, address(this), amountFeiIn);
        DAI.safeTransfer(to, amountOut);
-       emit Redeem(to, amountFeiIn, amountFeiIn);
+       emit Redeem(to, amountFeiIn, amountOut);
    }
```

**Affected source code:**

- [SimpleFeiDaiPSM.sol:42](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L42)
- [SimpleFeiDaiPSM.sol:57](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L57)

## **9. Unify coding style**

The `_multiClaim` method calls the `_claim` method multiple times, however the `_multiRedeem` method replicates the logic and allows different implementations to perform the same action.

Such is the case of line [231](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L231), where it is checked that the token is different from `address(0)`, a condition that is not done in the `_redeem` method.

```javascript
    require(cTokens[i] != address(0), "Invalid cToken address");
```

**Affected source code:**

- [RariMerkleRedeemer.sol:201-253](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L201-L253)


