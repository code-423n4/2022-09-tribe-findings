# [L-01] User could lose redeem tokens if the amount of cTokens given is too little

## Line References

[RariMerkleRedeemer.sol#L81-L86](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81-L86)

## Description

In the ```previewRedeem``` function in ```RariMerkleRedeemer.sol``` the amount of cToken sent to the contract is used to calculate the amount of ```baseToken``` to be received by the user. If a user enters an amount without adding decimals and the exchange rate is low enough then they may lose the amount of cTokens sent to the contract.

## Impact

Since the amount of cTokens must be low enough for the ```previewRedeem``` calculations to return 0, the amount of funds lost would realistically not be too much.

## Proof of Concept

```previewRedeem``` in ```RariMerkleRedeemer.sol```:

```solidity
    function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
        // Each ctoken exchange rate is stored as how much you should get for 1e18 of the particular cToken
        // Thus, we divide by 1e18 when returning the amount that a person should get when they provide
        // the amount of cTokens they're turning into the contract
        return (cTokenExchangeRates[cToken] * amount) / 1e18;
    }
```

Since the only requirement for the exchange rate is to be > 1e10, it could have a value of 2e10. The user would need to input an amount of atleast 5e7 so that ```previewRedeem``` does not return 0. If a user forgets to include the decimals and enters an amount of 10 for example then the returned amount will be 0.

## Recommended Mitigation Steps

Consider adding a sanity check in the ```_redeem``` function to ensure the amount redeemed is not 0.





# [L-02] A ```redeemBase``` value that is too large could break redeeming

## Line References

[TribeRedeemer.sol#L44-L61](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L44-L61)

## Description

If the ```redeemBase``` variable in ```TribeRedeemer.sol``` is too large [(```redeemBase > amountIn*balance```)](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L58) then the ```amountsOut``` to be sent to the redeemer could be 0 for some or all tokens.

## Impact

The redeemer could lose their ```redeemedToken``` amount sent to the contract. This could be true if the ```redeemBase``` is set too high in comparison to the balance of a ```tokensReceived``` token multiplied by the amount of ```redeemedToken``` sent by the redeemer. The function could also return 0 if the balances of the ```tokensReceived``` tokens in the contract are too low.

## Proof of Concept

```previewRedeem``` in ```TribeRedeemer.sol```:

```solidity
    function previewRedeem(uint256 amountIn)
        public
        view
        returns (address[] memory tokens, uint256[] memory amountsOut)
    {
        tokens = tokensReceivedOnRedeem();
        amountsOut = new uint256[](tokens.length);

        uint256 base = redeemBase;
        for (uint256 i = 0; i < tokensReceived.length; i++) {
            uint256 balance = IERC20(tokensReceived[i]).balanceOf(address(this));
            require(balance != 0, "ZERO_BALANCE");
            // @dev, this assumes all of `tokensReceived` and `redeemedToken`
            // have the same number of decimals
            uint256 redeemedAmount = (amountIn * balance) / base;
            amountsOut[i] = redeemedAmount;
        }
    }
```

If ```amountIn*balance < base``` the redeemedAmount for that token will be 0. If this is the case for all tokens, the user will receive nothing for the Tribe amount sent to the contract.

## Recommended Mitigation Steps

Check that at least 1 ```amountsOut``` is more than 0.