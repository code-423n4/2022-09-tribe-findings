### Typos
___
[RariMerkleRedeemer.sol: L163](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L163)
```solidity
    // User provides the the cToken & the amount they should get, and it is verified against the merkle root for that cToken
```
Remove repeated word `the`
___
[RariMerkleRedeemer.sol: L246](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L246)
```solidity
        // We give the interactions (the safeTransferFroms) their own for loop, juuuuust to be safe
```
Change `juuuuust` to `just`
___
___

### Unclear comment
While a comment may contain abbreviations, shorthand for well-known or previously defined terms, inconsistent or nonstandard punctuation, and use of minor grammatical errors, it should be readable. In other words, it should communicate clearly, immediately and without ambiguity. The readability of the comment below could be improved, as shown:
___
[SimpleFeiDaiPSM.sol: L11](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L11)
```solidity
/// preventing it to create new FEI.
```
Replace `to create` with `from creating`
___
___

### Long single line comments 
In theory, comments over 79 characters should wrap using multi-line comment syntax. Even if somewhat longer comments are acceptable and a scroll bar is provided, very long comments can interfere with readability. Some of the long comments in `FEI and TRIBE` do wrap (e.g., [TribeRedeemer.sol: L23-24](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L23-L24)) — the treatment of long comments is inconsistent and should be made consistent. Below are five instances of long comments whose readability could be improved by wrapping, as shown:
___
[SimpleFeiDaiPSM.sol: L77](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L77)
```solidity
    /// @notice gets the effective balance of "balanceReportedIn" token if the deposit were fully withdrawn
```
Suggestion:
```solidity
    /// @notice gets the effective balance of "balanceReportedIn" token
    ///   if the deposit were fully withdrawn.
```
___
[RariMerkleRedeemer.sol: L163](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L163)
```solidity
    // User provides the the cToken & the amount they should get, and it is verified against the merkle root for that cToken
```
Suggestion:
```solidity
    // User provides the cToken & the amount they should get, and it is verified
    //   against the merkle root for that cToken.
```
___
[RariMerkleRedeemer.sol: L183](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L183)
```solidity
    // User provides the cTokens & the amounts they should get, and it is verified against the merkle root for that cToken (for each cToken provided)
```
Suggestion:
```solidity
    // User provides the cTokens & the amounts they should get, and it is verified 
    //   against the merkle root for that cToken (for each cToken provided).
```
___
[RariMerkleRedeemer.sol: L200](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L200)
```solidity
    // Transfers in a particular amount of the user's cToken, and increments their redeemed amount in the redemption mapping
```
Suggestion:
```solidity
    // Transfers in a particular amount of the user's cToken, and increments
    //   their redeemed amount in the redemption mapping.
```
___
[RariMerkleRedeemer.sol: L205](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L205)
```solidity
        // check: verify that the user's claimedAmount+amount of this cToken doesn't exceed claimableAmount for this cToken
```
Suggestion:
```solidity
        // check: verify that the user's claimedAmount+amount of this cToken
        //   doesn't exceed claimableAmount for this cToken.
```
___
___


### Named `return` variable not used
The existence of a named return variable that is not subsequently used (here, `baseTokenAmount`) is potentially perplexing
___
[RariMerkleRedeemer.sol: L81-86](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81-L86)
```solidity
    function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
        // Each ctoken exchange rate is stored as how much you should get for 1e18 of the particular cToken
        // Thus, we divide by 1e18 when returning the amount that a person should get when they provide
        // the amount of cTokens they're turning into the contract
        return (cTokenExchangeRates[cToken] * amount) / 1e18;
    }
```
___
___