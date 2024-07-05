# Anonymous friend mode

The first time you add a friend, instead of passing the message via an identity key, you use a one-time pubkey.
There will be no information exchange between alice and bob at the identity key level.

## Preparation: OneTime Receving Keys

1. App maintains a `oneTimeKeys` list. Default: 3 public and private key pairs
2. Listen `oneTimeKeys` on relay. If notifications are enabled, they are sent to the notification server
3. If a one-time key receives a message, it will be marked as used and deleted after one day.
4. If the number of one-time keys falls below three, the program will automatically generate new keys.

## Add Friends Mode

### Scan QRCode

1. Alice shows her QR code to Bob.
2. Bob scans Alice's QR code.
3. Bob creates a double ratchet, and obtains Alice's one-time key.
4. Bob sends his relevant keys to Alice's one-time key with a NIP4(NIP4) message.
5. Alice receives the message, decrypts it, and then gets Bob's relevant keys.
6. Alice creates a double ratchet.
7. Communication is complete.

### Search from public key

1. Bob obtains Alice's public key. (From WhatsApp, Facebook, or a Keychat group)
2. Bob sends the relevant keys and one-time key to Alice's identity public key with a NIP4(NIP4) message.
3. Alice creates a double ratchet.
4. Alice sends the relevant keys to the one-time key with a NIP4(NIP4) message.
5. Bob creates a double ratchet when he receives the message.
6. Communication is complete.
