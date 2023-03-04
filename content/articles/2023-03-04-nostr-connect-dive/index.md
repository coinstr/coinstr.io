---
title: 🤿 Nostr Connect Deep Dive
date : 2023-03-04
tags:
  - security
  - nostr
slug: nostr-connect-deep-dive
authors: Max Gravitt
---
<head>
  <title>🤿 Nostr Connect Deep Dive</title>
  <meta charSet="utf-8" />
  <meta property="og:title" content="🤿 Nostr Connect Deep Dive" />
  <meta property="og:image" content="https://coinstr.app//articles/nostr-connect-deep-dive/bob-login-cover.png" />
  <meta property="og:description" content="Nostr Connect can be used to create a seamless and intuitive flow between applications where users do their work and their remote signing devices safely and securely." />
  <meta property="og:url" content="https://coinstr.app/articles/nostr-connect-deep-dive" />
  <meta name="twitter:title" content="🤿 Nostr Connect Deep Dive" />
  <meta name="twitter:creator" content="@MaxGravitt">
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:image" content="https://coinstr.app//articles/nostr-connect-deep-dive/bob-login-cover.png" />
  <meta name="twitter:description" content="Nostr Connect can be used to create a seamless and intuitive flow between applications where users do their work and their remote signing devices safely and securely." />

</head>

Nostr Connect can be used to create a seamless and intuitive flow between applications where users do their work and their remote signing devices safely and securely.  This articles describes the UX for shows how a circuit is created and how Bob can login to update his work on multiple devices.

# 📱 Logging In
{{< plantuml id="au" >}}
@startuml
autonumber 1 1 "<font color=red><b>msg 0  "
hnote over "Bobs Browser" : Running in Browser \non Bobs Desktop

"Bobs Browser" -> Relay: New Subscription to Listen for Users
"Bobs Browser" <-- "Bobs Phone": Scans QR Code for NC request
"Bobs Phone" -> Relay: Signs NC requests and Broadcasts
"Bobs Phone" -> Relay: Opens a subscription to receive events \nfrom Bobs Browser
"Bobs Browser" <- Relay: Receives response over subscription
hnote over "Bobs Browser" : Bob's Phone is connected \nto Bob's Browser

@enduml{{< /plantuml >}}

<font color=red>msg 1.</font> The webapp uses its private key to post a message to the relay that is awaiting for a party to establish a session. It also make a QR code or `nostrconnect://` link available on the site to scan. The `nostrconnect` string is static, and so the QR code is as well. 

The URI string has a format like the `uri` below.
```js
const pubkey = "pubkey-representing-the-browser"
const relay = "wss://which-relay-the-parties-will-use-to-bounce-events.com"
const applicationName = "Content Authoring Application Name"

const uri = `nostrconnect://${pubkey}?relay=${encodeURIComponent(relay)}&
                metadata=${encodeURIComponent(JSON.stringify({"name": applicationName}))}`
```
<font color=red>msg 2.</font> Bob scans the the QR code and is asked whether to approve sending a Login proof to the WebApp in Bob's Browser.

<font color=red>msg 3.</font> After signing, Bob broadcasts the message to the relay that was provided in the QR code. He does not broadcast this to other relays. 

<font color=red>msg 4.</font> Just as the Bob's Browser opened a subscription to Bob in step 1, Bob opens a websocket subscription to receive updates from the WebApp.

<font color=red>msg 5.</font> The WebApp was already subscribed from step 1 and now receives the Login proof and allows the user into the application.

The events described above all ephempheral, meaning they are not saved on the relay longer than a few minutes. They are also encrypted, but they **do leak metadata** about who is logging into which applications via which relays. A closed relay could be used for more privacy.

# 🖥️ Using the Application
At this point, Bob is logged into the application on his browser. He may start to author some content, post some events (tweets), creating bitcoin spending policies, or propose spends.

Let's say that he authors a long paper using markdown on his computer. He is not quite finished, but he wants to save it as a draft. 

