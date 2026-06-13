---
title: "On Cross-chain Bridges Security"
date: "2022-08-07T20:57:56+00:00"
modified: "2022-08-07T23:28:20+00:00"
slug: "on-cross-chain-bridges-security"
author: "Ahmed Lekssays"
featured_image: "../images/on-cross-chain-bridges-security/5edc642a-cody-hiscox-hp5acad1h0k-unsplash.jpg"
categories: ["Computer Science"]
tags: ["Blockchain", "Cross-chain Bridges", "Cyber Security", "DeFi"]
original_url: "https://lekssays.wordpress.com/2022/08/07/on-cross-chain-bridges-security/"
excerpt: "Blockchain and distributed ledger technology in general has been gaining a lot of traction in the last few years. The technology moves from simple cryptocurrencies transfer to more complex applications with the rise of smart contracts. The blockchain ecosystem has been growing in many directions to"
---
Blockchain and distributed ledger technology in general have been gaining a lot of traction in the last few years. The technology has moved from simple cryptocurrency transfers to more complex applications with the rise of smart contracts, and the blockchain ecosystem has been growing in many directions to keep up with the increasing demand.

## Introduction

Today there are many "blockchains" (I use quotes because some distributed ledgers do not follow the linked-list structure) that innovate to make the user experience admirable through low latency, high throughput, low fees, environmental friendliness, and more. With such innovations, a new field has emerged: DeFi, or Decentralized Finance. It aims to provide all the financial services offered by central banks in a decentralized fashion by leveraging smart contracts.

This has brought new challenges, such as exchanging assets across different blockchains. For instance, if you buy ETH (to be technically correct, it is wETH, or wrapped Ether, a concept I will discuss later in the article) on the Solana blockchain, you cannot use it directly on the Ethereum blockchain. Hence, there is a need to establish a link between the two blockchains. And yes, you guessed it right: this is what we call a cross-chain bridge. So, when a user wants to transfer assets from one blockchain to another, she sends the assets to the bridge contract on the first blockchain, and then receives them on the second blockchain, where the bridge contract locks the assets.

On a small note, each blockchain has a native token. For example, Ethereum has Ether (ETH), Solana has Solana (SOL), and Bitcoin has Bitcoin (BTC). However, to use a token of blockchain A on blockchain B, you need a wrapped token for that blockchain. For instance, to use Ether (ETH) on Solana, you need wETH (wrapped Ether). Wrapped tokens track the value of their native tokens.

## The Problem

Cross-chain bridges are one of the most attractive targets for attackers. In 2022, around 2 billion dollars were stolen across 13 different cross-chain bridge incidents, according to [Chainalysis](https://blog.chainalysis.com/reports/cross-chain-bridge-hacks-2022/). Cross-chain bridges are implemented in different ways, which can be considered one of the weaknesses that attackers exploit, since these variations present novel attack models. In addition, there is no "standard" or set of best practices to follow, as cross-chain bridges are still an undiscovered land. In their current technical implementations, cross-chain bridges present a single point of failure: the smart contract that holds the funds.

## Recent Incidents

|  |  |  |
| --- | --- | --- |
| Bridge Name | **Exploit Year** | **Stolen Funds (in $)** |
| Chainswap | 2021 | 8M |
| Poly Network | 2021 | 610M |
| Multichain/AnySwap | 2021 | 2M |
| Qubit | 2022 | 80M |
| Meter | 2022 | 4.2M |
| Wormhole | 2022 | 326M |
| Li Finance | 2022 | 600K |
| Ronin | 2022 | 624M |
| Harmony | 2022 | 97M |
| Nomad | 2022 | 190M |

*Recent cross-chain bridge attacks and their losses.*

## Vulnerabilities

The good thing about blockchains (this is a personal judgement) is that most of them are transparent, except for the privacy-preserving ones like Monero. This allows anyone to audit a smart contract and find deployment or implementation bugs. The analysis of different cross-chain bridge security incidents is presented in the following table:

|  |  |  |
| --- | --- | --- |
| Bridge Name | **Vulnerability** | **Notes** |
| Chainswap | Improper Checks | Improper signature check that allowed attackers to generate their own fake ETH signatures. |
| Poly Network | Comporomised Keys | – |
| Multichain/AnySwap | Improper Cryptography Usage | Private key was recovered after using the same k twice in ECDSA signatures. More details [here](https://bitcoin.stackexchange.com/questions/35848/recovering-private-key-when-someone-uses-the-same-k-twice-in-ecdsa-signatures). |
| Qubit | Improper Checks | Injecting fake ETH and minting new tokens |
| Meter | Improper Checks | Code Injection |
| Wormhole | Improper Checks | Injecting fake sysvar account and minting new tokens |
| Li Finance | Improper Checks | Code Injection |
| Ronin | Comporomised Keys | – |
| Harmony | Comporomised Keys | – |
| Nomad | Improper Checks | Deployment Misconfiguration |

*Recent cross-chain bridge attacks and their vulnerabilities.*

From the table above, we can see that the top cross-chain bridge vulnerabilities are:

- Improper Checks
- Compromised Keys
- Improper Cryptography Usage

However, there is some overlap, since some of the improper checks are related to the generation and usage of signatures, which leads to a verification failure. So, this could be considered a chain of vulnerabilities.

## Possible Countermeasures

Growth in nature comes with pain, and the pain is part of the game. The same applies to cross-chain bridges, since developing secure protocols requires experimenting with the available technological advancements. The actors in cross-chain bridges are shaping the technology by learning from these painful experiences. Some of the takeaways from the discussed cases could be:

- Broaden the scope of smart contract audits to also cover deployment.
- Implement insurance mechanisms to cover losses.
- Apply longer withdrawal times in bridges (as is the case in optimistic rollups).
- Monitor asset transfers on bridge contracts.

For the sake of completeness, there are opinions against the current implementation of bridges, as well as against the idea of holding non-native tokens on other blockchains, since they are considered unsafe. One of the main advocates of this view is Vitalik Buterin, the founder of Ethereum. You can read a long post about this topic [here](https://old.reddit.com/r/ethereum/comments/rwojtk/ama_we_are_the_efs_research_team_pt_7_07_january/hrngyk8/).

## References

<https://www.certik.com/resources/blog/GuBAYoHdhrS1mK9Nyfyto-cross-chain-vulnerabilities-and-bridge-exploits-in-2022>

<https://blog.chainalysis.com/reports/cross-chain-bridge-hacks-2022/>

<https://www.certik.com/resources/blog/28fMavD63CpZJOKOjb9DX3-nomad-bridge-exploit-incident-analysis>

<https://medium.com/coinmonks/cross-chain-bridge-vulnerability-summary-f16b7747f364>
