# DPOS contract

## Problem:



In a PoS blockchain the rewards accrued to a validator (ie. node participating in consensus), consisting of the transaction fees and block production rewards, are distributed amongst the stakeholders who have delegated to the validator.    This is a potentially expensive operation as each block produced by consensus will accrue rewards to many validators and each of those validators may be staked by a very large number of accounts.   The fee system implemented in Taraxa is inspired by the F1 Fee Distribution proposed by Ojha and Goes ([https://drops.dagstuhl.de/opus/volltexte/2020/11974/pdf/OASIcs-Tokenomics-2019-10.pdf](https://drops.dagstuhl.de/opus/volltexte/2020/11974/pdf/OASIcs-Tokenomics-2019-10.pdf))



The fundamental concept is to avoid the expense of “payout” to the many stakeholders for each validator at every new block, and instead to only update the accrued pool of rewards for each validator.   The number of validators for each block is much smaller than the product of validator count and their many delegations.  Additionally, its already required as part of the underlying consensus to loop over all the validators.   The expensive operation of individual payout to delegators/stakeholders for a given validator is deferred until the delegations to that validator is changed; either by a deposit or withdrawal of stake.  &#x20;



Since by the above assumption the ratio of staking by each delegator doesn’t change, the pool of rewards accumulated between payouts can be distributed easily during a payout without having to iterate/sum over all the blocks between payouts.   Indeed instead of thinking of payouts per block we can think of payouts from a given validator as occurring between pay-periods.



Commission for validator is simply subtracted at each block update and paid out to validator,  the rest is put into a growing pool for the current period (of blocks) between changes in validator’s stake.   Additional considerations such as slashing a easily handled, are outlined in the paper by Ojha and Goes, and will be implemented in future updates.



### Definitions:

Validator = node (runner) participating in in consensus

Delegator = money person staking to validator

Pay-Period = interval (of blocks) between changes to a validators staked amount (either by deposit or withdrawal)&#x20;

## Math:

\
When a delegator wants to change their stake to a validator a new entry for the validator is created to mark the start of the new pay-period, f, defined as:

Entryf = Entryf-1 + Tf / nf  &#x20;

Where Tf  is the amount of rewards the validator has accrued in the pay-period, f, and nf is the validators total amount of stake delegated to it in pay-period f.

The payout to a withdrawer who entered at pay-period k, and is withdrawing at pay-period f then becomes simply:

Payout = x ( Entryf, - Entryk)

Where x is the amount of stake delegated to the validator by the withdrawing delegator. &#x20;
