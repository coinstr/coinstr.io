---
title: 🔥 Coinstr vs Fireblocks Comparison
date : 2023-02-13
tags:
  - business
slug: vs-fireblocks
authors: Max Gravitt
---
<head>
  <title>Coinstr vs Fireblocks Comparison</title>
  <meta charSet="utf-8" />
  <meta property="og:image" content="https://coinstr.app/coinstr-mindmap.png" />
  <meta property="og:description" content="Detailed head-to-head comparison between Hashed Network and Fireblocks" />
  <meta property="og:title" content="Coinstr vs Fireblocks Comparison" />
  <meta property="og:url" content="https://coinstr.app/articles/vs-fireblocks" />
</head>

## What is Fireblocks? 

> "Fireblocks is an easy to use platform to create new blockchain based products, and manage day-to-day digital asset operations." 
> -- Fireblocks.com

The Fireblocks platform has a large presence and the general use case so similar, it serves as a helpful comparison to Coinstr.

Fireblocks is a commercial, closed-source platform for handling multisignature transactions, generating receiving addresses through a verified business network, and interfacing to Defi applications. It allows organizations' administrators to add and manage users and configure corresponding spending permissions. See more about their [policy engine](https://www.fireblocks.com/platforms/governance-%e2%80%a8policy-engine/).

There is a corresponding mobile application for approving transactions. This app receives push notifications when a spend requires a user's approval, and then they are able to approve directly in the app.

The platform supports an [Integrations network](https://www.fireblocks.com/integrations) that provides a wide breadth of compatibility and access to other service providers, including accounting/tax experts, AML partners, FIAT ramps, etc. 

A secure oracle application can run on an Intel SGX server and sign programmatically based on custom criteria.

# Multiparty Computation (MPC) as the secret sauce
Multiparty computation is an off-chain cryptographic algorithm that allows m-of-n key shares to be combined to produce the private key required for the transaction. The independent nature of the algorithm means that it is compatible with any secret and thus any blockchain. 

MPC does not expose signers on chain, but the data is persisted within the Fireblocks systems. 

# How secure is Fireblocks? 
This is unknown because the platform is closed-source.

# Coinstr Bitcoin Orchestration

{{< plantuml id="au" >}}
@startmindmap
+[#orange] Coinstr
++[#yellow] Coin/UTXO Control
++[#lightgreen] Send Signature Requests to Users
++[#yellow] Receive Signatures
++[#yellow] Broadcast Transactions
--[#lightgreen] Bitcoin Wallet
--[#lightgreen] Build Spending Policies
--[#yellow] Apply Spending Policies to Users
--[#lightgreen] Create Spending Proposals
@endmindmap{{< /plantuml >}}


## Key Similarities
### Direct and Self Custody
Both Fireblocks and Coinstr encourage [direct custody](https://drive.google.com/file/d/1YJwp0TtCO8HUHuKfl7OFB51M8j9WYhKR/view?usp=sharing) or self custody. Many businesses use centralized exchanges to custody their digital assets, meaning that they are dependent on that custodian for safety and withdrawals. However, the trend is for businesses to move their assets off of exchanges, especially after the collapse of FTX.

### Multisignature and Governance 
Both platforms support the ability for administrators to configure business rules for spending and handling assets.

### Verified Network of Businesses
Both platforms support a network of businesses with verified identities. Although the digital assets flow peer-to-peer on the native networks, verified profiles and dynamic address generation make remittance more secure and frictionless. 

Fireblocks uses their own propreitary identity network while Coinstr uses Nostr.

## Key Differences
### Administrator Centralization
As a commercial application, each of Fireblocks customers have a singular administrator account. This is the account that has ultimate control over the activities of users under that administrator's purview. 

Coinstr can be used with decentralized groups without a single administrator. 

### Asset Focus
Fireblocks supports over 40 protocols and what seems to be [thousands of tokens](https://www.fireblocks.com/integrations/tokens/). 

Coinstr is only for Bitcoin, but plans to also support USD stablecoins on using Lightning's [Taro protocol](https://docs.lightning.engineering/the-lightning-network/taro).

### Cryptography
Fireblocks uses closed source multiparty computation orchestrated via a closed source platform. MPC transactions cannot be executed with cold wallets. 

Coinstr uses [BIP 174](https://en.bitcoin.it/wiki/BIP_0174), and the taproot upgrade, [BIP 340](https://en.bitcoin.it/wiki/BIP_0340), [BIP 341](https://en.bitcoin.it/wiki/BIP_0341), and [BIP 342](https://en.bitcoin.it/wiki/BIP_0342). These are Bitcoin Core protocols, and thus signing has wide support in variety of hot and cold signers. 

Users may sign with their Nostr clients or use Bitcoin-only signers such as mobile apps and hardware wallets.

### Technology
Fireblocks is closed source so much is unknown.

Coinstr is a mash-up of several existing, well-known open source projects. 
- Nostr
- BitcoinDevKit
- BDK's Elephant
- Google's Blockly
- Rust Miniscript

## Side by Side Comparison Table
 |Category|Fireblocks|Coinstr|
    |---|---|---|
    |Technology|Propreitary|Substrate-based|
    |Cryptography|Multiparty Computation (MPC)|Bitcoin Core [BIP 174](https://en.bitcoin.it/wiki/BIP_0174), and the taproot upgrade, [BIP 340](https://en.bitcoin.it/wiki/BIP_0340), [BIP 341](https://en.bitcoin.it/wiki/BIP_0341), and [BIP 342](https://en.bitcoin.it/wiki/BIP_0342)|
    |Asset Types|44 protocols and thousands of tokens|Bitcoin, selected USD stablecoins, and DOT-native|
    |User-facing Apps|Web app and mobile app for 2FA approvals|Web app and mobile app signer|
    |Compatible Signers|Only Fireblocks apps|Any Bitcoin or Nostr signer, including hardware and air-gapped signers|
    |Verifiable Receiving Addresses|No|Yes|
    |Central Admin Required|Yes|No|
    |Decentralizable|No|Yes|
    |License|Propreitary/Closed|[Open-Source](https://github.com/3yekn/coinstr)|
    |Starting Price|$10,000 per month|TBD - seeking partners - contact below|
    |Valuation|[$ 8 B](https://techcrunch.com/2022/01/27/crypto-infrastructure-company-fireblocks-nearly-quadruples-valuation-to-8b-in-six-months/?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAADVcB8hiYZN0UpSb41kVicCAHlfYQsXfkot5DgeJ66zJMhNCEp9J2YXOd6DgCuYgYV4wlT8chccxuyzftcUO4jYWcKRdVwECvxuXXpqTsz7oGAwA2NucZdnpUjPLwkcDozFysEW-3BxaP03pUcmjC-xe58RMUwLrGnMlAbysnRZd)|Contact Below|
    |Revenue|$100 M+|Pre-revenue 😅|

## Contact
To learn more about Coinstr, contact via the [web form](https://maxgravitt.com/contact/) or [Nostr](https://snort.social/p/npub1ws2t95pdtpna4ps62rrz75mm6ujsudjv70yj2jk4wsqjhedlw22qsqwew9)