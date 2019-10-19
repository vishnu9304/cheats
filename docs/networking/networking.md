### OSI Model - Open Systems Interconnect

    Physical Layer - Twisted, Co-axial, Wireless, Copper cables etc.    

    Datalink Layer - Ethernet Protocol (Wired/Wireless), docsis3 Protocol (ISP -> Internet) - Data Over Cable Service Interface Specification. Datalink Layer is the place where data movement takes place from one device to an another device.

    Network Layer - IP Addressing and IP Routing. Allows end to end communication, where as Ethernet allows only device to device communications.

    Transport Layer - Transmission Control Protocol & Universal Datagram Protocol. Builds session between client and the server.

    Application Layer - http, https, ssh etc.

### Application Layer Protocols

    Data Transfer Protocols     - HTTP/HTTPS, protocol used to transfer hypertext documents.
                                - FTP (20 Auth, 21 Data Transfer), SFTP (22), TFTP (69) tranferring files. TFTP for sending small files   without authentication.
    Email Protocols             - POP3 110/995, SMTP 25/465, IMAP 143/993
    Authentication Protocols    - LDAP 389, LDAPS 636
    Network Service Protocols   - DHCP, DNS 53, NTP    
    Network Management Protocols- TELNET 23, SSH 22, SNMP
    Audio Visual Protocols      - H.323 1720/1721 (Audio/Video comms), SIP 5060/5061 (Session Initiation Protocol)

### Layer 4 & Layer 7

    Layer 4 - Transport Layer Protocols and Uinversal Datagram Protocols.
    
#### 3-way Handshake

    Step 1: SYN from client to server
    Step 2: ACK + SYN from server
    Step 3: ACK from client

    We user layer4 to establish the session and layer7 for actual data transfer.

#### 4-way Disconnect

    Step 1: FIN from server
    Step 2: FIN + ACK from client
    Step 3: FIN from client
    Step 4: FIN + ACK from server

    We can quicky end the session with out following 4-way Disconnect sequence by using RST (RESET) message.