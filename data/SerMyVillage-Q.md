https://github.com/code-423n4/2022-09-tribe/blob/769b0586b4975270b669d7d1581aa5672d6999d5/contracts/dao/nopeDAO/NopeDAO.sol#L27-L32

```
GovernorSettings(
   0, /* 0 blocks */

    26585,  /* 4 days measured in blocks. Assumed 13s block time */            0

```

`Low risk` time-attack vector which wrongly assumes a constant block-time. Malicious nodes and miners can collude to arbitrarily extend the voting time to present a batch vote/signal error from a front-end differential.

For proper calculations--

`Suggestion`: Change on a consistent basis to represent a twap of the amount of blocks per a time period (e.g. past 7 days). 

`Analysis`: Although this is an unlikely attack vector and only usable in extremely niche scenarios; it is common practice to not use hard-coded values, even in the constructor. 