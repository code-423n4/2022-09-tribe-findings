
Use open pragma only on interfaces or libraries

In [SimpleFeiDaiPSM.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L2), [TribeRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L2) and [MultiMerkleRedeemer.sol#L2](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L2)
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

Take a look into [SWC-103](https://swcregistry.io/docs/SWC-103)

---

Use named imports, as you do on [MerkleRedeemerDripper.sol#L4-L5](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L4-L5)

In this cases you are not using named imports;
[MultiMerkleRedeemer.sol#L4](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L4)
[SimpleFeiDaiPSM.sol#L4-L5](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L4-L5)
[RariMerkleRedeemer.sol#L4-L9](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L4-L9)

---

`SimpleFeiDaiPSM.sol#L92-L98`: Use recommend order especified in soliditylang
According to [order-of-layout](https://docs.soliditylang.org/en/latest/style-guide.html#order-of-layout) in soliditylang the order should be;
> Inside each contract, library or interface, use the following order:
> 1. Type declarations
> 2. State variables
> 3. Events
> 4. Modifiers
> 5. Functions

This variables in [SimpleFeiDaiPSM.sol#L92-L98](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98) and [SimpleFeiDaiPSM.sol#L75](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L75) should be declared in the beginning of the contract.


---

Revert messages should be lower than 32 bytes;
[SimpleFeiDaiPSM.sol#L39](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L39)
[SimpleFeiDaiPSM.sol#L54](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L54)
[RariMerkleRedeemer.sol#L125-L126](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L125-L126)
[RariMerkleRedeemer.sol#L138-L139](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L138-L139)
[RariMerkleRedeemer.sol#L171](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L171)
[RariMerkleRedeemer.sol#L190-L191](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L190-L191)
[RariMerkleRedeemer.sol#L227](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L227)
[RariMerkleRedeemer.sol#L239](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L239)
[MerkleRedeemerDripper.sol#L24](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L24)

Try to use custom errors or send a shorter error messages. Checkout this article;
https://blog.soliditylang.org/2021/04/21/custom-errors/
