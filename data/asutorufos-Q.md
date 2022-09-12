L-1 MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES
[TribeRedeemer.sol L#32](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#:~:text=redeemedToken%20%3D%20_redeemedToken%3B)

N-1 EVENT IS MISSING `INDEXED` FIELDS
Each event should use three `indexed` fields if there are three or more fields
[SimpleFeiDaiPSM.sol L#27,29](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#:~:text=event%20Redeem(,uint256%20amountFeiOut)%3B)

N-2 PRAGMA VERSION
In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#:~:text=pragma%20solidity%20%5E0.8.4%3B

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#:~:text=pragma%20solidity%20%5E0.8.4%3B

