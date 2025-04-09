# Rendu TP PKI

## 1. Création d'une autorité de certification (CA)

- Génération de la clé privée et certificat auto-signé
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl genrsa -aes256 -out demoCA/private/cakey.pem 4096
    Enter PEM pass phrase: motdepasse
    Verifying - Enter PEM pass phrase: motdepasse
    [it4@efrei-xmg4agau1 ~]$ openssl req -x509 -new -key demoCA/private/cakey.pem -days 1825 -out demoCA/cacert.pem -config ./demoCA/openssl.cnf
    Enter pass phrase for demoCA/private/cakey.pem: motdepasse
```

- Initialisation des fichiers CA
```bash
    [it4@efrei-xmg4agau1 ~]$ mkdir -p demoCA/{certs,crl,newcerts,private}
    [it4@efrei-xmg4agau1 ~]$ touch demoCA/index.txt
    [it4@efrei-xmg4agau1 ~]$ echo 1000 > demoCA/serial
    [it4@efrei-xmg4agau1 ~]$ echo 1000 > demoCA/crlnumber
```

- Configuration de openssl.cnf
```bash
    [ v3_ca ]
    basicConstraints = critical,CA:TRUE
    keyUsage = critical,keyCertSign,cRLSign
    subjectKeyIdentifier = hash
    authorityKeyIdentifier = keyid:always

    [ usr_cert ]
    basicConstraints = CA:FALSE
    keyUsage = digitalSignature, keyEncipherment
    extendedKeyUsage = serverAuth
    subjectKeyIdentifier = hash
    authorityKeyIdentifier = keyid:always

    [ crl_ext ]
    authorityKeyIdentifier=keyid:always
```
- Justificatif v3_ca :
    CA:TRUE : Indique explicitement qu'il s'agit d'un certificat d'autorité certificatrice
    keyCertSign/cRLSign : Limite strictement l'usage aux signatures de certificats et CRL
    critical : Rend ces extensions obligatoires pour la validation
    Durée : 5 ans (1825 jours) pour stabilité de l'infrastructure

- Justificatif usr_cert : 
    CA:FALSE : Empêche toute délégation de confiance non contrôlée
    keyUsage :
        digitalSignature : Authentification TLS
        keyEncipherment : Chiffrement des clés de session
    serverAuth : Restreint l'usage aux serveurs TLS
    Durée : 1 an (365 jours) pour rotation régulière

- Justifactif crl_ext :
    authorityKeyIdentifier : Permet de tracer l'origine de la CRL
    Durée CRL : 30 jours (default_crl_days) pour actualisation fréquente
    Génération automatique : Via crlnumber pour séquence incrémentielle


## 2. Génération d’un certificat serveur

- Création CSR et signature
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl genrsa -out server.key 2048
    [it4@efrei-xmg4agau1 ~]$ openssl req -new -key server.key -out server.csr -config ./demoCA/openssl.cnf
    [it4@efrei-xmg4agau1 ~]$ openssl ca -in server.csr -out server.crt -config ./demoCA/openssl.cnf -extensions usr_cert
    Using configuration from ./demoCA/openssl.cnf
    Enter pass phrase for ./demoCA/private/cakey.pem: motdepasse
    Check that the request matches the signature
    Signature ok
    The Subject's Distinguished Name is as follows
    countryName           :PRINTABLE:'FR'
    stateOrProvinceName   :ASN.1 12:'IDF'
    localityName          :ASN.1 12:'Paris'
    organizationName      :ASN.1 12:'ACME Corp'
    commonName            :ASN.1 12:'ACME Corp Root CA'
    Certificate is to be certified until Apr  8 10:22:10 2026 GMT (365 days)
    Sign the certificate? [y/n]:y


    1 out of 1 certificate requests certified, commit? [y/n]y
    Write out database with 1 new entries
    Database updated
```

- Vérification des extensions
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl x509 -in server.crt -text | grep -A 1 "Key Usage"
                X509v3 Key Usage: 
                    Digital Signature, Key Encipherment
                X509v3 Extended Key Usage: 
                    TLS Web Server Authentication
