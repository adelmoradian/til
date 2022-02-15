# TLS/SSL handshake

here's the server client dialogue.

- client: hey server! let's setup an encrypted session. Here's a list of cipher suites and TLS version that I'll be using
- server: let's use this specific cipher suite from your list. I can also work with your TLS version. Here's my certificate (which includes the public key)
- client: `it verifies the server cert. If the cert is self signed, it can't trust it... needs a CA to give the cert. if server cert is trusted, client extracts the server public key to encrypt the pre-master key`. hey server! here's the encrypted pre-master key. I've encrypted it using your own public key from your own certificate. if you're legic, decrypt it
- server: `if server is legit, it uses it's private key to decrypt the message`
- client and server: `they both use the pre-master key to communicate a shared secret key called shared secret`
- client: hey server! here's an encrypted message using our shared secret. decrypt it and see if it's up to spec. Also from now on all my messages will be encrypted using our shared secret!
- server: `if server can decrypt the message, it sends message back to client (encrypted using the shared secret)`. your encrypted message checks out! now take my encrypted message and verify that you can decrypt it. it was encrypted using our very own shared secret. From now on also i will encrypt all my messages with our shared secret
- client and server: both happily using shared secret for communication
