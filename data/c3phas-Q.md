
## QA

### Missing checks for address(0x0) when assigning values to address state variables
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L32
```solidity
File: /contracts/shutdown/redeem/TribeRedeemer.sol
32:        redeemedToken = _redeemedToken;
```

### Code implementation difficult to understand vis-à-vis the comment given (Refactor the code to do exactly how the comment puts it)

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L236-L240

```solidity
File: /contracts/shutdown/fuse/RariMerkleRedeemer.sol
236:            // check: amount is less than or equal to the user's claimableAmount-claimedAmount for this cToken
237:            require(
238:                redemptions[msg.sender][cTokens[i]] + cTokenAmounts[i] <= claims[msg.sender][cTokens[i]],
239:                "Amount exceeds available remaining claim."
240:            );
			
```

As much as the operation `redemptions[msg.sender][cTokens[i]] + cTokenAmounts[i] <= claims[msg.sender][cTokens[i]]` and `  cTokenAmounts[i] <= claims[msg.sender][cTokens[i]] - redemptions[msg.sender][cTokens[i]]`  evaluate to the same thing, it's perhaps easier to understand what's happening on the second one as the comment clearly makes use of the second operation.

We can also choose to modify the comment as follows
```solidity
236:  // check:   claimedAmount + amount is less than or equal to the user's claimableAmount for this cToken
```

### Constants should be defined rather than using magic numbers

There are several occurrences of literal values with unexplained meaning .Literal values in the codebase without an explained meaning make the code harder to read, understand and maintain, thus hindering the experience of developers, auditors and external contributors alike.

Developers should define a constant variable for every magic value used , giving it a clear and self-explanatory name. 

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L85
```solidity
File: /contracts/shutdown/fuse/RariMerkleRedeemer.sol

//@audit: 1e18
85:        return (cTokenExchangeRates[cToken] * amount) / 1e18;

//@audit: 27
125:        require(_cTokens.length == 27, "Must provide exactly 27 exchange rates.");

//@audit: 1e10
130:                _exchangeRates[i] > 1e10,

//@audit: 27
138:        require(_cTokens.length == 27, "Must provide exactly 27 merkle roots");
```

### Lock pragmas to specific compiler version
Contracts should be deployed with the same compiler version and flags that they have been tested the most with. Locking the pragma helps ensure that contracts do not accidentally get deployed using, for example, the latest compiler which may have higher risks of undiscovered bugs. Contracts may also be deployed by others and the pragma indicates the compiler version intended by the original authors.

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L2
```solidity
File: /contracts/peg/SimpleFeiDaiPSM.sol
2:   pragma solidity ^0.8.4;
```

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L2
```solidity
File: /contracts/shutdown/redeem/TribeRedeemer.sol
2:   pragma solidity ^0.8.4;
```

### Unused named return
Using both named returns and a return statement isn’t necessary in  a function.To  improve code quality, consider using only one of those.

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L81-L86
```solidity
File: /contracts/shutdown/fuse/RariMerkleRedeemer.sol
81:    function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {
82:        // Each ctoken exchange rate is stored as how much you should get for 1e18 of the particular cToken
83:        // Thus, we divide by 1e18 when returning the amount that a person should get when they provide
84:        // the amount of cTokens they're turning into the contract
85:        return (cTokenExchangeRates[cToken] * amount) / 1e18;
86:    }
```

### Typos/Grammer
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L163
```solidity
File: /contracts/shutdown/fuse/RariMerkleRedeemer.sol

//@audit: User provides **the the** cToken -> remove one **the**
163:   // User provides the the cToken & the amount they should get, and it is verified against the merkle root for that cToken

//@audit: user's claim amount **int he** - should be user's claim amount **in the**
164:    /// Should set the user's claim amount int he claims mapping for the provided cToken

```