```


## 3. Mise en place d’une CRL

- Révocation et génération CRL
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl ca -revoke server.crt -config ./demoCA/openssl.cnf
    Using configuration from ./demoCA/openssl.cnf
    Enter pass phrase for ./demoCA/private/cakey.pem: motdepasse
    Revoking Certificate 1000.
    Database updated
    [it4@efrei-xmg4agau1 ~]$ openssl ca -gencrl -out demoCA/crl/ca.crl -config ./demoCA/openssl.cnf
    Using configuration from ./demoCA/openssl.cnf
    Enter pass phrase for ./demoCA/private/cakey.pem: motdepasse
```

- Vérification CRL
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl crl -in demoCA/crl/ca.crl -text -noout
    Certificate Revocation List (CRL):
            Version 2 (0x1)
            Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=FR, ST=IDF, L=Paris, O=ACME Corp, CN=ACME Corp Root CA
            Last Update: Apr  8 10:25:56 2025 GMT
            Next Update: May  8 10:25:56 2025 GMT
            CRL extensions:
                X509v3 CRL Number: 
                    4096
    Revoked Certificates:
        Serial Number: 1000
            Revocation Date: Apr  8 10:25:38 2025 GMT
        Signature Algorithm: sha256WithRSAEncryption
        Signature Value:
            05:a1:d9:c0:e3:47:94:63:b2:2e:e2:5a:40:9a:e5:64:02:2b:
            5a:3a:ab:37:9d:30:c1:8f:e3:39:d8:0a:7f:5d:43:0f:85:68:
            c2:43:6a:68:0a:d6:4d:ed:89:03:c5:28:8f:4c:24:e2:65:70:
            26:db:af:ed:ca:ae:99:ed:fa:2d:2e:e5:b3:01:f3:2e:bd:6d:
            a0:1c:0e:c1:27:39:ef:da:8a:5e:77:0f:c2:33:bd:f8:68:73:
            00:b7:d7:44:bf:95:a4:ee:c2:34:8c:13:67:5c:97:26:e6:58:
            f8:92:f2:3a:96:d6:b2:31:3f:b1:be:5e:38:27:1f:42:c7:3f:
            fd:31:59:cb:6d:31:ae:c1:48:8d:1c:2b:e9:35:53:aa:50:f3:
            71:e4:7a:16:71:d0:90:85:e4:a6:00:3e:d9:c9:7b:08:c3:ff:
            b2:c0:ad:1f:5d:26:1a:a7:54:0f:61:53:f9:bc:a6:c2:d8:be:
            94:fa:de:cf:6f:f5:57:81:c3:10:5a:38:ff:4a:16:d6:5d:5f:
            98:99:8f:3a:1f:03:ac:bb:eb:14:a6:35:69:ac:78:0d:71:c3:
            c0:96:b2:7a:ed:29:20:20:1c:e2:5e:98:95:f1:46:fe:6b:f6:
            f1:f5:4b:f9:31:d9:c9:4d:10:10:46:a7:28:44:a8:22:1a:a5:
            34:b7:d5:4f:75:6a:5c:fd:db:8c:25:86:c1:99:a1:7d:01:76:
            bf:c2:ea:2f:a0:ab:05:0d:5b:18:ea:12:ec:7c:e9:18:91:ab:
            ad:90:05:e5:47:19:c4:38:16:b5:67:03:6c:05:41:c4:3a:20:
            e0:a0:23:3c:25:5d:1d:8e:a2:92:fb:81:aa:3d:68:22:84:3c:
            8a:5e:f4:cd:7f:90:e9:75:c3:b2:b7:32:bb:ed:c8:1a:e3:0d:
            bb:29:f6:de:f9:c3:20:9a:3f:d1:ed:5d:21:d8:3f:e6:91:01:
            99:4b:d1:f1:ff:22:52:d7:f0:91:fc:d8:52:4d:e9:36:4d:2c:
            9d:f9:d1:88:c4:1c:25:75:5d:ba:08:27:38:2d:ea:2a:fd:68:
            f2:02:17:f0:ab:fe:25:73:fb:39:d0:09:67:d5:c2:8c:8e:89:
            e7:13:69:70:e6:6b:0d:1a:3e:d6:5e:4d:2d:38:8d:5c:42:90:
            7c:98:94:f8:04:5f:e0:a2:27:f9:85:e4:57:b8:ec:b6:74:e8:
            7a:72:73:5b:51:1c:3c:a7:98:96:d7:04:14:b5:a2:80:23:76:
            cc:36:80:48:74:e5:54:4c:a0:d9:a9:ed:c5:58:06:00:fc:b2:
            dd:84:eb:bd:93:85:d3:3c:0d:94:b0:96:e5:87:93:19:44:40:
            b0:67:30:58:f7:a1:ea:65
