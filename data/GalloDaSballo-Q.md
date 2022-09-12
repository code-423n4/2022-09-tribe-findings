# Executive Summary

Personally haven't found any glaring vulnerability.

Because of the mix of immutability and Admin Trust, end users should review the deployment settings at that time to ensure that no misconfigurations have happened.

In case any misconfigurations happens, a new deployment will be required.

Listed below are some observations of things that could be refactored to make the code more consistent, as well as some gotchas that are introduced by the choice of architecture

## Inconsistent usage of `hasNotSigned`

`signAndClaimAndRedeem` has the modifier while `signAndClaim` doesn't

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L108-L118

```solidity
    function signAndClaimAndRedeem(
        bytes calldata signature,
        address[] calldata cTokens,
        uint256[] calldata amountsToClaim,
        uint256[] calldata amountsToRedeem,
        bytes32[][] calldata merkleProofs
    ) external override hasNotSigned nonReentrant {
        _sign(signature);
        _multiClaim(cTokens, amountsToClaim, merkleProofs);
        _multiRedeem(cTokens, amountsToRedeem);
    }
```

Doesn't have the modifier
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L88-L97

```solidity
    function signAndClaim(
        bytes calldata signature,
        address[] calldata cTokens,
        uint256[] calldata amounts,
        bytes32[][] calldata merkleProofs
    ) external override nonReentrant {
        // both sign and claim/multiclaim will revert on invalid signatures/proofs
        _sign(signature);
        _multiClaim(cTokens, amounts, merkleProofs);
    }
```

### Mitigation Steps

Add the modifier `hasNotSigned` for consistency, or remove the modifier altogether and allow to sign multiple times the same message


## Left the Default MESSAGE_HASH

For the in-scope code `MESSAGE` is left to the default value

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L53-L54

```solidity
    string public constant MESSAGE = "Sample message, please update.";

```

This could cause the signature to be replayable in other applications that use the same message.

### Mitigation Steps

Add the proper message, most likely a TOS acknowledgement or a ipfs hash to a document


## Technically the maximum amount at risk is `amountToDrip * 2 - 1`

While the code is meant to limit the total asset at risk, it's important to notice that because of the following check:

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L23-L24

```solidity
            IERC20(token).balanceOf(target) < amountToDrip,

```

Any balance below `amountToDrip`, after enough time has passed, will alow to drip more, meaning the total amount at risk is not `amountToDrip` but up to `2 * amountToDrip - 1`

### Mitigation Steps

I recommend commenting this


## Tautology, slippage check is ineffective

Because `amountOut = amountFeiIn;` and `amountFeiOut = amountIn;` there is no slippage, and no risk for any front-run

The check below is always true for that reason:

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L54-L55

```solidity
        require(amountOut >= minAmountOut, "SimpleFeiDaiPSM: Redeem not enough out");

```

### Mitigation Steps

A better require would be to check if the balance of the token that will be transferred out is sufficient to avoid a revert on the low level balance subtraction


## Trust in Deployer - No guarantee that base will be totalSupply
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L34-L35

```solidity
        redeemBase = _redeemBase;

```

Because `redeemedToken` is known, you could just retrieve the `totalSupply` from it to ensure the claims are for all tokens available.

End users will have to verify that `redeemBase` is consistent with the Circulating Supply.

### Mitigation Steps

Comment and let end users know of this, or use `totalSupply` from the token

## Trust in Deployer - Lack of check for same decimals

Because TribeRedeemer assumes all assets will have the same decimals, tokens can remain stuck if they have a different amount of decimals.

A simple check for decimals in the constructor can avoid this scenario
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L33-L34

```solidity
        tokensReceived = _tokensReceived;

```

In lack of a check, end users will have to verify all tokens have 18 decimals.

All of the tokens in the README have 18 decimals at this time.


## Notice - Slippage and Interest Free Loan for Arbitrageurs

Because `redeem` doesn't burn FEI, any caller can `mint` and `redeem` multiple times in the same tx with the goal of arbing out the FEI - DAI pair

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L105-L106

```solidity
    function burnFeiHeld() external {

```

While FEI being tradeable for DAI is enforcing a 1-1 trade (FEI price goes up due to arbing, up to 100% + FEE), allowing the opposite swap is a easy target for arbitrageurs



# Usual Suspects

## Lack of Zero Checks for new addresses

Constructors don't have zero-checks, which could force a re-deployment, funds are not at risk in those cases