In the application, he can perform a **Save** action. This would encrypt the document locally and send it to the relay which will forward it over the subscription to Bob's Phone. The device would ask Bob if he approves saving that event to his private own DMs (Notes to Self). 

When Bob approves, the signed event is returned the Bob's Browser via the Relay, where the WebApp then sends it to the relay as a NIP-04 encrypted message. 

{{< plantuml id="auq" >}}
@startuml
autonumber 10 1 "<font color=red><b>msg 0  "
hnote over "Bobs Browser" : Authoring Content...
"Bobs Browser" <- "Bobs Phone": Bob clicks the Save Draft button \nin the Browser WebApp
"Bobs Browser" -> Relay: Encrypts the Content, \nSends Request to Sign Event \nto save Draft to his Self Notes
Relay ->  "Bobs Phone": Bob approves, Signs event\n encapsulated in a \nwrapper event
"Bobs Phone" -> Relay: Sends the wrapper event \nback to the relay
Relay -> "Bobs Browser": Receives, discards wrapper \nas ephemeral, and broadcasts \nthe signed note\n to the relay to be \nstored in Bobs Messages to Self
@enduml{{< /plantuml >}}

> Do we need a LOG-OFF event to inform the Browser to end the subsciption? (and also erase any local data)

# 💻 Later that Day (or Year)
Later that day, Bob begins working on his laptop and wants to finalize his paper and publish it to Nostr. 

Bob goes to the URL of the application on his laptop browser and scans the QR to Login. The same login sequence that occurred at the beginning occurs again to create a session.

Once logged in, Bob sees his content within his Notes to Self and continues to edit and proofread. When he is complete, it clicks 'Post' to submit the content as a normal Nostr event. 

That click in the Browser constructs an Event and encapsulates it in wrapper event to send to Bob's Phone over the relay. Bob reviews the hash of the event in the browser and on the device to know it is the same one, and then approves.

The approval is sent back to Bob's Browser where it is broadcast to a relay as the public post. It is only at this point that the content is accessible unencrypted by any audience or component outside of Bob's local systems.

{{< plantuml id="auz" >}}
@startuml
autonumber 20 1 "<font color=red><b>msg 0  "
hnote over "Bobs Browser" : Now on Laptop
"Bobs Browser" <-- "Bobs Phone": Logs in on Laptop,\n the same he did on Desktop above
"Bobs Browser" <- Relay: Browser subscribes to events \nincluding the paper that \nwas saved in Notes to Self
Relay ->  "Bobs Browser": When editing is complete, Bob \nclicks Publish. This triggers a\n signature request to go from the\n browser to the relay
"Bobs Phone" <- Relay: The relay forwards the signature \nrequest through the subscription that \n Bob has on his phone \nand he asked to approve.
Relay <- "Bobs Phone": The signed event of \nthe ready-to-publish paper
hnote over Relay : Event is saved as \na Public Post
@enduml{{< /plantuml >}}

# ✨ Summary
Establishing secure, flexible, and real time communications between the browser and application and device makes many great use cases available for Nostr. 

# Bitcoin Signatures in Nostr Connect
I am an admitted and unashamed stalker. I noticed that yesterday, the author of the [Nostr Connect](https://github.com/nostr-connect/connect) protocol and the reference implementation [Nostrum](https://github.com/nostr-connect/nostrum) starred and forked a Javascript library for **signing and decoding Bitcoin transactions**. [Coinstr](https://coinstr.app) is very interested in using this protocol to sign more than Nostr events, especially Bitcoin. We will keep a close eye on what transpires.

![image](https://user-images.githubusercontent.com/32852271/222914638-fe23a97b-d616-428e-8c52-42e316881c60.png)

[Follow Max on Nostr](https://snort.social/p/npub1ws2t95pdtpna4ps62rrz75mm6ujsudjv70yj2jk4wsqjhedlw22qsqwew9) for more articles and information about innovation around Nostr, Bitcoin and Lightning.

https://snort.social/p/npub1ws2t95pdtpna4ps62rrz75mm6ujsudjv70yj2jk4wsqjhedlw22qsqwew9
