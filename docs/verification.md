# Verifying did:web DIDs

This document explains how to verify the `did:web` DIDs created in this project. Verification is a two-step process:

1.  **Resolution:** Finding the DID Document associated with the DID.
2.  **Verification:** Using the public key in the DID Document to verify a signature.

## IMPORTANT: Private Key Management

The private key is the most critical component of your DID. It is your proof of ownership and allows you to control your DID.

*   **DO NOT lose your private key.** If you lose it, you lose control of your DID forever. There is no recovery mechanism.
*   **DO NOT share your private key.** Anyone with access to your private key can impersonate you.
*   **DO NOT commit your private key to a public repository.** The `.gitignore` file in this project is configured to prevent this, but you should always be vigilant.

**Store your private keys in a secure location, such as a password manager or a hardware wallet.**

## 1. Resolution

A `did:web` is resolved by converting the DID into an HTTPS URL. The process is as follows:

1.  Replace `did:web:` with `https://`.
2.  Replace colons (`:`) in the domain with slashes (`/`).
3.  Append `/.well-known/did.json` if there is no path, or append `/did.json` if there is a path.

For example, to resolve `did:web:agents.slashlife.ai:max`:

1.  Start with the DID: `did:web:agents.slashlife.ai:max`
2.  Replace `did:web:` with `https://`: `https://agents.slashlife.ai:max`
3.  Replace the colon in the domain with a slash: `https://agents.slashlife.ai/max`
4.  Append `/did.json`: `https://agents.slashlife.ai/max/did.json`

You can use `curl` to fetch the DID Document:

```bash
curl https://agents.slashlife.ai/.well-known/max/did.json
```

The output should be the `did.json` file for Max.

## 2. Verification

Verification involves using the public key from the DID Document to verify a signature. Here's how you can do it:

### Step 2.1: Create a sample message and sign it

First, let's create a sample message and sign it with the private key.

```bash
# Create a sample message
echo -n "This is a test message" > message.txt

# Sign the message with the private key for Max
openssl pkeyutl -sign -inkey .well-known/max/private.pem -rawin -in message.txt -out signature.bin
```

### Step 2.2: Get the public key from the DID Document

Next, you need to extract the public key from the DID Document. You can do this manually or with a script. The public key is the `publicKeyBase64` value.

For Max, the public key is: `zLtQ2+2FreksAdOSp7/FKqET0x1LekT0z41O8bziasc=`

Let's decode it and save it to a file:

```bash
echo "MCowBQYDK2VwAyEAzLtQ2+2FreksAdOSp7/FKqET0x1LekT0z41O8bziasc=" | base64 -d > public_key.der
```

Now, convert the DER-encoded key to a PEM-encoded key:

```bash
openssl pkey -pubin -inform DER -in public_key.der -out public_key.pem
```

### Step 2.3: Verify the signature

Finally, you can verify the signature using the public key:

```bash
openssl pkeyutl -verify -pubin -inkey public_key.pem -rawin -in message.txt -sigfile signature.bin
```

If the signature is valid, the command will output `Signature Verified Successfully`.

This process demonstrates how to verify the authenticity of a message signed by a `did:web` DID. By resolving the DID and using the public key from the DID Document, you can be confident that the message was signed by the entity that controls the DID.