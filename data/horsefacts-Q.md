## Low

### Users may `mint` to the PSM contract

The project README notes that the `SimpleFeiDaiPSM` "should stay synced between the FEI and DAI supplies after each call to `burnFeiHeld()`, assuming it is seeded with enough DAI to match the circulating supply."

However, it's possible to specify the `SimpleFeiDaiPSM` contract itself as the `to` address in a call to `mint`, which will violate this assumption, leaving extra DAI locked in the contract once `burnFeiHeld()` is called to burn the contract's FEI balance:

[SimpleFeiDaiPSM.sol#L31-L43](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L31-L43)

```solidity
    /// @notice mint `amountFeiOut` FEI to address `to` for `amountIn` underlying tokens
    /// @dev see getMintAmountOut() to pre-calculate amount out
    function mint(
        address to,
        uint256 amountIn,
        uint256 minAmountOut
    ) external returns (uint256 amountFeiOut) {
        amountFeiOut = amountIn;
        require(amountFeiOut >= minAmountOut, "SimpleFeiDaiPSM: Mint not enough out");
        DAI.safeTransferFrom(msg.sender, address(this), amountIn);
        FEI.mint(to, amountFeiOut);
        emit Mint(to, amountIn, amountIn);
    }
```

Recommendation: Do not rely on the assumption that supply will remain synced between DAI and FEI in the PSM, since users may mint to the PSM or send FEI/DAI directly to the contract. Consider reverting if the `to` address is the `SimpleFeiDaiPSM` contract. (However, note that users might still transfer FEI to this contract directly, with a similar effect).

## QA

### Update temporary message

The temporary claim message used in `MultiMerkleRedeemer` should be updated before deployment.

[MultiMerkleRedeemer.sol#L52-L53](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/MultiMerkleRedeemer.sol#L52-L53)

```solidity
    /// @notice The message to be signed by any users claiming on cTokens
    string public constant MESSAGE = "Sample message, please update.";
```

### Missing event indexes

The `Redeem` and `Mint` events in `SimpleFeiDaiPSM` are missing indexes for the caller address. It's probably useful to index these parameters to make it easy to filter and query events by caller address.

[SimpleFeiDaiPSM.sol#L26-L29](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L27-L29)

```solidity
    /// @notice event emitted upon a redemption
    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
    /// @notice event emitted when fei gets minted
    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```

### Use interfaces to enforce common behavior

The `SimpleFeiDaiPSM` includes several functions meant to make it as close as possible to the interface of the `FixedPricePSM`:

[SimpleFeiDaiPSM.sol#L87-L99](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L87-L99)

```solidity
    // ----------------------------------------------------------------------------
    // These view functions are meant to make this contract's interface
    // as similar as possible to the FixedPricePSM as possible.
    // ----------------------------------------------------------------------------

    uint256 public constant mintFeeBasisPoints = 0;
    uint256 public constant redeemFeeBasisPoints = 0;
    address public constant underlyingToken = address(DAI);
    uint256 public constant getMaxMintAmountOut = type(uint256).max;
    bool public constant paused = false;
    bool public constant redeemPaused = false;
    bool public constant mintPaused = false;
```

However, these are not enforced at compile time. Consider extracting and using a Solidity interface to define these common methods and enforce this behavior. This is a safer pattern, since the compiler will check that the necessary methods are implemented.

### Unused imports

The contract `CoreRef.sol` is imported but unused in `RariMerkleRedeemer`.

[RariMerkleRedeemer.sol#L4-L5](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L4-L5)

```solidity
import "../../refs/CoreRef.sol";
```
