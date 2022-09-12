1.

Contract: SimpleFeiDaiPSM.sol

	The word "governanceless" in line 7 is not a globally recognizable English word.

Recommendation:

	Consider replacing with the phrase: "governless" or "ungoverned"
	
2.

Contract: SimpleFeiDaiPSM.sol

Code can be refactored to be more readable.

Consider refactoring variables, functions and phrasing in comments

amountFeiIn to depositedFei, 

amountAssetOut to withdrawnAsset, 

amountIn to deposited, 

amountFeiOut to withdrawnFei, 

getMintAmountOut() to getMintWithdrawn(), 

"amount out" to "withdrawn", 

minAmountOut to minWithdrawal, 

getRedeemAmountOut() to getRedeemWithdrawn(), 

accordingly to recommendation for potentially better user readability.

Recommendation:

line 27: event Redeem(address to, uint256 depositedFei, uint256 withdrawnAsset);

line 29: event Mint(address to, uint256 deposited, uint256 withdrawnFei);

line 31: /// @notice mint `withdrawnFei` FEI to address `to` for `deposited` underlying tokens

line 32: /// @dev see getMintWithdrawn() to pre-calculate withdrawn

line 35: uint256 deposited,

line 36: uint256 minWithdrawal

line 37: ) external returns (uint256 withdrawnFei) {

line 38: withdrawnFei = deposited;

line 39: require(withdrawnFei >= minWithdrawal, "SimpleFeiDaiPSM: Mint not enough out");

line 40: DAI.safeTransferFrom(msg.sender, address(this), deposited);

line 41: FEI.mint(to, withdrawnFei);

line 42: emit Mint(to, deposited, deposited);

line 45: /// @notice redeem `depositedFei` FEI for `amountOut` underlying tokens and send to address `to`

line 46: /// @dev see getRedeemWithdrawn() to pre-calculate amount out

line 50: uint256 depositedFei,

line 51: uint256 minWithdrawal

line 52: ) external returns (uint256 amountOut) {

line 53: amountOut = depositedFei;

line 54: require(amountOut >= minWithdrawal, "SimpleFeiDaiPSM: Redeem not enough out");

line 55: FEI.safeTransferFrom(msg.sender, address(this), depositedFei);

line 56: DAI.safeTransfer(to, amountOut);

line 57: emit Redeem(to, depositedFei, depositedFei);

line 60: /// @notice calculate the amount of FEI out for a given `deposited` of underlying

line 61: function getMintWithdrawn(uint256 deposited) external pure returns (uint256) {

line 62: return deposited;

line 65: /// @notice calculate the amount of FEI out for a given `deposited` of underlying

line 66: function getRedeemWithdrawn(uint256 deposited) external pure returns (uint256) {

line 67: return deposited;

This will also bring consistency to the code in respect to the PCVDeposit interface.
	
3. 

Cotract: RariMerkleRedeemer.sol

It is best practice and unnecessary to initialize for variables as they get set to 0 by default in:

	line 128
	line 141
	line 193
	line 229
	line 247
	
Recommendation:

	for (uint256 i; i < _cTokens.length; i++) {
	
4.

Contract: TribeRedeemer.sol

It is best practice and unnecessary to initialize for variables as they get set to 0 by default in:

	line 53
	line 71

Recommendation:

	for (uint256 i; i < tokensReceived.length; i++) {
	for (uint256 i; i < tokens.length; i++) {