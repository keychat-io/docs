# Greeting Message

When adding a friend for the first time, we hope to hide the identity key to achieve better privacy and metadata protection.

## 1. Preparation: OneTime Receving Keys

1. App maintains a `oneTimeKeys` list. Default: 3 public and private key pairs
2. Listen `oneTimeKeys` on relay. If notifications are enabled, they are sent to the notification server
3. If a one-time key receives a message, it will be marked as used and deleted after one day.
4. If the number of one-time keys falls below three, the app will automatically generate new keys.

## 2. Scan QRCode to add contact

1. Alice shows her QR code to Bob.
2. Bob scans Alice's QR code.
3. Bob creates a double ratchet, and obtains Alice's one-time key.
4. Bob sends his relevant keys to Alice's one-time key with a NIP4(NIP4) message.
5. Alice receives the message, decrypts it, and then gets Bob's relevant keys.
6. Alice creates a double ratchet.
7. Communication is complete.

## 3. Search public key to add contact

1. Bob obtains Alice's public key. (From WhatsApp, Facebook, or a Keychat group)
2. Bob sends the relevant keys and one-time key to Alice's identity public key with a NIP4(NIP4) message.
3. Alice creates a double ratchet.
4. Alice sends the relevant keys to the one-time key with a NIP4(NIP4) message.
5. Bob creates a double ratchet when he receives the message.
6. Communication is complete.

## 4. UserInfo

```
  {
    'name': identity.displayName,
    'pubkey': identity.secp256k1PKHex,
    'curve25519PkHex': identity.curve25519PkHex,
    'relay': <Select Relay to Chat>,
    'onetimekey': <onetimekey>,
    'time': <Expired at>,
    "globalSign": "",
    "signedId": $signedId,
    "signedPublic": $signedPublic,
    "signedSignature": $signedSignature,
    "prekeyId": $prekeyId,
    "prekeyPubkey": $prekeyPubkey,
  }
```
