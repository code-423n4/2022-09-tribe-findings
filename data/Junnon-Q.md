## Lack of `address(0)` and `0` value variable check
`address(0)` check is an best practice that can prevent attack by default variable. In some case lack of address(0) check can be fatal(recalling the [Nomad hack](https://twitter.com/samczsun/status/1554252024723546112)). it can prevent deploying contract with wrong constructor parameters too.

Mitigation 
this issue can be mitigated with these example code below
```js
    require(addr != address(0),"AddressZero");
    require(num != 0, "0 input");
```

Instance:
RariMerkleRedeemer.sol
1. RariMerkleRedeemer::constructor()::cTokens
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L37
2. RariMerkleRedeemer::_claim()::{_cToken, _amount}
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L165
3. RariMerkleRedeemer::_redeem()::cToken
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L201

TribeRedeemer.sol
1. TribeRedeemer::constructor()::{_redeemedToken, _tokensReceived, _redeemBase}
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L28-L30
2. TribeRedeemer::redeem()::{to, amountIn}
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L64

SimpleFeiDaiPSM.sol
1. SimpleFeiDaiPSM::mint()::{to, amountIn, minAmountOut}
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L34-L36
2. SimpleFeiDaiPSM::redeem()::{to, amountFeiIn, minAmountOut}
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L48


## Unnecessary check 
In [SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol) there's a check that unnecessary. check will only make contract difficult to operate without real effect for the contract. 

Mitigation:
the logic can be delete since has no effect for the contract 

NOTE: If the parameter can't be deleted (due to pattern for example). it can included in the function and give it a comment. 
example:
```js
    function mint(
        address to,
        uint256 amountIn,
        uint256 minAmountOut ///unused variable 
    ) external returns (uint256 amountFeiOut) {
        minAmountOut; /// the reason for not using the variable 
        
        ...// next implementation 
    }
```

Instance:
SimpleFeiDaiPSM.sol
1. SimpleFeiDaiPSM::mint()
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L39
2. SimpleFeiDaiPSM::redeem()
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L54

## Unnecessary `pure` function 
Unnecessary function can be deleted for more clean and readable code. the data that give in this function can be added to frond-end instead. 

Mitigation:
can be deleted

Instance:
SimpleFeiDaiPSM.sol
```js
60    /// @notice calculate the amount of FEI out for a given `amountIn` of underlying
61    function getMintAmountOut(uint256 amountIn) external pure returns (uint256) {
62        return amountIn;
63    }
64
65
66    /// @notice calculate the amount of underlying out for a given `amountFeiIn` of FEI
67    function getRedeemAmountOut(uint256 amountIn) external pure returns (uint256) {
68        return amountIn;
69    }
```
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L60-L68

## Unnecessary import 
import that ain't use in the contract can make contract more complex. i recommend to delete it instead if it doesn't have effect for the contract. 

Mitigation:
can be deleted

Instance:
RariMerkleRedeemer.sol
```js
import "@openzeppelin/contracts/utils/cryptography/SignatureChecker.sol";
```
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L7

## Missing `indexed` mark on event parameter
`indexed` parameter in event can very helpfull for indexing data offchain. In some instance below developer forget to add `indexed` mark in some event parameter.

Mitigation:
add `indexed` mark
example:
```js
    event Something(address indexed addr);
```

Instance:
SimpleFeiDaiPSM.sol
```js
26    /// @notice event emitted upon a redemption
27    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
28    /// @notice event emitted when fei gets minted
29    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L26-L29