```

- Pour la distribution d'une CRL (Certificate Revocation List) dans un environnement réel, voici les méthodes courantes :
    * La méthode la plus simple est de publier la CRL sur un serveur web accessible via HTTP. Les clients peuvent télécharger la CRL à partir de l'URL spécifiée dans le champ crlDistributionPoints du certificat.
    * Les CRL peuvent être stockées dans un serveur LDAP (Lightweight Directory Access Protocol).Les clients peuvent interroger le serveur LDAP pour récupérer la CRL.


## 4. Déploiement et test du service OCSP

- Mise en place
```bash
    it4@efrei-xmg4agau1 ~]$ openssl ocsp -index demoCA/index.txt -port 2560 -rsigner demoCA/cacert.pem -rkey demoCA/private/cakey.pem -CA demoCA/cacert.pem -text
    ACCEPT 0.0.0.0:2560 PID=2106
    Enter pass phrase for demoCA/private/cakey.pem:
    ocsp: waiting for OCSP client connections...
    ocsp: received request, 1st line: POST / HTTP/1.0
    OCSP Request Data:
        Version: 1 (0x0)
        Requestor List:
            Certificate ID:
              Hash Algorithm: sha1
              Issuer Name Hash: A1BCF5987DF3D7F091F8F38F06643BDCB79CD003
              Issuer Key Hash: 30DC5582AEA5272343B5C7CB9E2F294EA0A78EA3
              Serial Number: 1000
        Request Extensions:
            OCSP Nonce: 
                04102E6082EBC668C2AD9742D5672D754C04
    ocsp: sending response, 1st line: HTTP/1.0 200 OK
    OCSP Response Data:
        OCSP Response Status: successful (0x0)
        Response Type: Basic OCSP Response
        Version: 1 (0x0)
        Responder Id: C = FR, ST = IDF, L = Paris, O = ACME Corp, CN = ACME Corp Root CA
        Produced At: Apr  8 10:37:42 2025 GMT
        Responses:
        Certificate ID:
          Hash Algorithm: sha1
          Issuer Name Hash: A1BCF5987DF3D7F091F8F38F06643BDCB79CD003
          Issuer Key Hash: 30DC5582AEA5272343B5C7CB9E2F294EA0A78EA3
          Serial Number: 1000
        Cert Status: revoked
        Revocation Time: Apr  8 10:25:38 2025 GMT
        This Update: Apr  8 10:37:42 2025 GMT

        Response Extensions:
            OCSP Nonce: 
                04102E6082EBC668C2AD9742D5672D754C04
        Signature Algorithm: sha256WithRSAEncryption
        Signature Value:
            07:9d:f9:27:ae:c7:78:45:51:2d:2f:56:91:de:76:8c:0a:1a:
            57:f9:99:2e:b8:94:6a:e3:a1:a9:35:bf:80:15:d4:e0:a9:04:
            3d:c8:6a:40:cc:2a:1b:0a:1c:b7:2f:d2:12:ad:ce:0a:88:6a:
            48:bf:e4:88:0b:76:dd:8f:8c:e4:c1:5b:d3:1c:53:35:8f:f5:
            55:52:e6:8d:75:6f:72:15:8d:a7:33:af:dc:0e:1c:bf:a1:63:
            bc:2b:41:ac:3b:12:7b:b7:da:47:82:a2:9f:10:78:16:23:a5:
            43:11:ef:0b:e6:e1:b9:c9:b0:ba:55:16:49:88:da:61:99:bd:
            4e:12:22:1e:a3:8c:9b:0e:50:ae:92:80:fd:44:4f:ad:be:9d:
            47:f4:59:9c:2e:fe:cc:0f:13:35:70:df:1a:f8:b2:85:64:0a:
            c4:7b:c5:1c:d4:a3:c9:9e:f3:cc:4b:7b:dd:0b:72:d1:40:9c:
            c2:40:59:45:97:90:ab:f1:aa:dd:b5:05:dd:51:8e:60:2f:61:
            e8:9b:79:cb:81:78:f6:e5:f3:d2:2e:ef:20:c3:c5:01:c2:68:
            60:f7:d9:31:a3:e9:43:d1:52:ab:72:02:db:58:89:aa:9e:b7:
            1e:7c:3f:c5:92:56:19:32:5d:37:8a:5a:75:fb:74:62:55:58:
            f2:3b:9a:56:34:93:62:a0:81:9e:07:f2:98:09:69:e7:d6:de:
            67:5c:a3:1b:d8:c0:74:a5:78:c7:16:f8:78:3b:67:04:04:5f:
            8b:be:e1:65:c3:4f:9d:ef:e6:19:05:0a:5c:b9:46:25:d0:d2:
            eb:d8:56:7b:ed:e9:30:63:a1:e3:45:59:f9:61:46:ae:ca:89:
            e3:f0:20:3a:c7:55:92:c7:b2:ee:e7:13:7a:05:c1:a9:5f:0b:
            46:da:19:7a:4b:4e:3a:0b:48:37:53:d6:0d:af:c6:e2:59:39:
            1b:3a:15:3b:0c:34:e8:18:55:59:7b:c1:3f:c7:15:2a:23:7e:
            1c:a4:46:5b:3e:bf:50:10:9b:27:86:da:f4:08:8f:75:ef:c2:
            c7:e4:db:16:44:f1:a6:32:58:ec:cd:19:4a:de:dd:40:24:3c:
            76:ba:a5:71:d5:64:35:2e:ec:39:0b:14:0d:bf:87:19:57:65:
            1a:ce:8b:6f:e8:2c:67:10:a1:09:ec:81:53:7d:ff:8c:02:ab:
            e7:24:8f:c5:6d:4b:e3:52:55:1e:5c:bf:dc:b7:9d:f9:15:ed:
            91:27:48:e1:a1:9e:7a:48:0c:07:90:11:0a:f9:2a:8f:31:56:
            75:8e:46:ee:45:d2:98:a9:bb:b4:85:65:cd:0b:34:77:28:a5:
            ed:b4:61:76:bb:9e:e1:e9
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                56:33:06:a4:ac:df:f8:c7:6b:9d:29:3c:9c:3a:df:0f:9a:f6:df:2f
            Signature Algorithm: sha256WithRSAEncryption
            Issuer: C=FR, ST=IDF, L=Paris, O=ACME Corp, CN=ACME Corp Root CA
            Validity
                Not Before: Apr  8 10:08:09 2025 GMT
                Not After : Apr  7 10:08:09 2030 GMT
            Subject: C=FR, ST=IDF, L=Paris, O=ACME Corp, CN=ACME Corp Root CA
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (4096 bit)
                    Modulus:
                        00:b5:57:90:b2:47:15:07:09:bc:95:c5:75:01:62:
                        25:ca:86:2f:e9:f7:44:1d:9f:30:e9:1f:96:81:df:
                        a9:e9:0c:8f:cd:fb:b0:62:54:7e:6f:e7:df:e8:a5:
                        80:ea:e0:e1:4b:f7:d8:fc:ee:97:f9:2d:5c:7b:f1:
                        ea:15:f3:c7:cb:54:d2:ba:9d:28:5c:47:ae:ab:3f:
                        90:c8:ee:f2:1f:7c:aa:99:a7:60:04:45:b2:9b:75:
                        c2:71:09:1b:c6:1f:c9:b7:a2:6c:99:3f:a1:74:2e:
                        c9:72:28:51:26:d7:a7:3e:d3:18:52:93:de:f2:23:
                        2a:02:59:36:66:7d:51:b6:5e:ce:eb:17:2e:bf:92:
                        84:95:d2:70:ed:81:48:1d:41:b7:9a:34:a0:34:cc:
                        e2:ea:82:35:2c:dd:df:56:23:2e:fa:ca:95:40:e5:
                        cf:bc:82:1f:e0:98:ce:5e:72:25:94:37:40:63:1b:
                        9e:10:d0:dc:f6:79:01:c7:9f:51:fc:ce:26:39:eb:
                        f6:3b:05:8c:d5:4f:c8:a6:aa:9c:ce:1e:7e:3e:f6:
                        69:b2:e1:4f:10:ab:ea:01:d5:0b:4f:07:58:d9:80:
                        1d:42:d9:81:aa:2e:c3:d3:30:ed:cc:7d:09:73:47:
                        f9:cf:7c:0e:eb:f3:0f:e9:ab:13:b1:b0:b2:47:a6:
                        7e:6e:fb:85:2c:ba:1a:74:e7:8c:ca:be:f0:5c:c4:
                        f9:64:fa:82:3d:36:4f:45:40:ef:19:22:19:8e:64:
                        a5:15:1c:be:64:15:be:58:86:af:ab:69:1e:8e:39:
                        9b:15:02:f3:d1:a3:8d:14:d2:4a:01:45:ad:0b:ce:
                        20:43:23:95:c2:ca:13:c9:8d:af:77:65:3a:dd:64:
                        75:91:76:01:d0:e2:0b:86:53:bc:3e:00:fc:9d:4b:
                        27:71:82:79:38:e2:99:03:50:94:6c:46:f8:dd:c9:
                        9d:4c:fd:3a:da:89:73:5f:98:43:ed:6d:bb:df:76:
                        6a:5d:7b:eb:1b:a0:7e:11:ee:27:ac:e7:02:dd:8b:
                        32:14:58:c7:62:86:db:bb:2a:a0:12:d7:6a:02:4b:
                        a4:13:5d:9d:66:3a:0f:5b:79:b2:fc:67:13:df:42:
                        80:73:c8:fb:ad:8e:7f:2b:d7:a2:b7:a9:4b:7f:bf:
                        06:14:71:30:7f:42:de:37:6b:e6:6d:6e:41:ed:8d:
                        e9:cc:fd:ea:23:53:6c:c2:79:7a:90:ad:fa:f6:a3:
                        3e:b4:bd:a9:c1:1f:9c:a4:80:aa:05:02:9e:7a:79:
                        96:95:6a:52:93:d8:fc:97:75:2a:a6:47:35:50:65:
                        b1:7e:e6:a8:4c:bf:11:5a:4c:f5:28:6a:f1:f2:84:
                        a1:15:69
                    Exponent: 65537 (0x10001)
            X509v3 extensions:
                X509v3 Basic Constraints: critical
                    CA:TRUE
                X509v3 Key Usage: critical
                    Certificate Sign, CRL Sign
                X509v3 Subject Key Identifier: 
                    30:DC:55:82:AE:A5:27:23:43:B5:C7:CB:9E:2F:29:4E:A0:A7:8E:A3
                X509v3 Authority Key Identifier: 
                    30:DC:55:82:AE:A5:27:23:43:B5:C7:CB:9E:2F:29:4E:A0:A7:8E:A3
        Signature Algorithm: sha256WithRSAEncryption
        Signature Value:
            5b:a4:39:66:a8:cd:0f:c6:0d:24:9d:ee:df:86:e5:9f:be:bd:
            f3:32:b3:6e:4c:f4:f2:6c:00:79:c8:22:38:e0:76:22:01:e9:
            6c:f1:55:f7:75:f2:3f:9a:0b:6b:f5:cc:f0:86:21:95:3c:ec:
            78:84:c5:af:e5:13:e8:19:3b:7c:eb:5c:da:9a:41:29:ae:f0:
            dd:27:18:29:3a:b7:9f:1b:c0:8b:ee:de:a2:0b:f8:a9:9a:fa:
            2d:a6:d3:b3:c1:c7:8a:ac:da:5c:b7:c6:8e:13:22:11:73:75:
            8f:c2:09:d8:47:b2:81:80:f6:28:8c:b2:1c:7b:1c:93:53:25:
            dc:76:67:50:d1:9f:ec:36:6b:cd:98:a9:ca:b6:72:5c:c2:e7:
            85:3d:0d:0d:be:a3:e6:0a:75:53:b0:0b:3d:ec:98:dc:37:d1:
            95:e5:be:9f:7a:2f:95:01:30:79:5d:53:f2:5d:ff:70:65:45:
            ab:f6:75:a4:89:92:26:9f:61:bb:a1:3d:67:6a:ae:6e:dc:88:
            0a:7f:1b:6c:46:04:b4:1d:48:0d:55:67:2a:03:80:ab:7f:8d:
            d1:ab:ed:34:d7:11:e5:a0:6f:b8:58:8d:1d:62:94:da:56:a2:
            f7:a4:cc:01:50:27:9b:50:cf:c6:e4:23:a9:ec:36:58:cf:ab:
            ba:a4:7b:3f:fa:b9:26:b0:24:c7:dd:83:2e:f8:d5:b6:75:43:
            3b:29:34:66:d6:e4:07:da:99:20:24:03:b1:0a:1d:5a:b8:47:
            9f:04:15:36:6b:99:f2:de:05:d2:03:f0:13:6c:3f:de:1a:81:
            4c:be:33:5a:0d:84:85:f7:51:c3:6a:d7:85:ea:a5:f2:e2:d7:
            a9:36:10:06:15:34:b6:00:4c:ee:29:c0:60:d5:18:c1:98:85:
            20:a5:65:04:1d:5a:13:40:a7:89:b9:f4:14:09:d0:28:bb:07:
            0e:56:7b:92:63:e1:46:58:89:f7:11:42:8a:73:79:8f:81:0b:
            e9:f2:a6:1f:2d:64:19:24:cd:54:77:c2:15:fd:09:4f:71:7b:
            73:ff:41:01:a8:cc:ee:f6:41:7f:fe:e0:11:ea:54:e7:67:24:
            d6:45:fb:2d:2f:29:2b:0a:d8:7a:22:68:e5:87:b3:f9:b7:e9:
            40:cd:3e:92:48:40:e0:91:91:69:99:ed:e6:27:8e:a8:74:0a:
            a0:4a:14:2d:94:49:dd:1f:11:22:90:dd:3f:24:65:a7:0a:f2:
            8b:a3:60:18:ef:c1:55:2b:e7:48:97:7c:d7:a9:1e:81:c8:33:
            9c:8e:23:fb:8e:a0:e0:ae:49:08:83:85:95:2a:35:9e:82:79:
            40:71:59:e3:7d:e5:9e:04
    -----BEGIN CERTIFICATE-----
    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
    noJ5QHFZ433lngQ=
    -----END CERTIFICATE-----
