1. Missing Indexed

Files :

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L14

Missing `indexed` uint256 amount

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L26-L29

Misssing `indexed` address to

2. Unnecessary Reason Strings

Files :

1.) https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L131

Doesn't need `Did you forget to multiply by 1e18?` .


3. Missmatch require with comment

Files :

1.) https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L202-L203

It must be used `> 0` to be same as `greater than zero` or it can be changed into `can't be zero`

Same case :

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L233-L234

4. Typo Comment

Files : 

1.) https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L246

`juuuuust`

5. Locked Pragma Compiler 

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L2

Since it was used ^0.8.6. As the compiler can be use for example 0.8.10 and consider locking at this version the same as another. It can be consider using  locking the pragma version whenever possible and avoid using a floating pragma in the final deployment. Since it can be problematic, if there are publicly disclosed bugs and issues that affect the current compiler version used.
