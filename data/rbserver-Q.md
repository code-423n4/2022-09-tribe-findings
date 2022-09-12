## [L-01] FUNCTIONALITY OF `RariMerkleRedeemer.claimAndRedeem` RELATED TO AMOUNT INPUTS IS MORE LIMITED THAN `RariMerkleRedeemer.signAndClaimAndRedeem`
When calling the following `RariMerkleRedeemer.signAndClaimAndRedeem` function, different amount inputs can be used for calling `_multiClaim` and `_multiRedeem`. However, when calling the `claimAndRedeem` function below, only one `amounts` input is used for calling both `_multiClaim` and `_multiRedeem`. Users, who have signed already, are not able to call `claimAndRedeem` if they want to call `_multiClaim` and `_multiRedeem` with different amount inputs and are forced to call `_multiClaim` and `_multiRedeem` separately in two different transactions, which costs more gas. To improve the usability of the `claimAndRedeem` function, it can be updated to include two different amount inputs that are used for calling `_multiClaim` and `_multiRedeem` respectively, which is similar to the `signAndClaimAndRedeem` function.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L108-L118
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

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L99-L106
```solidity
    function claimAndRedeem(
        address[] calldata cTokens,
        uint256[] calldata amounts,
        bytes32[][] calldata merkleProofs
    ) external hasSigned nonReentrant {
        _multiClaim(cTokens, amounts, merkleProofs);
        _multiRedeem(cTokens, amounts);
    }
```

## [L-02] DECIMALS OF `tokensReceived` AND `redeemedToken` CAN BE CHECKED
As mentioned by the comment below, the `previewRedeem` function "assumes all of `tokensReceived` and `redeemedToken` have the same number of decimals". To ensure this, a `require` statement can be added in the `TribeRedeemer` contract's `constructor` to verify that the decimals of `tokensReceived` and `redeemedToken` are all equal before completing the construction of the contract.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L44-L61
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

## [L-03] `MESSAGE` CAN BE UPDATED TO BE MORE SPECIFIC
The following `MESSAGE` is currently a sample generic message. Because `MESSAGE` is used for being signed by users, it can be updated to be more specific for improving user experience and avoiding confusion, especially if `MESSAGE` needs to be displayed by the frontend.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L52-L53
```solidity
    /// @notice The message to be signed by any users claiming on cTokens
    string public constant MESSAGE = "Sample message, please update.";
```

## [L-04] `_redeemBase` CAN BE CHECKED AGAINST 0 IN `TribeRedeemer` contract's `constructor`
As the `TribeRedeemer` contract's `constructor` shows below, `redeemBase` is possible to be set to 0. However, calling the following `previewRedeem` function will revert if `redeemBase` is 0 due to the division by 0. To prevent this from happening, a `require` statement can be added in the contract's `constructor` to verify that `_redeemBase` is not 0 before executing `redeemBase = _redeemBase`.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L27-L35
```solidity
    constructor(
        address _redeemedToken,
        address[] memory _tokensReceived,
        uint256 _redeemBase
    ) {
        redeemedToken = _redeemedToken;
        tokensReceived = _tokensReceived;
        redeemBase = _redeemBase;
    }
```

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L44-L61
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

## [L-05] MISSING ZERO-ADDRESS CHECK FOR CRITICAL ADDRESSES
To prevent unintended behaviors, the critical address inputs should be checked against `address(0)`.

Please consider checking the addresses of `cTokens` in the following `constructor`.
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L35-L44
```solidity
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
```

Please consider checking `_core` in the following `constructor`.
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/MerkleRedeemerDripper.sol#L10-L16
```solidity
    constructor(
        address _core,
        address _target,
        uint256 _dripPeriod,
        uint256 _amountToDrip,
        address _token
    ) ERC20Dripper(_core, _target, _dripPeriod, _amountToDrip, _token) {}
```

Please consider chekcing `_redeemedToken` and addresses of `_tokensReceived` in the following `constructor`.
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#L27-L35
```solidity
    constructor(
        address _redeemedToken,
        address[] memory _tokensReceived,
        uint256 _redeemBase
    ) {
        redeemedToken = _redeemedToken;
        tokensReceived = _tokensReceived;
        redeemBase = _redeemBase;
    }
```

## [N-01] TYPO IN COMMENT
The phrase in the comment for the following `_claim` function should be "in the" instead of "int he".
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L164-L169
```solidity
    /// Should set the user's claim amount int he claims mapping for the provided cToken
    function _claim(
        address _cToken,
        uint256 _amount,
        bytes32[] calldata _merkleProof
    ) internal virtual {
```

## [N-02] MISSING INDEXED EVENT FIELDS
Querying events can be optimized with index. Please consider adding index to fields of the following events.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#L26-L29
```solidity
    /// @notice event emitted upon a redemption
    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
    /// @notice event emitted when fei gets minted
    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```

## [N-03] NEWER VERSION OF SOLIDITY CAN BE USED
The protocol can benefit from more fixes and gas-efficient features by using a newer version of Solidity. Changes for newer Solidity versions can be found in https://blog.soliditylang.org/category/releases/. Please consider updating the versions for the following files.
```solidity
contracts\shutdown\fuse\MerkleRedeemerDripper.sol
  2: pragma solidity =0.8.10;

contracts\shutdown\fuse\RariMerkleRedeemer.sol
  2: pragma solidity =0.8.10;
```