#### Certificate extensions

    An SSL Certificate is essentially an X.509 certificate, it defines the structure of the certificate.
    These certificate files will have different extensions based on the format and encoding they use.

#### Types of encoding
 
    Binary
    Base64 ASCII

#### Formats

    PEM uses Base64 ASCII encoding
    DER uses Binary encoding

#### PEM file extensions (pem, crt, key, cer)

    The .pem file can include the server certificate, the intermediate certificate and the private key in a single file
    The server certificate and intermediate certificate can also be in a separate .crt or .cer file
    The private key can be in a .key file
    
    Each certificate in the PEM file is contained between the ---- BEGIN CERTIFICATE---- and ----END CERTIFICATE---- statements
    The private key is contained between the ---- BEGIN RSA PRIVATE KEY----- and -----END RSA PRIVATE KEY----- statements
    The CSR is contained between the -----BEGIN CERTIFICATE REQUEST----- and -----END CERTIFICATE REQUEST----- statements

#### DER file extensions (der, cer)

    The DER certificates are in binary form, contained in .der or .cer files.
    These certificates are mainly used in Java-based web servers.
    
#### OpenSSL commands

Print the certificate in text form and don't print certificate output.
    
    openssl x509 -in server.crt -text -noout

#### Verify Whether a Certificate and Private Key Match

Verify Whether a Certificate and Private Key Match.To verify you need to print out md5 checksums and compare them.
    
    openssl x509 -noout -modulus -in server.crt| openssl md5
    openssl rsa -noout -modulus -in server.key| openssl md5

#### Verify key and it's validity

    sudo openssl rsa -in server.key -noout -check
    RSA key ok

#### Verify a Certificate was Signed by a CA
 
    openssl verify -verbose -CAFile ca.crt server.crt

#### Convert PEM to DER

    openssl x509 -in server.crt -outform der -out server.der

#### Convert DER to PEM

    openssl x509 -inform der -in server.der -out server.crt

#### Encrypt, Sign, Decrypt mechanism using public key cryptography

- Creating public, private key pairs using openssl command.
    - `openssl genrsa -out private.pem 3072` [generate a private key with the correct length]
    - `openssl rsa -in private-key.pem -pubout -out public-key.pem` [openssl rsa -in private-key.pem -pubout -out public-key.pem]
- Take the secret file and compute the hash and encrypt it using private key. [This yields a signature].
    - `openssl dgst -sha256 [filename]` to get the file digest
    - `openssl dgst -sha256 -sign private-key.pem -out sign.sha256  message.txt` to sign a file
- Encrypt a file using receivers public key.
    - `openssl rsautl -in message.txt -out message.txt.enc -inkey public-key.pem -pubin -encrypt`
- On the receiving sid decrypt the message using receivers public key.
    - `openssl rsautl -in message.txt.enc -out message.txt -inkey private.pem -decrypt`
- Now you have decrypted the message, how will you verify the integrity of the message.
    - `openssl dgst -sha256 -verify [sender_public_key] -signature [signature] [decrypted_message]`
