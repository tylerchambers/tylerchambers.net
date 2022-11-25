---
title: The Mempool as a Collective Hallucination.
description: There is no spoon.
date: 2022-11-25
tags:
  - Bitcoin
  - Mempool
  - RBF
  - Crypto
layout: layouts/post.njk
---

To spend bitcoin, a bitcoin transaction must be built, signed, and included in a valid block. Because most bitcoiners do not mine their own blocks, they often send their transactions to specialized miners, and pay a fee to have their transaction included in a mined block.

But how do transactions get from non-mining nodes to the bitcoin miners? Bitcoin is a peer-to-peer network in which each node connects to multiple other nodes. These connections form a giant "mesh" that propagates messages across the network. A transaction is "broadcast" from an origin node to all nodes to which it is connected. Those nodes perform checks to make sure the transaction is valid, and if they pass, forward the transaction on to their peers, and so on, until the whole network (including mining nodes) is aware of the transaction.

After the broadcast, but before being mined, the transaction gets stored in each node's "mempool."  The mempool is like a waiting room for the blockchain, where each node stores its record of unconfirmed (not-mined) transactions before it receives a message from the network informing it of a new block containing the transaction.

Historically, the bitcoin network has converged, or come close to converging, on mempool state. Most of the nodes will have close to the same contents most of the time. This convergence has led to many bitcoiners adopting the phrase "the mempool." "I just sent the transaction; it's in the mempool," "the mempool is looking pretty full," etc.

However, as explained above, there is not *a* mempool. There are *many* mempools—one for each node. More confusingly, the rules for what may be allowed into a node's mempool is "pre-consensus." The bitcoin network's consensus rules govern only the state of the blockchain. Node operators can configure their nodes to accept and relay whatever transactions they want. Bitcoin core even provides CLI flags (`maxmempool`, `mempoolexpiry`) and `bitcoin.conf` settings (`incrementalrelayfee`, `dustrelayfee`, `bytespersigop`, `datacarrier`, `datacarriersize`, `permitbaremultisig`, `minrelaytxfee`). Notably, there is the recent controversial addition of `mempoolfullrbf`.

The controversial nature of `mempoolfullrbf` has increased the popularity of mempool acceptance criteria tweaking by enthusiast node operators. Many are discovering for the first time that there is no "the mempool," but rather a whole universe of mempools, over which each operator has sovereignty.

As the collective hallucination known as "the mempool" begins to fall apart, there will be significant implications for those who have developed a dependence on pre-consensus (read: not guaranteed by the network) behavior.

If you are one of those individuals, adjust your worldview to accommodate the pluralist reality of mempool policy instead of fighting for consensus where it can't exist. Breathe in, breathe out; a new block is arriving in 10 minutes 🧘‍♂️