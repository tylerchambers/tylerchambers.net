---
title: Why I don't like BIP-47.
description: BIP-47 (and paynyms) are popular for creating reusable payment codes. However, there are some often overlooked privacy footguns that make them not worth it.
date: 2023-01-14
tags:
  - Bitcoin
  - BIP-37
  - Crypto
layout: layouts/post.njk
---

BIP-47 outlines a process by which bitcoin users can generate Reusable payment codes for use with HD Wallets. Created in December 2015, this BIP was marked as "Unanimously Discourage for implementation" by core maintainers and was widely ignored by the bitcoin community until Samurai Wallet implemented the BIP as a part of its PayNym system.

Many bitcoiners praise the PayNym implementation and reusable payment codes generally for their ease of use and apparent privacy benefits. However, BIP-47 is fundamentally unscalable, overly complex, and riddled with non-obvious privacy footguns.

PayNyms and BIP-47 reusable payment codes are often prescribed as the solution to the following problem:

You want to receive payments from someone (possibly multiple people). Still, you don't want to hand out a single bitcoin address because you are privacy conscious and know that receiving multiple payments to a public address is bad for opsec.

The proposed solution is to download a wallet that supports generating BIP-47 reusable payment codes and possibly registering on Samurai Wallet's PayNym service.

This way, individuals looking to send you sats can generate a unique address using your published code and send you the funds.

Here are the problems with that approach:

1) If you are using BIP 47 entirely on-chain with a notification address, there is now forever an on-chain record of two parties cooperating to pay each other. Unless great care is taken by the sender and receiver to not doxx themselves with toxic change from the BIP-47 notification transaction, it forever becomes visible that you and the payee are connected somehow. While future transactions and their amounts aren't visible, this metadata makes it possible to build a social graph

2) You have to share your payment code. How do you do that securely? Signal? OTP over Tor? You can't just post it online (especially without PGP signing) because verifying who owns it is impossible. 

Most commonly, payment codes are linked to a samurai PayNym. Even if you use Tor to register, you're still giving Samurai sever operators a significant identifiable for your payment code, and now they can see and log the individuals who search for your Nym (I'm not saying they do this, but they can).

Using ephemeral payment codes makes the most sense from a privacy perspective. Generate one per payee, PGP sign it (assuming you can distribute your PGP pubkey in a trusted manner), and send it out. But at that point, why not just derive an XPUB and send that instead? Or a list of addresses in batches?

3) Because all of the communication for BIP-47 is on-chain, it's fundamentally unscalable. The data storage via OP_RETURN for on-chain recovery with an HD seed is concerning enough, but the notification transactions are even worse. Notification transactions must be created for every pair of senders/receivers- so N^2 transactions for N users. Right now, on-chain payments are cheap, but these transactions quickly become prohibitively expensive in a world with significant bitcoin adoption.

4) Recovery quickly becomes messy. BIP-47 was designed with only p2pkh addresses in mind. There is no method in the standard for encoding which script type is associated with a payment code. Now that we have Segwit and Taproot as well as p2pkh, wallets have to scan for all existing standard script types to discover addresses and funds associated with a payment code. Further, you need a copy of the blockchain, at least from the creation of the first payment code, to discover, then recover the funds associated with it properly (and anonymously). The disk space required for this isn't feasible for most mobile devices. An alternative is querying a 3rd party, which carries its own (hopefully obvious) privacy concerns.

All this to say- the convenience of BIP-47 is very nice, but the benefits are marginal at best (you still have to share the payment code securely and hope you or your payee aren't doxxing yourselves on-chain). Simply sending an address, a signed list of addresses, or an XPUB + derivation path gets one the same benefit without the privacy and scalability headaches.

Hopefully, bitcoiners will come up with something better. Right now, the most promising candidate is reusable taproot addresses: https://gist.github.com/Kixunil/0ddb3a9cdec33342b97431e438252c0a.

But for now, BIP-47 should be avoided.
