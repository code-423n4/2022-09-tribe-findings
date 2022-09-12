1. Missing has not sign modifier

##POC
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L93

##IMPACT
making a user that has called sign function be able to call sign and claim function. 


2.Redeem base can't be modified after deployment.

##POC
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L34

##IMPACT
While deploying the tribe redeemer contract the deployer might set the redeem base value to a wrong value, this could lead user that wants to redeem their TRIBE token
didn't get what they are expecting and the deployer/owner cant fix this because there is no function that can modify the redeem base value.

3. While redeeming TRIBE token the total suply didnt decrease.

##POC
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L65

##IMPACT
While redeeming TRIBE token a user get a number of ERC20 token, while burning the TRIBE token
if you burn the TRIBE token you will need to call burn function so, the total supply will decrease.
So, the total supply will match the actual circulated TRIBE token.
