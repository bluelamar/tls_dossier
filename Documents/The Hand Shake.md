
# The Hand Shake

The client opens a connection to a server where the server is listening on a port that supports TLS (ex: TCP port 443).

This is the initiation of the Hand Shake.

The Hand Shake is done in several steps whereby the client and server must be able to agree on several aspects to the setup
of a secure connection. Which means that upon verification of the server (or mutual verification), they will agree to use
shared symmetric keys with which to encrypt all subsequent messages between them.

## Major goals of the Hand Shake
- Cipher suite negotiation
- Client Authentication of the server - prevents Man-in-the-Middle attacks. Optionally, the server authenticates the client : asymmetric keys
- Gemeration of the Session keys : symmetric keys

## What attributes must they agree upon?

### 1. TLS Protocol Version
* Recommended version: 1.2 or 1.1
* Note that 1.0 is NOT recommended, nor the earlier SSL versions such as SSLv3

### 2. Cipher Suite
ex: ECDH-RSA-AES128-SHA

A cipher suite contains an algorithm for each of the following:
* key exchange : ex: ECDH
* authentication : ex: RSA
* data encryption: ex: AES-128
* message authentication (MAC): ex: SHA1

### 3. Pre-master secret
The client generates a 46-byte random number used in the generation of the Master secret.

### 4. Master secret
The server and client each generate the 48 byte Master Secret based on the Pre-Master Secret.

Then both client and server create the session keys based on the Master secret:
- client write MAC key : used for authentication and integrity check
- server write MAC key : used for authentication and integrity check
- client write key : used for message encryption
- server write key : used for message encryption

## What messages are exchanged?

### 1. Client sends cipher suite message:
* tls version
* supported cipher suites(prioritized by the client)
* data compression methods
* 28 byte random number

TODO 

### n. Client sends Change cipher spec:
client will start using the new session keys for hashing and encrypting messages
client and server now start using the session keys to exchange all messages




