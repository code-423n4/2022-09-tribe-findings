### **Misleading comment in `RariMerkleRedeemer.sol` `_claim()`**

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L170](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L170)

**Description**

```solidity
// check: verify that claimableAmount is zero, revert if not
require(claims[msg.sender][_cToken] == 0, "User has already claimed for this cToken.");
```

The comment says we check if `claimableAmount` is zero but it actually checks if a user has already claimed for a given cToken

**Recommendation**

Remove the misleading comment - the code and the require statement error message are self explanatory

### **`claimAndRedeem` is missing from `MultiMerkleRedeemer.sol` base class**

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L99](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L99)

**Description**

The `claimAndRedeem()` function in `RariMerkleRedeemer.sol` is external but it is missing from the `MultiMerkleRedeemer.sol` base class, even though `claim()` `multiClaim()` and `multiRedeem()` are part of it.

**Recommendation**

Add the `claimAndRedeem()` function to the `MultiMerkleRedeemer.sol` base class, and then add the `override` keyword to `claimAndRedeem()` in `RariMerkleRedeemer.sol`

### **Missing modifier in `signAndClaim` in `RariMerkleRedeemer.sol`**

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L88](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L88)

**Description**

Function `signAndClaim` allows signing, but doesn’t enforce with the intended modifier that the user hasn’t signed a message before.

**Recommendation**

Add the `hasNotSigned` modifier to `signAndClaim`

### **Missing upper bound for `_exchangeRates` in `RariMerkleRedeemer.sol`**

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L129](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L129)

**Description**

The code has a validation if the exchange rate is too small

```
require( _exchangeRates[i] > 1e10,
                "Exchange rate must be greater than 1e10. Did you forget to multiply by 1e18?"
       );
```

but it does not have to check if the exchange rate is too big

**Recommendation**

Add the following piece of code as a validation for each exchange rate

```solidity
uint256 private constant MAX_EXCHANGE_RATE = 0; // you should configure this based on the tokens you are using
require(_exchangeRates[i] <= MAX_EXCHANGE_RATE,
"Exchange rate must be smaller than MAX_EXCHANGE_RATE");
```

### **Missing non-zero address checks in `RariMerkleRedeemer.sol` constructor**

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L41](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L41)

**Description**

The constructor of `RariMerkleRedeemer.sol` takes an array of addresses `_cTokens` argument and saves them to memory in `_configureExchangeRates()`, but does not validate if it doesn’t contain a zero address.

**Recommendation**

In the loop of `_configureExchangeRates()` add a check that each cToken has non-zero address value. If you add the check there you can omit it in `_configureMerkleRoots()` and also remove the non-zero address check from `[_multiReedeem()](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L231)`

### **Wrong error messages in `RariMerkleRedeemer.sol`**

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125)

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138)

**Description**

In `_configureExchangeRates()` we have the following check

```
require(_cTokens.length == 27, "Must provide exactly 27 exchange rates.");
```

but the error message is not exactly what the require statements checks. Same situation for `_configureMerkleRoots()` , the code there is 

```
require(_cTokens.length == 27, "Must provide exactly 27 merkle roots");
```

**Recommendation**

Change the require statement error message in `_configureExchangeRates()` from "Must provide exactly 27 exchange rates." to “Must provide exactly 27 cTokens”

and change the require statement error message in `_configureMerkleRoots()` from "Must provide exactly 27 merkle roots" to “Must provide exactly 27 cTokens”

### **Missing non-zero address checks in `TribeRedeemer.sol` constructor**

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L27](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L27)

**Description**

`_redeemedToken` and `_tokensReceived` are of types `address` and `address[]` but they are not checked if the value passed is not the zero address

**Recommendation**

Add checks to ensure that each value passed of type `address` is not the zero address

### **Missing non-zero address check in `TribeRedeemer.sol` `redeem()`**

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L64](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L64)

**Description**

The `redeem()` function in `TribeRedeemer.sol` has a `to` parameter that is not checked if it is the zero address so if a user mistakenly passes the zero address as an argument it can result in tokens transferred to the zero address, effectively getting burned. For this to happen the token contract should allow `transfer` to the zero address and there are some ERC20 tokens that allow it.

**Recommendation**

Add the following code on the first line of the `redeem()` method:

```jsx
require(to != address(0), "Zero address recipient");
```

### **Code has an assumption comment instead of an implemented check**

[https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L56](https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L56)

**Description**

In `previewRedeem()` in `TribeRedeemer.sol` we see this comment

```solidity
// @dev, this assumes all of `tokensReceived` and `redeemedToken`
// have the same number of decimals
```

Instead of it a check should be implemented in the constructor when setting both `tokensReceived` and `redeemedToken` so the code reverts if they don’t have the same number of decimals.

**Recommendation**

In the constructor of `TribeRedeemer.sol` loop through the `_tokensReceived` array and check if each token’s decimals are the same as the `_redeemedToken` argument’s decimals.