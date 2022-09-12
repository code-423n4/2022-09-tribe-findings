## no address zero checks 
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L32-L35
## if deployer makes a mistake and adds a token with more then 18 decimals then there will be an accounting error and users will get to much funds back.Also if there is token with less then 18 decimals there can be fund loss. 
```
  uint256 balance = IERC20(tokensReceived[i]).balanceOf(address(this));
            require(balance != 0, "ZERO_BALANCE");
            // @dev, this assumes all of `tokensReceived` and `redeemedToken`
            // have the same number of decimals
          
            uint256 redeemedAmount = (amountIn * balance) / base;
```
and balance can be zero and there is no way of changeing it 
## since their is no way of changing tokens list  in `TribeRedeemer.sol` if the deploy makes a mistake on depolyment and  adds a eoa address  then both functoins will dos and or if a token balance of this address is zero then this functoin will revert 
## if there are alot of tokens on the token list then the function can be very exspensive to execute or cause the functions to dos
if on token list there are alot of `atokens` or `ctokens` which take up more gas.
Also since you are using sloading for what tokens are in the token array then its more gas which can cause there to be a dos.
```
  for (uint256 i = 0; i < tokensReceived.length; i++) {
            uint256 balance = IERC20(tokensReceived[i]).balanceOf(address(this));
            require(balance != 0, "ZERO_BALANCE");

```
## dont use unlocked pragma it can cause bugs in depolyment if there is diffrent version used for testing 
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/redeem/TribeRedeemer.sol#L2
##  if `cTokenExchangeRates` token deciamls is less then 18 decimals then the amount will be zero and there can be loss of funds
the decimals has to be more then 1e10 not more then 1e18 so if its less then 18 decimals then users wont get anything back and they can loose funds.
```

        for (uint256 i = 0; i < _cTokens.length; i++) {
            require(
                _exchangeRates[i] > 1e10,
                "Exchange rate must be greater than 1e10. Did you forget to multiply by 1e18?"
            );
            cTokenExchangeRates[_cTokens[i]] = _exchangeRates[i];
        }
```
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81
## typos 
instead of :juuuust
use : just
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L247
## there is no way of this contract burning its fei tokens 
beacuse it will burn tokens from msg.sender instead of address(this)
this function wont be called and anyone who calls this function will losse their `fei` but if its dosnt happen their can be a oversupply of fei and cause the price  to go down.
```js
    function burnFeiHeld() external {
        uint256 feiBalance = FEI.balanceOf(address(this));
        if (feiBalance != 0) {
            FEI.burn(feiBalance);
        }

```
https://github.com/code-423n4/2022-09-tribe/blob/eea6e9b31dfcb69b82446fc08917f857dc8e911a/contracts/peg/SimpleFeiDaiPSM.sol#L108
## if there is no intermidate contract that users wont be able to get their funds out
```
 DAI.safeTransfer(to, amountOut);

```
because balance is token off from msg.sender so there has to be a contract that calls this to make it work.
