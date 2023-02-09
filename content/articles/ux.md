---
title: "👤 Coinstr User Experience"
date : 2023-02-08
author : Max Gravitt
---

Coinstr has two user types: 
1. power users that configure spending policies 
2. spending proposal approvers

# 👷 Power User
The power user is perhaps an accountant, financial controller, or someone in a company's IT department. This user does not need to be able to write code or understand cryptography, but they do need to be able to use the [blockly](https://developers.google.com/blockly) powered UI to configure policies.

{{< plantuml id="pw" >}}
@startuml
left to right direction
actor "Power User" as pw

package "Coinstr Web App" {
  usecase "Configure Policies" as UC1
  usecase "Sweep UTXOs (Coin Control)" as UC2
  usecase "Create Proposals" as UC3
}
pw --> UC1
pw --> UC2
pw --> UC3

@enduml{{< /plantuml >}}

# 🤵 Approval User
The approval user is someone who will review spending proposals and make decisions about whether or not to sign and approve them. 

{{< plantuml id="au" >}}
@startuml
left to right direction
actor "Approval User" as au

package "Coinstr Mobile Signer" {
  usecase "Review Proposals" as UC5
  usecase "Create Proposals" as UC6
  usecase "Approve Proposals" as UC7
}

au --> UC5
au --> UC6
au --> UC7
@enduml{{< /plantuml >}}

# 🔐 Authentication

Power users may authenticate to the web application using any compatible Nostr method, including a browser plugin, but the preferred method is to use [NIP-46 Nostr Connect](https://github.com/nostr-connect/nips/blob/nostr-connect/46.md). This method keeps the users' private key(s) safely on their mobile device instead of in the browser.

This video shows [Nostrum](https://github.com/nostr-connect/nostrum), a pioneering NIP-46 compatible mobile app. Coinstr will launch a mobile application (named **Hancock Signer**) that supports NIP-46-style QR-code based signing for Nostr and Bitcoin transactions.

{{< video "https://user-images.githubusercontent.com/3596602/211125690-a16d0d3d-d010-44b2-85e3-94b4e9f476c7.mp4" "my-5" >}}

Of course, IRL, instead of using copy and paste from the browser to the mobile app, the QR code will be scanned. 

When an in-browser action requires a signature from the user, a push notification is sent to the mobile app where the user can review and approve.