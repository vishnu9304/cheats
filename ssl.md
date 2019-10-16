**Certificate extensions**

    An SSL Certificate is essentially an X.509 certificate, it defines the structure of the certificate.
    These certificate files will have different extensions based on the format and encoding they use.

**Types of encoding**
    
    Binary
    Base64 ASCII

**Formats**

    PEM uses Base64 ASCII encoding
    DER uses Binary encoding

**PEM file extensions (pem, crt, key, cer)**

    The .pem file can include the server certificate, the intermediate certificate and the private key in a single file
    The server certificate and intermediate certificate can also be in a separate .crt or .cer file
    The private key can be in a .key file
    
    Each certificate in the PEM file is contained between the ---- BEGIN CERTIFICATE---- and ----END CERTIFICATE---- statements
    The private key is contained between the ---- BEGIN RSA PRIVATE KEY----- and -----END RSA PRIVATE KEY----- statements
    The CSR is contained between the -----BEGIN CERTIFICATE REQUEST----- and -----END CERTIFICATE REQUEST----- statements

**DER file extensions (der, cer)**

    The DER certificates are in binary form, contained in .der or .cer files.
    These certificates are mainly used in Java-based web servers.
    
**OpenSSL commands**

Print the certificate in text form and don't print certificate output.
    
    openssl x509 -in <certificate path> -text -noout


