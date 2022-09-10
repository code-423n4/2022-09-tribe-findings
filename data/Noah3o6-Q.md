-> EVENT IS MISSING INDEXED FIELDS
Each event should use three indexed fields if there are three or more fields.

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#:~:text=event%20Redeem(address%20to%2C%20uint256%20amountFeiIn%2C%20uint256%20amountAssetOut)%3B
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#:~:text=event%20Mint(address%20to%2C%20uint256%20amountIn%2C%20uint256%20amountFeiOut)%3B
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#:~:text=event%20Redeemed(address%20indexed%20owner%2C%20address%20indexed%20receiver%2C%20uint256%20amount%2C%20uint256%20base)%3B


-> _SAFEMINT() SHOULD BE USED RATHER THAN _MINT() WHEREVER POSSIBLE

_mint() is discouraged in favor of _safeMint() which ensures that the recipient is either an EOA or implements IERC721Receiver

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#:~:text=FEI.-,mint,-(to%2C%20amountFeiOut)%3B
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#:~:text=function-,mint,-(


->USE A MORE RECENT VERSION OF SOLIDITY

Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value

https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/shutdown/redeem/TribeRedeemer.sol#:~:text=pragma%20solidity%20%5E0.8.4%3B
https://github.com/code-423n4/2022-09-tribe/blob/main/contracts/peg/SimpleFeiDaiPSM.sol#:~:text=pragma%20solidity%20%5E0.8.4%3B