```

- Vérification
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl ocsp -CAfile demoCA/cacert.pem -issuer demoCA/cacert.pem -cert server.crt -url http://localhost:2560
    Response verify OK
    server.crt: revoked
	    This Update: Apr  8 10:37:42 2025 GMT
	    Revocation Time: Apr  8 10:25:38 2025 GMT
```

- Comparer le fonctionnement avec celui de la CRL
    | Caractéristique            | CRL                                             | OCSP                                        |
    | -------------------------- | ----------------------------------------------- | ------------------------------------------- |
    | Méthode de vérification    | Téléchargement périodique de listes complètes   | Requêtes en temps réel à un serveur OCSP    |
    | Fraîcheur des informations | Dépend de la fréquence de mise à jour de la CRL | Information en temps réel                   |
    | Confidentialité            | Pas de confidentialité (liste publique)         | Peut être configuré pour la confidentialité |


## 5. Lancer un serveur HTTPS

- Lancement du serveur
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl s_server -cert server.crt -key server.key -WWW -port 4443
    Using default temp DH parameters
    ACCEPT
```

- Test de connexion
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl ocsp -CAfile demoCA/cacert.pem -issuer demoCA/cacert.pem -cert server.crt -url http://localhost:2560
    Response verify OK
    server.crt: revoked
	    This Update: Apr  8 10:37:42 2025 GMT
	    Revocation Time: Apr  8 10:25:38 2025 GMT
    [it4@efrei-xmg4agau1 ~]$ curl -v https://localhost:4443 --cacert demoCA/cacert.pem
    *   Trying ::1:4443...
    * Connected to localhost (::1) port 4443 (#0)
    * ALPN, offering h2
    * ALPN, offering http/1.1
    *  CAfile: demoCA/cacert.pem
    * TLSv1.0 (OUT), TLS header, Certificate Status (22):
    * TLSv1.3 (OUT), TLS handshake, Client hello (1):
    * TLSv1.2 (IN), TLS header, Certificate Status (22):
    * TLSv1.3 (IN), TLS handshake, Server hello (2):
    * TLSv1.2 (IN), TLS header, Finished (20):
    * TLSv1.2 (IN), TLS header, Unknown (23):
    * TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8):
    * TLSv1.2 (IN), TLS header, Unknown (23):
    * TLSv1.3 (IN), TLS handshake, Certificate (11):
    * TLSv1.2 (IN), TLS header, Unknown (23):
    * TLSv1.3 (IN), TLS handshake, CERT verify (15):
    * TLSv1.2 (IN), TLS header, Unknown (23):
    * TLSv1.3 (IN), TLS handshake, Finished (20):
    * TLSv1.2 (OUT), TLS header, Finished (20):
    * TLSv1.3 (OUT), TLS change cipher, Change cipher spec (1):
    * TLSv1.2 (OUT), TLS header, Unknown (23):
    * TLSv1.3 (OUT), TLS handshake, Finished (20):
    * SSL connection using TLSv1.3 / TLS_AES_256_GCM_SHA384
    * ALPN, server did not agree to a protocol
    * Server certificate:
    *  subject: C=FR; ST=IDF; O=ACME Corp; CN=ACME Corp Root CA
    *  start date: Apr  8 10:22:10 2025 GMT
    *  expire date: Apr  8 10:22:10 2026 GMT
    * SSL: certificate subject name 'ACME Corp Root CA' does not match target host name 'localhost'
    * Closing connection 0
    * TLSv1.2 (OUT), TLS header, Unknown (23):
    * TLSv1.3 (OUT), TLS alert, close notify (256):
    curl: (60) SSL: certificate subject name 'ACME Corp Root CA' does not match target host name 'localhost'
    More details here: https://curl.se/docs/sslcerts.html

    curl failed to verify the legitimacy of the server and therefore could not
    establish a secure connection to it. To learn more about this situation and
    how to fix it, please visit the web page mentioned above.
```

