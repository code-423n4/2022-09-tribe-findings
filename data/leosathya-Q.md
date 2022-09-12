## L-1 Loops on Dynamic array can lead to Denial Of Service(DoS)
In previewRedeem() from TribeRedeemer.sol file, a for loop operates on a Dynamic array. If the array is that much big then it could lead to a DoS condition.
Again this previewRedeem() used inside redeem() so this function also vulnerable to this condition.

## L-2 Address 0 check absent in Constructor
In TribeRedeemer.sol Zero address checks absent for reedmeToken and uint condition check absent for redeemBase variable.

## L-3 Some Error Messages are more than 32 bytes
In whole code base there are some require error messages that could be more than 32bytes
To Optimize those use error functions 

## L-4 Multiple Solidity pragma:
It is better to use one Solidity compiler version across all contracts instead of different versions with different bugs and security checks.
RariMerkleRedeemer.sol , MerkleRedeemerDripper.sol both use 0.8.10 solidity version where, SimpleFeiDaiPSM.sol and TribeRedeemer.sol use floating pragma

## G-1 Duplicate Require() statements used
In RariMarkleRedeemer.sol same require() statement used in line-125 and 138
To save gas, require() that use multiple time should convert to a modifier

## G-2 _cTokens.length should cached 
In function _configureExchangeRates() and _configureMerkleRoots() _cToken.length calculated multiple time, so it should be cached first then used in loop

## G-3 Loops could be more optimized
Here i notice that each loop which are used whole code base could be optimized as follow
. Do not assign 0 to uint, by doing so you save some gas. it's default value is 0
. first cach the <Array>.length to a stack variable and then use it in loop condition, so that it will not repeatedly calculate corresponding array's length
. use ++i instead of i++  

## N-1 Solidity Coding style violeted
In SimpleFeiDaiPSM.sol from line - 92 to 98 constant variables are declared in smaller case
Should declared in Capitilized SNAKE_CASE