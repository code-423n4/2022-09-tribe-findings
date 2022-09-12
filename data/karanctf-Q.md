## \[N-1\] Inconsistent  `/` in comments

```solidity
shutdown/fuse/RariMerkleRedeemer.sol:
    163:	// User provides the the cToken & the amount they should get, and it is verified against the merkle root for that cToken
    164:	/// Should set the user's claim amount int he claims mapping for the provided cToken
```



## \[N-20\] Event is missing `indexed` fields

Each `event` should use three `indexed` fields if there are three or more fields

```solidity
shutdown/redeem/TribeRedeemer.sol
		14:		event Redeemed(address indexed owner, address indexed receiver, uint256 amount, uint256 base);
		
contracts/peg/SimpleFeiDaiPSM.sol
		27:		event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
		29:		event Mint(address to, uint256 amountIn, uint256 amountFeiOut);


```
