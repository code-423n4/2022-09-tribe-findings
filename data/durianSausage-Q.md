## none-critical issue foundings
### N01: EVENT IS MISSING INDEXED FIELDS
#### prof
peg/SimpleFeiDaiPSM.sol, 27, b'    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);'
peg/SimpleFeiDaiPSM.sol, 29, b'    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
'
### N02: inconsistent solidity version usage.
shutdown/fuse/MerkleRedeemerDripper.sol, 2, b'pragma solidity =0.8.10;'
shutdown/fuse/RariMerkleRedeemer.sol, 2, b'pragma solidity =0.8.10;'
peg/SimpleFeiDaiPSM.sol, 2, b'pragma solidity ^0.8.4;'
shutdown/redeem/TribeRedeemer.sol, 2, b'pragma solidity ^0.8.4;'


## Low risk
### L01: RETURN VALUES OF TRANSFER()/TRANSFERFROM() NOT CHECKED
#### prof
TRANSFER()/TRANSFERFROM() NOT CHECKED
Not all IERC20 implementations revert() when there’s a failure in transfer()/transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment
#### problem
shutdown/fuse/RariMerkleRedeemer.sol, 217, b'        IERC20(cToken).safeTransferFrom(msg.sender, address(this), cTokenAmount);'
shutdown/fuse/RariMerkleRedeemer.sol, 218, b'        IERC20(baseToken).safeTransfer(msg.sender, baseTokenAmountReceived);'
shutdown/fuse/RariMerkleRedeemer.sol, 249, b'            IERC20(cTokens[i]).safeTransferFrom(msg.sender, address(this), cTokenAmounts[i]);'
shutdown/fuse/RariMerkleRedeemer.sol, 250, b'            IERC20(baseToken).safeTransfer(msg.sender, baseTokenAmountReceived);'
peg/SimpleFeiDaiPSM.sol, 40, b'        DAI.safeTransferFrom(msg.sender, address(this), amountIn);'
peg/SimpleFeiDaiPSM.sol, 55, b'        FEI.safeTransferFrom(msg.sender, address(this), amountFeiIn);'
peg/SimpleFeiDaiPSM.sol, 56, b'        DAI.safeTransfer(to, amountOut);'
shutdown/redeem/TribeRedeemer.sol, 65, b'        IERC20(redeemedToken).safeTransferFrom(msg.sender, address(this), amountIn);'
shutdown/redeem/TribeRedeemer.sol, 72, b'            IERC20(tokens[i]).safeTransfer(to, amountsOut[i]);'
risk discourage_safeTransfer
shutdown/fuse/RariMerkleRedeemer.sol, 218, b'        IERC20(baseToken).safeTransfer(msg.sender, baseTokenAmountReceived);'
shutdown/fuse/RariMerkleRedeemer.sol, 250, b'            IERC20(baseToken).safeTransfer(msg.sender, baseTokenAmountReceived);'
peg/SimpleFeiDaiPSM.sol, 56, b'        DAI.safeTransfer(to, amountOut);'
shutdown/redeem/TribeRedeemer.sol, 72, b'            IERC20(tokens[i]).safeTransfer(to, amountsOut[i]);'


### L02: INPUT ARRAY LENGTHS MAY DIFFER
#### problem
If the caller makes a copy-paste error, the lengths may be mismatchd and an operation believed to have been completed may not in fact have been completed
function withdrawMultipleERC721(address[] memory _tokens, uint256[] memory _tokenId, address _to) external override {
#### prof
shutdown/fuse/RariMerkleRedeemer.sol, 66, b'    function multiClaim(\n        address[] calldata _cTokens,\n        uint256[] calldata _amounts,\n        bytes32[][] calldata _merkleProofs\n    ) external override hasSigned nonReentrant '
shutdown/fuse/RariMerkleRedeemer.sol, 79, b'    function multiRedeem(address[] calldata cTokens, uint256[] calldata cTokenAmounts)\n        external\n        override\n        hasSigned\n        nonReentrant\n    '
shutdown/fuse/RariMerkleRedeemer.sol, 97, b'    function signAndClaim(\n        bytes calldata signature,\n        address[] calldata cTokens,\n        uint256[] calldata amounts,\n        bytes32[][] calldata merkleProofs\n    ) external override nonReentrant '
shutdown/fuse/RariMerkleRedeemer.sol, 106, b'    function claimAndRedeem(\n        address[] calldata cTokens,\n        uint256[] calldata amounts,\n        bytes32[][] calldata merkleProofs\n    ) external hasSigned nonReentrant '
shutdown/fuse/RariMerkleRedeemer.sol, 118, b'    function signAndClaimAndRedeem(\n        bytes calldata signature,\n        address[] calldata cTokens,\n        uint256[] calldata amountsToClaim,\n        uint256[] calldata amountsToRedeem,\n        bytes32[][] calldata merkleProofs\n    ) external override hasNotSigned nonReentrant '


### L03: address variable should check if it is zero MISSING CHECKS FOR ADDRESS(0X0) WHEN ASSIGNING VALUES TO ADDRESS STATE VARIABLES
#### prof
shutdown/redeem/TribeRedeemer.sol, 32, b'        redeemedToken = _redeemedToken;'
shutdown/redeem/TribeRedeemer.sol, 33, b'        tokensReceived = _tokensReceived;'


### L04: _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE
#### problem
_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver. Both OpenZeppelin and solmate have versions of this function
#### prof
SimpleFeiDaiPSM.sol,41,FEI.mint(to, amountFeiOut);