---
title: "💸 Spending Policy Deep Dive"
date : 2023-02-08
author : Max Gravitt
tags: 
  - business
---

# What can be used in a spending policy? 
A spending policy is comprised of a *tree* of criteria, where one *path* of the tree must be **true** in order for the bitcoin to be spent.

The tooling for criteria is always expanding, but includes elements such as: 
- 🤵 **Who** can approve
- 🧑‍🔧 Which **Roles** can approve
- 🎚️ **Thresholds** and **weights** to individual users or roles
- ⏳ **Wait time**, such as 1 week, which allows for decision makers to passively approve and only act if they wish to prevent a spend.
- ₿ **Amount** of bitcoin being spent
- 🤖 **Oracles** or **bots** that query offchain data and only sign based on custom criteria

# Steps for Policy and Spending

# More Coming Soon

In the meantime, see Bitcoin Dev Kit's [Elephant workshop](https://github.com/bitcoindevkit/elephant) to learn more about spending policies. 

Coinstr implements a hardened and secure version of the UI components found in the workshop.