## IMMUTABLE ADDRESSES LACK ZERO-ADDRESS CHECK

Constructors should check the address written in an immutable address variable is not the zero address.

*There is 1 instance of this issue:*

```solidity
File: contracts/shutdown/redeem/TribeRedeemer.sol
32:   redeemedToken = _redeemedToken;
```

### Mitigation

Add a zero address check for each variables.

---

## CHECK ZERO DENOMINATOR

When a division is computed, it must be ensured that the denominator is non-zero to prevent failure of the function call.

*There is 1 instance of this issue:*

```solidity
File: contracts/shutdown/redeem/TribeRedeemer.sol
58:   uint256 redeemedAmount = (amountIn * balance) / base;
```

### Mitigation

Add a zero check for `base` variable.

---

## **PRAGMA VERSION**

In the contracts, floating pragmas should not be used. Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Proof of Concept

[https://swcregistry.io/docs/SWC-103](https://swcregistry.io/docs/SWC-103)

```solidity
contracts/peg/SimpleFeiDaiPSM.sol
contracts/shutdown/fuse/RariMerkleRedeemer.sol
contracts/shutdown/fuse/MerkleRedeemerDripper.sol
contracts/shutdown/redeem/TribeRedeemer.sol
```

### Mitigation

Lock the pragma version and use the latest version (0.8.17)