---
title: 🚀 Coinstr Launches CLI for Bitcoin Multisig on Nostr
date : 2023-03-23
slug: coinstr-cli-launch
authors: Max Gravitt
---
<head>
  <title>🚀 Coinstr Launches CLI for Bitcoin Multisig on Nostr</title>
  <meta charSet="utf-8" />
  <meta property="og:title" content="🚀 Coinstr Launches CLI for Bitcoin Multisig on Nostr" />
  <meta property="og:image" content="https://coinstr.app/coinstr.png" />
  <meta property="og:description" content="This Coinstr CLI tutorial provides a step by step command line demo of using Coinstr to create and spend with a 2-of-2 multisignature." />
  <meta property="og:url" content="https://coinstr.app/articles/coinstr-cli-launch" />
  <meta name="twitter:title" content="🚀 Coinstr Launches CLI for Bitcoin Multisig on Nostr" />
  <meta name="twitter:creator" content="@MaxGravitt">
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:image" content="https://coinstr.app/coinstr.png" />
  <meta name="twitter:description" content="This Coinstr CLI tutorial provides a step by step command line demo of using Coinstr to create and spend with a 2-of-2 multisignature." />

</head>

The companion video walks through the steps of the tutorial.

<iframe width="560" height="315" src="https://www.youtube.com/embed/jW5_6kZWuWU" frameborder="0" allowfullscreen></iframe>

Here are the activities that we will cover in the tutorial:

![image](https://user-images.githubusercontent.com/32852271/227331892-d8447b4b-09be-4faf-9e05-b700350d1332.png)

## Step by Step - 2 of 2 Multisig
This step by step guide shows how to create a 2 of 2 multisignature Bitcoin transaction using `coinstr-cli`. 

For this tutorial, you'll need to [install rust](https://rustup.rs) and also build Coinstr.

## Step 1. Build Coinstr
```bash
git clone https://github.com/NostrDevKit/coinstr.git

cd coinstr

cargo build --release
```

## Step 2. Setup Keys
Create keys and save them to keychains. I've used the keychain names of `frank` and `gary`.

> NOTE: Mnemonics will be printed but they don't need to be saved for this tutorial.

`--password` is for encrypting your local keechain file, while `--passphrase` is applied to your mnemonic and alters the keys. Below, the password is set to `1234` and I am not using a passphrase.

```
./target/release/coinstr-cli generate --password 1234 --passphrase "" frank
./target/release/coinstr-cli generate --password 1234 --passphrase "" gary
```

### Inspect Keys

We can inspect the keys we just created by running: 
```bash
COINSTR_PASSWORD=1234 ./target/release/coinstr-cli --network testnet inspect frank
```

This will show the mnemonic as well as useful key-related derived information for both Nostr and Bitcoin.
![image](inspect-keys.png)

## Step 2: Create the 2-of-2 Policy
Using miniscript, we will create a 2 of 2 multisignature policy that follows this format: 
```
thresh(2,pk(<FRANK_PUBKEY>),pk(<GARY_PUBKEY>))
```

Inspect the keys you created above and paste your Nostr HEX public key into the miniscript template.

So, the miniscript for our 2 of 2 policy is (your keys will be different):
```
thresh(2,pk(5e61551ceb04521181d9ad40295e32dce5dc5609c4612a3239dbc60c30080dcd),pk(d223b67e6091ef0665188a4016d20a51a7bbb1b240fafc4429bf1329527338d1))
```

## Step 3: Save the Policy 
Save the policy using the key, policy name, policy description, and the output descriptor.

```
COINSTR_PASSWORD=1234 \
./target/release/coinstr-cli save-policy \
    frank \
    "Multisig 2 of 2" \
    "Testing multisig as part of the Coinstr CLI tutorial" \
    "thresh(2,pk(5e61551ceb04521181d9ad40295e32dce5dc5609c4612a3239dbc60c30080dcd),pk(d223b67e6091ef0665188a4016d20a51a7bbb1b240fafc4429bf1329527338d1))"
```

Now you can review the saved policies for frank using the following command: 
```
COINSTR_PASSWORD=1234 ./target/release/coinstr-cli get policies frank
```

Produces: 
```
+---+------------------------------------------------------------------+-----------------+------------------------------------------------------+
| # | ID                                                               | Name            | Description                                          |
+===+==================================================================+=================+======================================================+
| 1 | e2927eabd79c817df2e7e16be9f6125bba979259f41efe0f22cc9a33fc2b9824 | Multisig 2 of 2 | Testing multisig as part of the Coinstr CLI tutorial |
+---+------------------------------------------------------------------+-----------------+------------------------------------------------------+
```

You can see the details of the policy by calling `get policy`: 
```
COINSTR_PASSWORD=1234 ./target/release/coinstr-cli --network testnet \
    get policy \
    frank \
    e2927eabd79c817df2e7e16be9f6125bba979259f41efe0f22cc9a33fc2b9824
```

> NOTE: Gary has the same policy saved into his list.

Produces the following output: 
```
Policy
- ID: e2927eabd79c817df2e7e16be9f6125bba979259f41efe0f22cc9a33fc2b9824
- Name: Multisig 2 of 2
- Description: Testing multisig as part of the Coinstr CLI tutorial
- Descriptor
└── id -> 8hmw3yl0
    └── 👑 Threshold Condition   : 1 of 2 
        ├── id -> en4snfs4
        │   └── 🔑 Schnorr Sig of  <xonly-pk:bbbc8e8b39d46a289a51bd5029dde3c946b9e97be935dc1e59845aa3b36ae101>
        └── id -> eh682v0m
            └── 🤝 MultiSig  :  2 of 2
                ├── 🔑 <xonly-pk:5e61551ceb04521181d9ad40295e32dce5dc5609c4612a3239dbc60c30080dcd>
                └── 🔑 <xonly-pk:d223b67e6091ef0665188a4016d20a51a7bbb1b240fafc4429bf1329527338d1>

Balances
- Immature            	: 0 sats
- Trusted pending     	: 0 sats
- Untrusted pending   	: 0 sats
- Confirmed           	: 0 sats

Deposit address: tb1pw3kszzyg67crrevpgxnfkyh3zn045hdqp56cgspkufvuefz9rllszm7kss
```

## Step 4: Get Testnet BTC from Faucet
Use the [testnet bitcoin faucet](https://testnet-faucet.com/btc-testnet/) to request BTC for our policy. The deposit address is at the bottom of the output above.

## Step 5: Generate a Spend Proposal
We will create the spend proposal from Alice's perspective. to create a spend proposal: 
```
Usage: coinstr-cli spend <NAME> <POLICY_ID> <TO_ADDRESS> <AMOUNT> <MEMO>
```
```
COINSTR_PASSWORD=1234 ./target/release/coinstr-cli --network testnet spend \
    frank \
    e2927eabd79c817df2e7e16be9f6125bba979259f41efe0f22cc9a33fc2b9824 \
    mohjSavDdQYHRYXcS3uS6ttaHP8amyvX78 \
    1000 \
    "Send back to the faucet"
```
You can now view the spend proposal:
```
COINSTR_PASSWORD=1234 ./target/release/coinstr-cli get proposals frank
```
Create the below table: 
```
+---+------------------------------------------------------------------+-----------+-------------------------+------------------------------------+------------+
| # | ID                                                               | Policy ID | Memo                    | Address                            | Amount     |
+===+==================================================================+===========+=========================+====================================+============+
| 1 | 04ecf276638dc441aaac9eb8d6ae884f907e63d7f65d302515de5ada001d1e0e | e2927eabd | Send back to the faucet | mohjSavDdQYHRYXcS3uS6ttaHP8amyvX78 | 1 000 sats |
+---+------------------------------------------------------------------+-----------+-------------------------+------------------------------------+------------+
```

## Step 6: Approve a Spend Proposal
Now we need to approve the proposal from both Alice and Bob's perspective.
```
./target/release/coinstr-cli --network testnet approve \
    frank \
    04ecf276638dc441aaac9eb8d6ae884f907e63d7f65d302515de5ada001d1e0e

./target/release/coinstr-cli --network testnet approve \
    gary \
    04ecf276638dc441aaac9eb8d6ae884f907e63d7f65d302515de5ada001d1e0e
```

> NOTE: if you try to broadcast the transaction before it is finalized, you will get an error such as `PSBT not finalized: [InputError(CouldNotSatisfyTr, 0), InputError(CouldNotSatisfyTr, 1)]`.


## Step 7: Broadcast the Transaction
```
./target/release/coinstr-cli --network testnet broadcast \
    gary \
    04ecf276638dc441aaac9eb8d6ae884f907e63d7f65d302515de5ada001d1e0e
```

You will get a transaction-id that you can view with a block explorer: 
```
Transaction 56294f3e8071d621d7158d2c2690faf05e9b7a81aeed7e6e6f312b0a8eb6e5b5 broadcasted

Explorer: https://blockstream.info/testnet/tx/56294f3e8071d621d7158d2c2690faf05e9b7a81aeed7e6e6f312b0a8eb6e5b5 
```
