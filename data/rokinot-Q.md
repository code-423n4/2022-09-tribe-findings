// Low

Total FEI supply can be inflated

FEI at `SimpleFeiDaiPSM.sol` , evidenced by how it can be burned by anyone who calls this function. But it's still FEI that can be added to the total supply. If an actor were to acquire 10M FEI either by being wealthy, or using a flash loan, they can artificially increase supply.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L105