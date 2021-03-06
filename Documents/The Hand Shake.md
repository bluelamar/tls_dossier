
# The Hand Shake

The client opens a connection to a server where the server is listening on a port that supports TLS (ex: TCP port 443).

This is the initiation of the Hand Shake.

The Hand Shake is done in several steps whereby the client and server must be able to agree on several aspects to the setup
of a secure connection. Which means that upon verification of the server (or mutual verification), they will agree to use
shared symmetric keys with which to encrypt all subsequent messages between them.

## Major goals of the Hand Shake
- Cipher suite negotiation
- Client Authentication of the server - prevents Man-in-the-Middle attacks. Optionally, the server authenticates the client : **asymmetric keys**
- Generation of the Session keys : **symmetric keys**

## What attributes must they agree upon?

### 1. TLS Protocol Version
* Recommended version: 1.2 or 1.1
* Note that 1.0 is NOT recommended, nor the earlier SSL versions such as SSLv3

### 2. Cipher Suite
ex: ECDH-RSA-AES128-SHA :

For a listing of Cipher suites using ECDH you can view the RFC https://tools.ietf.org/html/rfc4492#page-27

A cipher suite contains an algorithm for each of the following:
* key exchange : ex: ECDH, Elliptic-curve Diffie–Hellman public-private key pair algo
* authentication : ex: RSA, Rivest–Shamir–Adleman public-private key pair algo
* data encryption: ex: AES-128, Advanced Encryption Standard cipher
* message authentication (HMAC): ex: SHA1, Secure Hash Algorithm produces 160-bit (20-byte) hash value as the message digest

### 3. Pre-master secret
The client generates a 46-byte random number used in the generation of the Master secret.

### 4. Master secret
The server and client each generate the 48 byte Master Secret based on the Pre-Master Secret.

Then both client and server create the session keys based on the Master secret:
- client write MAC key : used for authentication and integrity check
- server write MAC key : used for authentication and integrity check
- client write key : used for message encryption
- server write key : used for message encryption
MAC:  message authentication code - calculated by a key against the message

## What messages are exchanged?

### 1. Client sends Hello message:
The Hello contains the following attributes:
* tls version
* supported cipher suites(prioritized by the client)
* data compression methods
* 28 byte random number

### 2. Server sends Hello back to the client:
This message contains the following attributes:
* chosen Cipher Suite based on the strongest suite that both the client and server support 
* chosen data compression method
* session id
* random number

### 3. Server sends its digital certificate:

### 4. Optionally, the server sends a Digital Certificate request:
This message is sent only if the server requires the client to authenticate.
* list of the types of digital certificates supported
* distinguished names(DN) of acceptable certificate authorities

### 5. Server sends Hello Done message:

### 6. Optionally, the client sends a Digital certificate response
If the client received the Digital Certificate request (message 4 above), then the client does only 1 of the following attributes is sent:
* clients digital certificate
* no certificate alert

### 7. Client sends a Client Key Exchange message:
This message is encrypted with the servers public key:
* pre-master secret
* a 46-byte random number 
* the message authentication code (MAC) keys

### 8. Client sends a Digital Certificate Verify message:
This message is signed with the client's private key.
* message signature

### 9. Client sends Change Cipher Spec:
client will start using the new session keys for hashing and encrypting messages
client and server now start using the session keys to exchange all messages

### 10. Client sends Finished message:
This message is encrypted with the chosen cipher and keys.

### 11. The server sends a Change Cipher Spec
This is an acknowledgement to the client.

### 12. The server sends a Finished message:
This is an acknowledgment to the client.

This is the End of the Hand Shake