### Lack of indexed values in events
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L26-L29
```solidity
File: /contracts/peg/SimpleFeiDaiPSM.sol
26:    /// @notice event emitted upon a redemption
27:    event Redeem(address to, uint256 amountFeiIn, uint256 amountAssetOut);
28:    /// @notice event emitted when fei gets minted
29:    event Mint(address to, uint256 amountIn, uint256 amountFeiOut);
```

### Use Uppercase names for constant variables
Constants should be named with all capital letters with underscores separating words. 
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L75
```solidity
File: /contracts/peg/SimpleFeiDaiPSM.sol
75:    address public constant balanceReportedIn = address(DAI);
```

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L92-L98
```solidity
File: /contracts/peg/SimpleFeiDaiPSM.sol
92:    uint256 public constant mintFeeBasisPoints = 0;
93:    uint256 public constant redeemFeeBasisPoints = 0;
94:    address public constant underlyingToken = address(DAI);
95:    uint256 public constant getMaxMintAmountOut = type(uint256).max;
96:    bool public constant paused = false;
97:    bool public constant redeemPaused = false;
98:    bool public constant mintPaused = false;
```
### Avoid contract existence checks by using solidity version 0.8.10 or later

Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L106
```solidity
File: /contracts/peg/SimpleFeiDaiPSM.sol
106:        uint256 feiBalance = FEI.balanceOf(address(this));
```
The file [SimpleFeiDaiPSM.sol](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol) uses a compiler version 0.8.4

### Natspec is incomplete
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L33-L37
```solidity
File: /contracts/peg/SimpleFeiDaiPSM.sol

//@audit: Missing @param to, amountIn, minAmountOut and @return amountFeiOut
33:    function mint(
34:        address to,
35:        uint256 amountIn,
36:        uint256 minAmountOut
37:    ) external returns (uint256 amountFeiOut) {
```


https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/peg/SimpleFeiDaiPSM.sol#L48-L52
```solidity
File: /contracts/peg/SimpleFeiDaiPSM.sol

//@audit: Missing @param to, amountFeiIn, minAmountOut and @return amountOut
48:    function redeem(
49:        address to,
50:        uint256 amountFeiIn,
51:        uint256 minAmountOut
52:    ) external returns (uint256 amountOut) {
```


https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L64
```solidity
File: /contracts/shutdown/redeem/TribeRedeemer.sol

//@audit: Missing @param amountIn, @return tokens, @return amountsOut
44:    function previewRedeem(uint256 amountIn)
45:        public
46:        view
47:        returns (address[] memory tokens, uint256[] memory amountsOut)


//@audit: Missing @param to,@param amountIn
64:    function redeem(address to, uint256 amountIn) external nonReentrant {
```

## FIle is Missing Natspec comments
https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol

It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI).
NatSpec includes the formatting for comments that the smart contract author will use, and which are understood by the Solidity compiler. Also detailed below is output of the Solidity compiler, which extracts these comments into a machine-readable format.

External functions in this file should make use of this comments

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L48
```solidity
File: /contracts/shutdown/fuse/RariMerkleRedeemer.sol

48:    function sign(bytes calldata signature) external override hasNotSigned nonReentrant {

52:    function claim(
53:        address _cToken,
54:        uint256 _amount,
55:        bytes32[] calldata _merkleProof
56:    ) external override hasSigned nonReentrant {


60:    function multiClaim(
61:        address[] calldata _cTokens,
62:        uint256[] calldata _amounts,
63:        bytes32[][] calldata _merkleProofs
64:    ) external override hasSigned nonReentrant {


68:     function redeem(address cToken, uint256 cTokenAmount) external override hasSigned nonReentrant {

72:    function multiRedeem(address[] calldata cTokens, uint256[] calldata cTokenAmounts)

81:    function previewRedeem(address cToken, uint256 amount) public view override returns (uint256 baseTokenAmount) {


88:    function signAndClaim(
89:        bytes calldata signature,
90:        address[] calldata cTokens,
91:        uint256[] calldata amounts,
92:        bytes32[][] calldata merkleProofs
93:    ) external override nonReentrant {


99:    function claimAndRedeem(
100:        address[] calldata cTokens,
101:        uint256[] calldata amounts,
102:        bytes32[][] calldata merkleProofs
103:    ) external hasSigned nonReentrant {


108:    function signAndClaimAndRedeem(
109:        bytes calldata signature,
110:        address[] calldata cTokens,
111:        uint256[] calldata amountsToClaim,
112:        uint256[] calldata amountsToRedeem,
113:        bytes32[][] calldata merkleProofs
114:    ) external override hasNotSigned nonReentrant {
```


