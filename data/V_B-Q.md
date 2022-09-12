### 1. MESSAGE constant
There is `sign` function in `RariMerkleRedeemer`. It accepts signature as a parameter and checks it is validity. Specifically, the function performs such checks.

```
require(ECDSA.recover(MESSAGE_HASH, _signature) == msg.sender, "Signature not valid");
```

According to the code, 

```
/// @notice The message to be signed by any users claiming on cTokens
string public constant MESSAGE = "Sample message, please update.";

/// @notice The hash of the message to be signed by any users claiming on cTokens
bytes32 public MESSAGE_HASH = ECDSA.toEthSignedMessageHash(bytes(MESSAGE));
```

So, the message that the user signs is `Sample message, please update.`, seems like it should be changed to another message with useful information.

### 2. Redeem and Mint events

There is `Redeem` and `Mint` events in `SimpleFeiDaiPSM`.

```
/// @notice event emitted upon a redemption
event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
/// @notice event emitted when fei gets minted
event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```

It reasonable to make `to` parameter as `indexed`.

Also, in `TribeRedeemer` there is an event with analogical indexed variable:

```
/// @notice event to track redemptions
event Redeemed(address indexed owner, address indexed receiver, uint256 amount, uint256 base);
```

### 3. Redundant hasSigned and hasNotSigned

The logic corresponding to `hasSigned` and `hasNotSigned` modifiers from `RariMerkleRedeemer` contract is redundant. `hasSigned` check that `msg.sender` already signed message but actally this `msg.sender` calls such a function, so he is always agree to call such logic. `hasNotSigned` also redundant it is used only before calling `_sign` function and makes no practical sense.