### Questions :
#### 1. Pourquoi le navigateur indique-t-il une alerte de sécurité ?
        Car le nom de sujet du certificat (ACME Corp Root CA) ne correspond pas au nom d'hôte auquel on veux accéder (localhost).

#### 2. Quelle est la durée de validité du certificat serveur ? Où cette info est-elle visible ?
        La durée de validité du certificat serveur est d'un an, du "Apr 8 10:22:10 2025 GMT" au "Apr 8 10:22:10 2026 GMT". Visible sous la section "Server certificate".

#### 3. Quelles informations contient le certificat ?
        Subject (Sujet): Identité à laquelle le certificat a été délivré (ici, ACME Corp Root CA).
        Issuer (Émetteur): L'entité qui a signé et délivré le certificat.
        Serial Number (Numéro de série): Un identifiant unique pour le certificat.
        Valid From (Valide à partir de): La date de début de validité du certificat.
        Valid To (Valide jusqu'à): La date d'expiration du certificat.
        Public Key (Clé publique): La clé publique associée à l'identité.
        Signature Algorithm (Algorithme de signature): L'algorithme utilisé pour signer le certificat.
        Extensions: Des informations supplémentaires comme les usages de la clé (keyUsage, extendedKeyUsage).     

#### 4. Quel est le rôle d’une CRL ? Quelles limites présente-t-elle ?
        Une CRL (Certificate Revocation List) est une liste de certificats qui ont été révoqués par l'autorité de certification (CA) avant leur date d'expiration prévue. Elle permet aux parties qui font confiance à la CA de vérifier si un certificat est toujours valide.
        Comme limite nous aurons; Latence : La CRL n'est mise à jour qu'à intervalles réguliers. Un certificat révoqué récemment peut ne pas apparaître immédiatement dans la CRL. 
                                  Taille : Les CRL peuvent devenir volumineuses, ce qui peut impacter la bande passante et le temps de téléchargement.

#### 5. Quelle différence avec OCSP ? Pourquoi est-il plus utilisé ?
        OCSP (Online Certificate Status Protocol) est un protocole qui permet de vérifier l'état d'un certificat en temps réel en interrogeant un serveur OCSP. À la différence de la CRL, OCSP fournit une réponse immédiate sur l'état d'un certificat spécifique. Il est plus utilisé car l'état du certificat est vérifié en temps réel et le client n'a besoin de télécharger que l'information concernant un seul certificat, ce qui réduit la bande passante et le temps de réponse.

#### 6. Comment tester si un certificat est bien révoqué (via CRL et via OCSP) ?
        - Via CRL :
            Générer une CRL après avoir révoqué le certificat : openssl ca -gencrl -out demoCA/crl/ca.crl -config openssl.cnf
            Vérifier si le numéro de série du certificat révoqué apparaît dans la CRL : openssl crl -in demoCA/crl/ca.crl -text -noout | grep <numéro_de_série>
        - Via OCSP :
            Lancer un serveur OCSP local : openssl ocsp -index demoCA/index.txt -port 2560 -rsigner demoCA/cacert.pem -rkey demoCA/private/cakey.pem -CA demoCA/cacert.pem -text
            Interroger le serveur OCSP pour vérifier l'état du certificat : openssl ocsp -CAfile demoCA/cacert.pem -issuer demoCA/cacert.pem -cert server.crt -url http://localhost:2560
            La réponse doit indiquer revoked.

#### 7. En quoi la configuration d’un fichier openssl.cnf est-elle cruciale dans un environnement professionnel ? expliquez chaque partie du fichier. 
        Le fichier openssl.cnf est crucial car il définit la politique de l'autorité de certification (CA) et contrôle la manière dont les certificats sont créés, signés et gérés. Une configuration incorrecte peut entraîner des vulnérabilités de sécurité ou un fonctionnement incorrect de l'infrastructure PKI.
    Explication des sections :
        [ca]: Définit les paramètres par défaut de la CA.
            default_ca: Nom de la section contenant les paramètres spécifiques de la CA.
        [CA_default]: Paramètres spécifiques de la CA.
            dir: Répertoire racine de la CA.
            certs, crl_dir, new_certs_dir: Répertoires pour stocker les certificats, les CRL et les nouveaux certificats.
            database: Fichier index pour suivre les certificats.
            serial, crlnumber: Fichiers contenant les numéros de série.
            certificate, private_key: Chemin vers le certificat et la clé privée de la CA.
            default_md: Algorithme de hachage par défaut.
            default_days: Durée de validité par défaut des certificats.
            x509_extensions: Extensions X.509 par défaut.
            policy: Politique à suivre lors de la signature des certificats.
        [policy_strict]: Définit les règles pour les champs du nom distinctif (DN) des certificats.
            countryName, stateOrProvinceName, organizationName: Doivent correspondre à ceux de la CA.
            organizationalUnitName, emailAddress: Facultatifs.
            commonName: Doit être fourni.
        [req]: Paramètres par défaut pour les demandes de signature de certificat (CSR).
            default_bits: Taille de la clé privée par défaut.
            prompt: Indique si les champs doivent être demandés interactivement.
            distinguished_name: Nom de la section contenant les informations du DN.
            x509_extensions: Extensions X.509 à inclure dans le certificat auto-signé de la CA.
        [req_distinguished_name]: Informations du DN pour les CSR.
            C, ST, L, O, CN: Champs du DN (Pays, État, Localité, Organisation, Nom commun).
        [v3_ca]: Extensions pour les certificats de CA.
            basicConstraints: Indique que le certificat est pour une CA.
            keyUsage: Définit les usages de la clé (signature de certificats, signature de CRL).
            subjectKeyIdentifier, authorityKeyIdentifier: Identifiants de clé pour faciliter la vérification.
        [usr_cert]: Extensions pour les certificats d'utilisateur (serveur).
            basicConstraints: Indique que le certificat n'est pas pour une CA.
            keyUsage: Définit les usages de la clé (signature numérique, chiffrement de clé).
            extendedKeyUsage: Définit les usages étendus (authentification serveur).
        [crl_ext]: Extensions pour les CRL.
            authorityKeyIdentifier: Identifiant de clé de l'autorité qui a signé la CRL.