### The nonReentrant modifier should occur before all other modifiers
This is a best-practice to protect against reentrancy in other modifiers

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L48
```solidity
File: /contracts/shutdown/fuse/RariMerkleRedeemer.sol

48:     function sign(bytes calldata signature) external override hasNotSigned nonReentrant {



52:    function claim(
53:        address _cToken,
54:        uint256 _amount,
55:        bytes32[] calldata _merkleProof
56:    ) external override hasSigned nonReentrant {
	


60:    function multiClaim(
61:        address[] calldata _cTokens,
62:        uint256[] calldata _amounts,
63:        bytes32[][] calldata _merkleProofs
64:    ) external override hasSigned nonReentrant {


68:    function redeem(address cToken, uint256 cTokenAmount) external override hasSigned nonReentrant {


72:    function multiRedeem(address[] calldata cTokens, uint256[] calldata cTokenAmounts)
73:        external
74:        override
75:        hasSigned
76:        nonReentrant
77:    {
	
	
99:    function claimAndRedeem(
100:        address[] calldata cTokens,
101:        uint256[] calldata amounts,
102:        bytes32[][] calldata merkleProofs
103:    ) external hasSigned nonReentrant {	



108:    function signAndClaimAndRedeem(
109:        bytes calldata signature,
110:        address[] calldata cTokens,
111:        uint256[] calldata amountsToClaim,
112:        uint256[] calldata amountsToRedeem,
113:        bytes32[][] calldata merkleProofs
114:    ) external override hasNotSigned nonReentrant {
```

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L42-L47
```solidity
File: /contracts/shutdown/redeem/TribeRedeemer.sol
//@audit: Missing @param amountIn,@return tokens,amountsOut
44:    function previewRedeem(uint256 amountIn)
45:        public
46:        view
47:        returns (address[] memory tokens, uint256[] memory amountsOut)


//@audit: Missing @param to,@param amountIn
64:    function redeem(address to, uint256 amountIn) external nonReentrant {
```

### Use a more recent version of solidity

Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings
Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
Use a solidity version of at least 0.8.13 to get the ability to use using for with a list of free functions


https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L2
```solidity
2: pragma solidity ^0.8.4;
```


### For loops
As for the loops my suggestions are as follows.

General structure used:
```solidity
for (uint256 i = 0; CONDITION; ++i) { ... }
```

Change to :
```solidity
uint256 i = 0;

for (; CONDITION;) { 
    ...
    unchecked {
         ++i;
    } 
}
```

eg. The [Line 128](https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L128) can be modified to the following
```solidity
uint256 i = 0;

for (; i < _cTokens.length;) { 
    ...
    unchecked {
         ++i;
    } 
}
```

**Instances**

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/fuse/RariMerkleRedeemer.sol#L128
```solidity
File: /contracts/shutdown/fuse/RariMerkleRedeemer.sol
128:        for (uint256 i = 0; i < _cTokens.length; i++) {

141:        for (uint256 i = 0; i < _cTokens.length; i++) {

193:        for (uint256 i = 0; i < _cTokens.length; i++) {

229:        for (uint256 i = 0; i < cTokens.length; i++) {

247:        for (uint256 i = 0; i < cTokens.length; i++) {
```

https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/shutdown/redeem/TribeRedeemer.sol#L53
```solidity
FIle: /contracts/shutdown/redeem/TribeRedeemer.sol
53:        for (uint256 i = 0; i < tokensReceived.length; i++) {

71:        for (uint256 i = 0; i < tokens.length; i++) {
```