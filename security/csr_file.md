# CSR file

A Certificate Signing Request (CSR) file is something you generate and give to a Certificate Authority, who in turn signs and sends you the requested SSL certificate that used for enabling HTTPS on your web server.

CSR files contain information on your organization and the type of certificate you’re requesting. They’re usually generated automatically with the help of a utility like OpenSSL. If you’re using LetsEncrypt, CSR file creation is all managed by certbot for you.

CSR files contain the following info:

- Common Name (CN) – Your server’s hostname. It must match exactly, or your users will see an error page in their browser saying the certificate is untrusted. You can use wildcards (e.g., *.domain.com) to request a wildcard certificate applying to all subdomains. A wildcard like this applies to www, but if you’re looking to secure your root domain and all subdomains, you’ll need two separate certificates. Common Name is the only field that is technically required, so you could leave everything else blank if you desired. However, it’s good to fill out the others.
- Organization (O) – The full legal name of your company, including any suffixes such as LLC. If you’re requesting an EV or OV certificate (which are entirely pointless), it will need to be validated. For a normal SSL though, you can put whatever, as it’s not checked and nor even required.
- Organizational Unit (OU) – The division of your company that is handling the certificate.
- Country (C) – The two-letter country code of the country you’re located in.
- State/County/Region (S) – The full name of the state you’re located in.
- City/Locality (L) – The full name of the city you’re located in.
- Email Address – Your organization’s email address.
- The RSA public key used

The only one that affects how your CSR file is processed is your common name. The domain name will need to be validated to prevent you from registering someone else’s domain; you’ll be given a challenge from the Certificate Authority later in the process to prove you own the domain, but the CSR file has no effect on that.

The actual CSR file itself is in PEM format, and is a large chunk of base64 encoded data.
