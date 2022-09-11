## 1. No modifier ``hasNotSigned`` in ``RariMerkleRedeemer.signAndClaim()``

## 2. How to update ``RariMerkleRedeemer``
### Desctiption
You are deploying ``RariMerkleRedeemer`` with ``merkleRoots``, representing current state
But then new users can deposit funds to your pools. How can they get their money back? Should they wait before you will deploy another ``RariMerkleRedeemer`` with new state (merkleRoots)?
