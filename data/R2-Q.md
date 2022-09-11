## 1. No modifier ``hasNotSigned`` in ``RariMerkleRedeemer.signAndClaim()``

## 2. How to update ``RariMerkleRedeemer``
### Desctiption
You are deploying ``RariMerkleRedeemer`` with ``merkleRoots``, representing current state
But then new users can deposit funds to your pools. How can they get their money back? Should they wait before you will deploy another ``RariMerkleRedeemer`` with new state (merkleRoots)?

## 3. No checks that all tokens in ``TribeRedeemer`` have the same ``decimals()``
### Desctiption
Decimals for ``redeemedToken`` and each of ``tokensReceived`` may be different. It will break contract logic. So check it in constructor

## 4. No checks that tokens in ``TribeRedeemer`` are not address(0)

## 5. Check redeemedToken, do not use ``amountIn``
If you will use deflationary token or fee token as a ``redeemedToken``, your logic will fail
In ``TribeRedeemer.redeem()`` you are using ``amountIn`` to calculate ``previewRedeem()``
But on your balance you may receive tokens less then ``amountIn``



