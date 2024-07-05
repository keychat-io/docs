# Room's Receiving Addresses

In Keychat, each room has a unique receiving addresses, and this addresses change after each round of communication. This separation of identity and receiving addresses enhances privacy protection.

## How to Generate a New Receiving Address?

1. The Signal encryption algorithm generates a new encryption key for each round of chat.
2. Take the encryption key and perform sha256 to get a `seed string`.
3. Use the `seed string` as a seed to create a new secp256k1 private key and public key.
4. The public key is the new receiving address.

## Field Description

ReceiveKeys represents the receiving keys for my one-on-one (1v1) communication with any user. This information is stored in the contact.

```
List<String> receiveKeys = [];
```

## Add

When encrypting a message, generate a new address `pubkey1`.
Check if `pubkey1` is equal to `receiveKeys.last`.
If not, receiveKeys = [pubkey1].

```
receiveKeys = [pubkey1, pubkey2].
receiveKeys = [pubkey1, pubkey2, pubkey3].
receiveKeys = [pubkey1, pubkey2, pubkey3, pubkey4]
```

## Delete

1. Set a maximum of 3 receiving addresses to be retained.
2. When a message is received at any of the addresses `pubkey1`, `pubkey2`, or `pubkey3`, no deletion action is performed.
3. When a message is received at my `pubkey4`, delete `pubkey1`.
