#### Basic Concepts

    Encryption: Conversion of plain text to cipher text. (Sender)

    Decryption: Conversion of cipher text to plain text. (Receiver)

    Cryptography: Study of Encryption

    Cryptanalysis: Study of Decryption

    Cryptology: Encryption + Decryption

#### Encryption can be done in two ways:
    
    Stream Cipher: Conversion takes place bit by bit. (Recommended for short length messages
        
    Block Cipher: Conversion takes place block by block (grouping of bits), block size is determined by the type of algorithm.
    
#### Two type of Encryption mechanism:

    Symmetric Encryption: Same key will be used for Encryption process and Decryption process (Secret key).
    
    Asymmetric Encryption: Two keys will be used that are mathematically linked (Public and Private keys).
    
    Note: It is possible to encrypt data with private key and decrypt it with public key.

#### Types of Attacks

    Passive attacks: No modification will be done to the data by unauthorized user.
    
    Active attacks: Data will modified by unauthorized user.

    Passive attacks types:

        Eavesdropping - No modification will be done to the data, the attacker will just listen to the messages.
        Traffic Analysis - Just Analysing the traffic. Based on the traffic flow access will be gained.
    
    Active attacks types:

        Masquerade attacks - Receiver will receive the data from 3rd party in the name of sender.

        Replay attacks - Alice (A) sends her hashed password to Bob (B). Eve (E) sniffs the hash and replays it pretending like alice.

        Data Modification attacks - Sender sends the data, the attacker takes it and modifies it send it to receiver.

        Denial Of Service attacks - Disrupts the services provided by the server.

#### Security Services

    Authentication - Acess control. Prevent un-authorized access.

    Authorization - Verification of user identity.

    Confidentiality - Securely sharing the data.

    Integrity - Make sure the data is not modified during transmission.

    Non-Repudiation.

    Substitution Techniques - The characters are replaced with corresponding cipher texts which are calulated mathematically (Vulnerable to attacks, suitable only for short length methods).
                              Example: Caesar cipher
                              
    Transposition Techniques - Re-arrange the order of bit positions.
                              Example: Rail fence Cipher
                                        Columnar transposition

#### FEISTEL STRUCTURE

    Most of the block cipher techniques follows a strcture called as FEISTEL STRUCTURE.
    FEISTEL STRUCTURE - Surf online for more info.

    Block ciper design priciples:
        block size
        key size
        number of rounds
        subkeys

#### Symmentric Algorithms
    
    DES (Data Encyption Standard)
    AES  (Advanced Encyption Standard)

#### ASymmentric Algorithms

    RSA

    Diffe Hellman Key Exchange.
        This is not an Encryption algorithm.
        It is used only to exchange the secret/symmetric keys.      