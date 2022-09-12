* For the ```SimpleFeiDaiPSM``` contract, please keep in mind that DAI is considering to depeg from USD:
https://decrypt.co/107273/makerdao-founder-dai-drop-dollar-peg-tornado-cash-usdc

* Consider making event parameters ```indexed```, e.g. address field here:
```solidity
  /// @notice event emitted upon a redemption
  event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
  /// @notice event emitted when fei gets minted
  event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```

* Function ```signAndClaim``` in ```RariMerkleRedeemer``` probably needs ```hasNotSigned``` modifier, otherwise a malicious user can call the function multiple times with a signature and empty tokens array, thus spamming ```Signed``` event:
```solidity
  function signAndClaim(
      bytes calldata signature,
      address[] calldata cTokens,
      uint256[] calldata amounts,
      bytes32[][] calldata merkleProofs
  ) external override nonReentrant {
```

* Misleading comments about nonexisting ```claimableAmount``` (there are more places of occurrence):
```solidity
  // check: verify that claimableAmount is zero, revert if not
  require(claims[msg.sender][_cToken] == 0, "User has already claimed for this cToken.");
```
```solidity
  // effect: update claimableAmount for the user
  claims[msg.sender][_cToken] = _amount;
```

* A small discrepancy between ```_redeem``` and ```_multiRedeem```: multi redeem for some reason checks that cToken address is not empty while single redeem doesn't:
```solidity
  // check: cToken cannot be the zero address
  require(cTokens[i] != address(0), "Invalid cToken address");
```
This should not cause any issue but it would be nice to have these functions coherent.

* ```TribeRedeemer``` should have a reasonable upper limit for the number of ```tokensReceived```, otherwise calculations when iterating all the tokens might exceed the block gas limit.

* Function ```previewRedeem``` queries ```tokensReceivedOnRedeem``` but later iterates over ```tokensReceived``` directly:
```solidity
  function previewRedeem(uint256 amountIn)
    ...
  {
    tokens = tokensReceivedOnRedeem();
    amountsOut = new uint256[](tokens.length);
    ...
    for (uint256 i = 0; i < tokensReceived.length; i++)
```
While in practice ```tokensReceived``` and ```tokensReceivedOnRedeem``` return the same array, it would be better not to make any assumptions and continue with a local instance of tokens.

* Make sure that all the ```tokensReceived``` for ```TRIBE``` redemption is sent initially, otherwise those who redeem later would get more:
```solidity
  uint256 balance = IERC20(tokensReceived[i]).balanceOf(address(this));
  ...
  uint256 redeemedAmount = (amountIn * balance) / base;
```
Also, consider adding an option for the user to opt out of specific tokens to basically skip them when iterating, e.g. if there is not enough balance of the certain token and the user does not care about it.

* Do not forget to update the message:
```solidity
  require(ECDSA.recover(MESSAGE_HASH, _signature) == msg.sender, "Signature not valid");
```
```solidity
  string public constant MESSAGE = "Sample message, please update.";
```