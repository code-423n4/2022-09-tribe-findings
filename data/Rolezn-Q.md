## (1) Avoid Floating Pragmas: The Version Should Be Locked

Severity: Non-Critical

## Proof Of Concept

	pragma solidity ^0.8.4;
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/SimpleFeiDaiPSM.sol#L2

	pragma solidity ^0.8.4;
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2

## Recommended Mitigation:
Avoid the usage of floating pragmas, the version should be locked.


## (2) Event Is Missing Indexed Fields

Severity: Non-Critical

Each event should use three indexed fields if there are three or more fields.

## Proof Of Concept


	event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/SimpleFeiDaiPSM.sol#L27

	event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/SimpleFeiDaiPSM.sol#L29

## Recommended Mitigation Steps
Add indexed fields for events.


## (3) Implementation contract may not be initialized

OpenZeppelin recommends that the initializer modifier be applied to constructors. 
Per OZs Post implementation contract should be initialized to avoid potential griefs or exploits.
https://forum.openzeppelin.com/t/uupsupgradeable-vulnerability-post-mortem/15680/5

Severity: Non-Critical

## Proof Of Concept

	constructor(
        address _core,
        address _target,
        uint256 _dripPeriod,
        uint256 _amountToDrip,
        address _token
    ) ERC20Dripper(_core, _target, _dripPeriod, _amountToDrip, _token) {}
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L10-L16

	constructor(
        address token,
        address[] memory cTokens,
        uint256[] memory rates,
        bytes32[] memory roots
    ) {
        _configureExchangeRates(cTokens, rates);
        _configureMerkleRoots(cTokens, roots);
        _configureBaseToken(token);
    }
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L35-L44

	constructor(
        address _redeemedToken,
        address[] memory _tokensReceived,
        uint256 _redeemBase
    ) {
        redeemedToken = _redeemedToken;
        tokensReceived = _tokensReceived;
        redeemBase = _redeemBase;
    }
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/redeem/TribeRedeemer.sol#L27-L35

## Recommended Mitigation Steps
Add initializer modifier in the constructor per OpenZeppelin's recommendation.


## (4) Use a more recent version of Solidity

Severity: Non-Critical

Use a solidity version of at least 0.8.12 to get string.concat() instead of abi.encodePacked(<str>,<str>)
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions

## Proof Of Concept


	Found old version 0.8.4 of Solidity SimpleFeiDaiPSM.sol
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/peg/SimpleFeiDaiPSM.sol#L2

	Found old version 0.8.10 of Solidity in MerkleRedeemerDripper.sol
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L2

	Found old version 0.8.10 of Solidity in RariMerkleRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L2

	Found old version 0.8.4 of Solidity in TribeRedeemer.sol
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/redeem/TribeRedeemer.sol#L2



## Recommended Mitigation Steps

Consider updating to a more recent solidity version.



## (5) Public Functions Not Called By The Contract Should Be Declared External Instead

Severity: Non-Critical

Contracts are allowed to override their parents’ functions and change the visibility from external to public.

## Proof Of Concept


	function drip() public override {
https://github.com/code-423n4/2022-09-tribe/tree/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L21

## Recommended Mitigation Steps
Update function drip from public to external.



