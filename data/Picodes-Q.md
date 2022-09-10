[NC - 01] - Incorrect comment

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L48

“The amount of cTokens a user in their claim” -> “The amount of cTokens a user has in their claim” ?

[NC - 02] - `MESSAGE_HASH` could be constant

In `MultiMerkleRedeemer`, `MESSAGE` is constant but `MESSAGE_HASH` is not, which looks like an incoherence.

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L56


[NC - 03] - `MultiMerkleRedeemer` only works for EOA
`MultiMerkleRedeemer`: How do you intend to handle all the multisig and other contracts ?

[NC - 04] - Typo

“User provides the the cToken” -> “User provides the cToken”

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L163
