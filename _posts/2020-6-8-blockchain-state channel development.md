---
layout: post
comments: true
categories: blockchain
---
## State Channel Scenario
* Ben sells movie stream, Joo consumes movie stream with payment
1. Joo's payment is too slow and costly on the layer 1 blockchain
2. so Ben and Joo starts using 'state channel'
3. Ben and Joo agree to open a state channel. By doing this, they agree to a channelId, an initial state, and a set of rules.
4. in this case, initial balance { Joo: 10, Ben: 0 }, update rules: Joo can send 10 coins, but Ben can't unilaterally take coins from alice.
5. Joo deposits the coins into an 'asset holder', where they are held specifically for Joo and Ben's channel. But Ben doesn't need to send any coins because his channel balance starts out at 0 coins.
6. Joo send an pay 1 coin, updated state, where the balances are now {Joo: 9, Ben: 1}. For an update to be valid, it must conform to the update rules.
7. they makes several transactions and finished payments, they submit the final state of the channel to a smart contract called the 'adjudicator'
8. once the outcome is finalized, the adjudicator sends it to another contract called the asset holder, which releases coins back to Joo and Ben accordingly

![diagram](/static/img/blockchain state channel.png)

* Difficulty of state channel - one of participants might block or go offline, and small but residual cost
1. channel advances and ends in a finite time
2. channel closes in latest state

therefore, we must define
1. how to advance the state
2. what to do if co-party goes offline
3. how to finalize the outcome on-chain
