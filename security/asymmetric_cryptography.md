# Asymmetric cryptography

Asymmetric cryptography, also known as public key cryptography, uses pairs of public and private keys. These two different but mathematically linked keys are used to transform plaintext into encrypted ciphertext or encrypted text back to plaintext.

When the private key is used to encrypt ciphertext, that text can be decrypted using the public key. That ciphertext can be a component of a digital signature and used to authenticate the signature. Only the holder of the private key could have encrypted ciphertext, so if the related public key successfully decrypts it, the digital signature is verified.

The public key is made available to everyone that needs it in a publicly accessible repository. The private key is confidential and should only be accessible to the public key pair owner. In this method, whatever is encrypted with the public key requires the related private key for decryption and vice versa. Public key encryption is typically used for securing communication channels, such as email.
