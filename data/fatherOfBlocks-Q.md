**SimpleFeiDaiPSM**
- L2 - Uses a newer version of robustness.
Use a robustness version of at least 0.8.10 so that external calls skip contract existence checks if the external call has a return value.

- L36/38/39/51/53/54 - The function mint() and redeem() request three parameters, but one of them (minAmountOut) is only used to compare that it is <= another input. This is something that does not make much sense, the code should be simplified.

- L19/75/94 - The constant balanceReportedIn is created but it is never used. In addition, it is a duplicate data since in line 19 the DAI address is defined with the IERC20 interface, therefore, if you want to use it as an address, it is wrapped when you want to use it.

**Reedemer Tribe**
- L34/52 - Since redeemBase is initialized without any restrictions, when used in the previewRedeem() function it will be able to make the base equal to zero, making the division reverse since it cannot be divided by zero.
The correct thing would be to validate that base != 0 and otherwise throw an exception with a message. Another solution would be to validate in the constructor that it is != 0.

**RariMerkleRedeemer**
- L4/7 - There is an import that is not used by anyone, it could be eliminated so that it does not generate any problem.