# PEM file

PEM is a container file format often used to store cryptographic keys. It’s used for many different things, as it simply defines the structure and encoding type of the file used to store a bit of data.

PEM is just a standard; they contain text, and the format dictates that PEM files start with `-----BEGIN <type>-----`
and end with `-----END <type>-----`

Everything in between is base64 encoded (uppercase and lowercase letters, digits, +, and /). This forms a block of data that can be used in other programs. A single PEM file can contain multiple blocks.

This can be used to represent all kinds of data, but it’s commonly used to encode keyfiles, such as RSA keys used for SSH, and certificates used for SSL encryption. The PEM file will tell you what it’s used for in the header; for example, you might see a PEM file start with…
