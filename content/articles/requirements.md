---
title: "🗒️ Coinstr Requirements"
date : 2023-02-08
author : Max Gravitt
---

This document tracks some of the Coinstr requirements. 

1. Approval Users must be able to sign/approve spending proposals with a mobile app (i.e. Hancock) or Nostr client.
2. Power Users may use Coinstr as a desktop application (local key storage), or if a web application, use a remote signing protocol to the mobile device using QR codes or push notifications. 
3. Authentication must not rely on browser plugins for either user type.
4. Powers Users must be able to use [blockly](https://developers.google.com/blockly) (see [BDK Playground](https://bitcoindevkit.org/bdk-cli/playground/)) or similar no-code environment to configure flexible spending policies.
5. Coinstr must use a Nostr relay to route signature requests to leverage network effects of the blooming ecosystem.