# Incognito Nip4 Message

In the Keychat app, we prioritize user privacy.

The most crucial aspect is safeguarding the ID Pubkey, which is as essential as your phone number. If the ID Pubkey is exposed, anyone can send messages and add friends to this identity key.

By tracing back historical messages, it is possible to identify which ID Pubkeys engaged in conversations. Therefore, we employ a double-layered Nip04 message structure, Nip04(Nip04(rawMessage)).

## Nip04

[nostr protocol: Nip04](https://github.com/nostr-protocol/nips/blob/master/04.md)

```javascript
let event = {
  pubkey: ourPubKey, // my id pubkey
  created_at: Math.floor(Date.now() / 1000),
  kind: 4,
  tags: [["p", theirPublicKey]], // to his id pubkey
  content: encryptedMessage + "?iv=" + ivBase64,
};
```

This approach exposes the identity keys of both the recipient and the sender.

## Protecting the sender's ID Pubkey

> Mode: Nip04(Nip04(rawMessage))

```javascript
let subEvent = JSON.stringify({
  pubkey: myIDPubKey,
  created_at: Math.floor(Date.now() / 1000),
  kind: 4,
  tags: [["p", theirPublicKey]],
  content: nip04Encrypt(rawMessage, myIDPrivateKey),
});

let event = {
  pubkey: oneTimeRandomPubKey, // "Once encryption is complete, it can be deleted.
  created_at: Math.floor(Date.now() / 1000),
  kind: 4,
  tags: [["p", receiveKey]], // receiveKey: oneTimeKey,group shared Key, their ID Pubkey
  content: nip04Encrypt(subEncryptedMessage, randomPrivateKey),
};
```

1. Randomly generated sender private key and public key can be discarded after encryption
2. Encryption key is random, relay cannot know the real sender
3. After the user receives the message, they need to decrypt the outer event first
4. Then decrypt the subEvent
5. The subEvent contains the actual sender's ID Pubkey (Group Memeber's ID)

## Protecting the recipient's ID Pubkey

1. In the design of nip04, receiveKey is generally IDPubkey
2. If we set the receiveKey to a oneTimeKey agreed upon by both parties, then the effect of hiding the recipient's ID can be achieved
3. In one-to-one signal chat room, For each conversation session, we generate a recipient key. All messages are not sent to the identity key, but rather to the recipient key, which continuously rotates.
