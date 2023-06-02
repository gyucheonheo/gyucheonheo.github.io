---
layout: post
title: How securely shares your secret with others?
subtitle:
tags: [encryption, decryption, openssl]
comments: false
---

Do you plan to share your secret, such as apikey, application secret, password, to your colleagues? But you don't want them to be stored in a persistant storage such as a messenger's database.
Then, let's follow it

Let's make up a situation.
Alice asks me to renew an apikey because Alices needs one more for a new application domain.
She told me "Send it to me through Slack!".
However, Slack stores our conversation and it'd be security breach candidate while scanning!
You and Alice and follows this.
1. Alice generates her private key
``` bash
$>openssl genpkey -algorithm RSA -out private_key.pem
```

2. Alice extracts her public key out of her private key
``` bash
$>openssl rsa -pubout -in private_key.pem -out public_key.pem
```

3. Alice shares her *public* key

4. You encrypts a file that includes a secret
``` bash
$>openssl rsautl -encrypt -pubin -inkey public_key.pem -in plaintext.txt -out encrypted.bin
```

5. You share an output file `encrypted.bin` to Alice

6. Alice descrypts the file `encrypted.bin` using her *private* key
``` bash
$>openssl rsautl -decrypt -inkey private_key.pem -in encrypted.bin -out decrypted.txt
```

Asymmetric encryption is a powerful tool that can be used to protect sensitive information. It is often used in conjunction with other security measures, such as passwords and authentication tokens, to create a more secure system.

Here are some additional benefits of using asymmetric encryption:

Confidentiality: Asymmetric encryption can be used to protect sensitive information from unauthorized access.
Integrity: Asymmetric encryption can be used to ensure that the contents of a message have not been tampered with.
Non-repudiation: Asymmetric encryption can be used to prove that a message was sent by a specific person.
