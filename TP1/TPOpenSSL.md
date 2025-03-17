# TP OpenSSL

## 1. Chiffrement
### 1.1. Générer une clé privée RSA de 256 bits (priv256.key), 1024 bits (priv1024.key) et sans donner de
longueur (priv.key)
```bash
	[it4@efrei-xmg4agau1 ~]$ openssl genrsa -out priv256.key 256
	Error setting RSA length
	00FE57325F7F0000:error:1C8000AB:Provider routines:rsa_gen_set_params:key size too small:providers/implementations/keymgmt/rsa_kmgmt.c:526:
	[it4@efrei-xmg4agau1 ~]$ openssl genrsa -out priv1024.key 1024
	[it4@efrei-xmg4agau1 ~]$ openssl genrsa -out priv.key 
```
#### De quelle taille est la clé que vous avez créée sans donner la taille en paramètre ?
```bash
	[it4@efrei-xmg4agau1 ~]$ openssl rsa -in priv.key -text -noout | grep "Private-Key"
	Private-Key: (2048 bit, 2 primes)
```
#### Quelle est votre conclusion ?
	
	- OpenSSL ne permet plus la génération de clés RSA de 256 bits, car cette taille est considérée comme trop petite et non sécurisée.
	- La taille par défaut utilisée est généralement de 2048 bits

#### Existe-il d’autres formats que .key ?

	- Oui tel que .pem, .der ...

### 1.2. Afficher priv.key à l’aide de ces deux commandes :
```bash
	[it4@efrei-xmg4agau1 ~]$ cat priv.key 
	-----BEGIN PRIVATE KEY-----
	MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQDEI/klbnTNG/Jp
	K7auYxMZjZnoGUAGy3IeJbo41dRU8flxYHRSSX/6bWkW1tP7kIko3QnNrmXdan3/
	ljjQhf7WTRy4LT2Ti0MppMepG1XgvndVdSjgqghc8F1phQt+acNS/7yjl8jdykmR
	S86EtW6xhbI+hCe1sZvnA+mWNH37d87+3MN31td6NXdHs0MDVFHmnXDg7agsnUvO
	O9gvcUfSrmNmP1BxYAI8kNArAW4tw6J4r2xgXElTTUPRzcttaoAMBJHRqLWGm18H
	379cfbjH2of3cmdPuEYf6v/shjd/rW1JRil/PKxitIv9/eVmQXmLVQsNUim4s3qx
	v3q2FWMdAgMBAAECggEABlgShi0rLnotrkypaT0PoFc15+2vJjkc3RBD1HRIHKJB
	+vKPeJzjfI5e5Iv4yc18PjveBg7SwSVhY2uwtT/sRPK0D9n4wKWzu6xWf+gAMFeA
	xR0XwCKHyDfHEbNpsoJJvMzERXdSmifhlsdArAaxSKEmKYL9RNF0lTWJb+ANRh2k
	rmblpeqowNzFgR7VOLq+Notdp+sXuqayyb1RzFu6MHIZgRpxManAOBVloTFm/vYN
	mJ3I8Giu1NbKxxt9lSTJLc0SUWwj0kabAuK+PRPYHbQhRCfAP3eEBhq5IAyOV7O5
	oRLXxp1P9g3UBcL5zKgXy66s1GeVlgPHZm4SlVclyQKBgQDmErkhBriU2NndZbUu
	etHM4J8ZYz0XtsG89dGyMNhAKpHVYex3U0M5WV0fKbW1fQz2IEX9p+bwxksRiyva
	7zU3YWIrO2KXSgSx7g4H8gctZ1aDoPkzxZa90Vk50pHzI1mjN1eGsOyH0PadMbTR
	myfd1gneYsP9bpj36rduVEnXOQKBgQDaPlfcbve25eQlgTdA0VZ54MHIOYQLCqdc
	6jcO/ysYHunOwTZ2MfSVN+hGkQmEae/hbtvqLvK8RevPUWD5FOIgKxJ1voZ1YbQj
	qrYTV7vfZXAAzREFBGhsyYrz7jbDcFYqKoMURgO/UheIvMRQjenR1v3hghP0uUmm
	thETAS6nBQKBgHzisOcCdPMm+qu4565BMNzbGMF5ItJae7OMY7Ur2laKRPrk2qyy
	34yju26M/3tyU7TOM+/KUgtkN59gQf/nVKWpENaSt6OTpBTJOYiKkebNbyKoIF0/
	5eFHX/2JFXw7Ng7onXQZWWsnvJX7Q0F7zRoXcufqCKTqgdIg8EzNJ//RAoGAc6da
	FDzSFSCa2K0zRKwq8YeD6cFhEiDhNEICT3Y1tiCbsq99VwF+JZ1tAAhYTM1/C64d
	6GpcxB0hq8nqY9UHSBjLPY5r3sSaG9SZjIKP0wEEa8hAXrJJTG05r67rYoKjhtDk
	idfYoIi2ZAX02uN5p3QRMnImdSfDug792V5lBKkCgYEAtGWXp6nrDJ/3V8TsuaqH
	a9jhhq3weojPVo40UkBXMTG4EK9VaWHc8MLjoWggfwNAxmXJyqmL4ch0yaAkD/Vm
	DULe9Aa6RpLjzHyEmfsVrpkouj+YVcDE5/Rk4yrs0wsw4Dbez4isAkplTaTf5ASW
	MhxExR7+hN0+F/aWVKFk3Pw=
	-----END PRIVATE KEY-----
	[it4@efrei-xmg4agau1 ~]$ openssl rsa -text -in priv.key
	Private-Key: (2048 bit, 2 primes)
	modulus:
	    00:c4:23:f9:25:6e:74:cd:1b:f2:69:2b:b6:ae:63:
	    13:19:8d:99:e8:19:40:06:cb:72:1e:25:ba:38:d5:
	    d4:54:f1:f9:71:60:74:52:49:7f:fa:6d:69:16:d6:
	    d3:fb:90:89:28:dd:09:cd:ae:65:dd:6a:7d:ff:96:
	    38:d0:85:fe:d6:4d:1c:b8:2d:3d:93:8b:43:29:a4:
	    c7:a9:1b:55:e0:be:77:55:75:28:e0:aa:08:5c:f0:
	    5d:69:85:0b:7e:69:c3:52:ff:bc:a3:97:c8:dd:ca:
	    49:91:4b:ce:84:b5:6e:b1:85:b2:3e:84:27:b5:b1:
	    9b:e7:03:e9:96:34:7d:fb:77:ce:fe:dc:c3:77:d6:
	    d7:7a:35:77:47:b3:43:03:54:51:e6:9d:70:e0:ed:
	    a8:2c:9d:4b:ce:3b:d8:2f:71:47:d2:ae:63:66:3f:
	    50:71:60:02:3c:90:d0:2b:01:6e:2d:c3:a2:78:af:
	    6c:60:5c:49:53:4d:43:d1:cd:cb:6d:6a:80:0c:04:
	    91:d1:a8:b5:86:9b:5f:07:df:bf:5c:7d:b8:c7:da:
	    87:f7:72:67:4f:b8:46:1f:ea:ff:ec:86:37:7f:ad:
	    6d:49:46:29:7f:3c:ac:62:b4:8b:fd:fd:e5:66:41:
	    79:8b:55:0b:0d:52:29:b8:b3:7a:b1:bf:7a:b6:15:
	    63:1d
	publicExponent: 65537 (0x10001)
	privateExponent:
	    06:58:12:86:2d:2b:2e:7a:2d:ae:4c:a9:69:3d:0f:
	    a0:57:35:e7:ed:af:26:39:1c:dd:10:43:d4:74:48:
	    1c:a2:41:fa:f2:8f:78:9c:e3:7c:8e:5e:e4:8b:f8:
	    c9:cd:7c:3e:3b:de:06:0e:d2:c1:25:61:63:6b:b0:
	    b5:3f:ec:44:f2:b4:0f:d9:f8:c0:a5:b3:bb:ac:56:
	    7f:e8:00:30:57:80:c5:1d:17:c0:22:87:c8:37:c7:
	    11:b3:69:b2:82:49:bc:cc:c4:45:77:52:9a:27:e1:
	    96:c7:40:ac:06:b1:48:a1:26:29:82:fd:44:d1:74:
	    95:35:89:6f:e0:0d:46:1d:a4:ae:66:e5:a5:ea:a8:
	    c0:dc:c5:81:1e:d5:38:ba:be:36:8b:5d:a7:eb:17:
	    ba:a6:b2:c9:bd:51:cc:5b:ba:30:72:19:81:1a:71:
	    31:a9:c0:38:15:65:a1:31:66:fe:f6:0d:98:9d:c8:
	    f0:68:ae:d4:d6:ca:c7:1b:7d:95:24:c9:2d:cd:12:
	    51:6c:23:d2:46:9b:02:e2:be:3d:13:d8:1d:b4:21:
	    44:27:c0:3f:77:84:06:1a:b9:20:0c:8e:57:b3:b9:
	    a1:12:d7:c6:9d:4f:f6:0d:d4:05:c2:f9:cc:a8:17:
	    cb:ae:ac:d4:67:95:96:03:c7:66:6e:12:95:57:25:
	    c9
	prime1:
	    00:e6:12:b9:21:06:b8:94:d8:d9:dd:65:b5:2e:7a:
	    d1:cc:e0:9f:19:63:3d:17:b6:c1:bc:f5:d1:b2:30:
	    d8:40:2a:91:d5:61:ec:77:53:43:39:59:5d:1f:29:
	    b5:b5:7d:0c:f6:20:45:fd:a7:e6:f0:c6:4b:11:8b:
	    2b:da:ef:35:37:61:62:2b:3b:62:97:4a:04:b1:ee:
	    0e:07:f2:07:2d:67:56:83:a0:f9:33:c5:96:bd:d1:
	    59:39:d2:91:f3:23:59:a3:37:57:86:b0:ec:87:d0:
	    f6:9d:31:b4:d1:9b:27:dd:d6:09:de:62:c3:fd:6e:
	    98:f7:ea:b7:6e:54:49:d7:39
	prime2:
	    00:da:3e:57:dc:6e:f7:b6:e5:e4:25:81:37:40:d1:
	    56:79:e0:c1:c8:39:84:0b:0a:a7:5c:ea:37:0e:ff:
	    2b:18:1e:e9:ce:c1:36:76:31:f4:95:37:e8:46:91:
	    09:84:69:ef:e1:6e:db:ea:2e:f2:bc:45:eb:cf:51:
	    60:f9:14:e2:20:2b:12:75:be:86:75:61:b4:23:aa:
	    b6:13:57:bb:df:65:70:00:cd:11:05:04:68:6c:c9:
	    8a:f3:ee:36:c3:70:56:2a:2a:83:14:46:03:bf:52:
	    17:88:bc:c4:50:8d:e9:d1:d6:fd:e1:82:13:f4:b9:
	    49:a6:b6:11:13:01:2e:a7:05
	exponent1:
	    7c:e2:b0:e7:02:74:f3:26:fa:ab:b8:e7:ae:41:30:
	    dc:db:18:c1:79:22:d2:5a:7b:b3:8c:63:b5:2b:da:
	    56:8a:44:fa:e4:da:ac:b2:df:8c:a3:bb:6e:8c:ff:
	    7b:72:53:b4:ce:33:ef:ca:52:0b:64:37:9f:60:41:
	    ff:e7:54:a5:a9:10:d6:92:b7:a3:93:a4:14:c9:39:
	    88:8a:91:e6:cd:6f:22:a8:20:5d:3f:e5:e1:47:5f:
	    fd:89:15:7c:3b:36:0e:e8:9d:74:19:59:6b:27:bc:
	    95:fb:43:41:7b:cd:1a:17:72:e7:ea:08:a4:ea:81:
	    d2:20:f0:4c:cd:27:ff:d1
	exponent2:
	    73:a7:5a:14:3c:d2:15:20:9a:d8:ad:33:44:ac:2a:
	    f1:87:83:e9:c1:61:12:20:e1:34:42:02:4f:76:35:
	    b6:20:9b:b2:af:7d:57:01:7e:25:9d:6d:00:08:58:
	    4c:cd:7f:0b:ae:1d:e8:6a:5c:c4:1d:21:ab:c9:ea:
	    63:d5:07:48:18:cb:3d:8e:6b:de:c4:9a:1b:d4:99:
	    8c:82:8f:d3:01:04:6b:c8:40:5e:b2:49:4c:6d:39:
	    af:ae:eb:62:82:a3:86:d0:e4:89:d7:d8:a0:88:b6:
	    64:05:f4:da:e3:79:a7:74:11:32:72:26:75:27:c3:
	    ba:0e:fd:d9:5e:65:04:a9
	coefficient:
	    00:b4:65:97:a7:a9:eb:0c:9f:f7:57:c4:ec:b9:aa:
	    87:6b:d8:e1:86:ad:f0:7a:88:cf:56:8e:34:52:40:
	    57:31:31:b8:10:af:55:69:61:dc:f0:c2:e3:a1:68:
	    20:7f:03:40:c6:65:c9:ca:a9:8b:e1:c8:74:c9:a0:
	    24:0f:f5:66:0d:42:de:f4:06:ba:46:92:e3:cc:7c:
	    84:99:fb:15:ae:99:28:ba:3f:98:55:c0:c4:e7:f4:
	    64:e3:2a:ec:d3:0b:30:e0:36:de:cf:88:ac:02:4a:
	    65:4d:a4:df:e4:04:96:32:1c:44:c5:1e:fe:84:dd:
	    3e:17:f6:96:54:a1:64:dc:fc
	writing RSA key
	-----BEGIN PRIVATE KEY-----
	MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQDEI/klbnTNG/Jp
	K7auYxMZjZnoGUAGy3IeJbo41dRU8flxYHRSSX/6bWkW1tP7kIko3QnNrmXdan3/
	ljjQhf7WTRy4LT2Ti0MppMepG1XgvndVdSjgqghc8F1phQt+acNS/7yjl8jdykmR
	S86EtW6xhbI+hCe1sZvnA+mWNH37d87+3MN31td6NXdHs0MDVFHmnXDg7agsnUvO
	O9gvcUfSrmNmP1BxYAI8kNArAW4tw6J4r2xgXElTTUPRzcttaoAMBJHRqLWGm18H
	379cfbjH2of3cmdPuEYf6v/shjd/rW1JRil/PKxitIv9/eVmQXmLVQsNUim4s3qx
	v3q2FWMdAgMBAAECggEABlgShi0rLnotrkypaT0PoFc15+2vJjkc3RBD1HRIHKJB
	+vKPeJzjfI5e5Iv4yc18PjveBg7SwSVhY2uwtT/sRPK0D9n4wKWzu6xWf+gAMFeA
	xR0XwCKHyDfHEbNpsoJJvMzERXdSmifhlsdArAaxSKEmKYL9RNF0lTWJb+ANRh2k
	rmblpeqowNzFgR7VOLq+Notdp+sXuqayyb1RzFu6MHIZgRpxManAOBVloTFm/vYN
	mJ3I8Giu1NbKxxt9lSTJLc0SUWwj0kabAuK+PRPYHbQhRCfAP3eEBhq5IAyOV7O5
	oRLXxp1P9g3UBcL5zKgXy66s1GeVlgPHZm4SlVclyQKBgQDmErkhBriU2NndZbUu
	etHM4J8ZYz0XtsG89dGyMNhAKpHVYex3U0M5WV0fKbW1fQz2IEX9p+bwxksRiyva
	7zU3YWIrO2KXSgSx7g4H8gctZ1aDoPkzxZa90Vk50pHzI1mjN1eGsOyH0PadMbTR
	myfd1gneYsP9bpj36rduVEnXOQKBgQDaPlfcbve25eQlgTdA0VZ54MHIOYQLCqdc
	6jcO/ysYHunOwTZ2MfSVN+hGkQmEae/hbtvqLvK8RevPUWD5FOIgKxJ1voZ1YbQj
	qrYTV7vfZXAAzREFBGhsyYrz7jbDcFYqKoMURgO/UheIvMRQjenR1v3hghP0uUmm
	thETAS6nBQKBgHzisOcCdPMm+qu4565BMNzbGMF5ItJae7OMY7Ur2laKRPrk2qyy
	34yju26M/3tyU7TOM+/KUgtkN59gQf/nVKWpENaSt6OTpBTJOYiKkebNbyKoIF0/
	5eFHX/2JFXw7Ng7onXQZWWsnvJX7Q0F7zRoXcufqCKTqgdIg8EzNJ//RAoGAc6da
	FDzSFSCa2K0zRKwq8YeD6cFhEiDhNEICT3Y1tiCbsq99VwF+JZ1tAAhYTM1/C64d
	6GpcxB0hq8nqY9UHSBjLPY5r3sSaG9SZjIKP0wEEa8hAXrJJTG05r67rYoKjhtDk
	idfYoIi2ZAX02uN5p3QRMnImdSfDug792V5lBKkCgYEAtGWXp6nrDJ/3V8TsuaqH
	a9jhhq3weojPVo40UkBXMTG4EK9VaWHc8MLjoWggfwNAxmXJyqmL4ch0yaAkD/Vm
	DULe9Aa6RpLjzHyEmfsVrpkouj+YVcDE5/Rk4yrs0wsw4Dbez4isAkplTaTf5ASW
	MhxExR7+hN0+F/aWVKFk3Pw=
	-----END PRIVATE KEY-----
```
#### Quelle est la differnce entre les deux affichages ?

	- La commande cat priv.key affiche le contenu brut du fichier de clé privée au format PEM tandis que la commande openssl rsa -text -in priv.key décode et affiche les détails de la clé RSA de manière structurée et lisible

### 1.3. Chiffrer priv.key à l’aide d’un cryptosystème symétrique de votre choix (privC.key)
```bash
	[it4@efrei-xmg4agau1 ~]$ openssl enc -aes-256-cbc -salt -in priv.key -out privC.key
	enter AES-256-CBC encryption password:
	Verifying - enter AES-256-CBC encryption password:
	*** WARNING : deprecated key derivation used.
	Using -iter or -pbkdf2 would be better.
```
### 1.4. Afficher le contenue de privC.key
```bash
	[it4@efrei-xmg4agau1 ~]$ cat privC.key
	Salted__��"�Ѻ���5�db�
		             �8T���<-0���Onk
	�!�(�!�%���(<$y؏M������c�rb۠�?D$�?q"�Kg�O�B��.�=������ܔ���Z%��Z��/:/��t|5��+!#�c��TDF�-�/ߖ�����5�X;�7�+8�f��P�p�+�Bv�[�|����0+e+��b���p�Q��r �iH�]v�c�QV���@a�_�f�� �2a+i���_8�?�)��g@��)6�m�r$L��A��
	�Ȕ�,���ob��/i�W�Lfb������X��}��������
		                             �P�j
	��Q��InR�K��6�*�!b�c�Ä�1�^��0�Jl�������f1�f����ЯL�x��i}KZ���d���A��M	��a�4�w���K�:
		                                                                             ʧ��<����AԯmW���"U�zS
		                                                                                                 ���w&qu���
		                                                                                                           ������	&��X/�����I��'����	=)�2
	z�r�%���6                                                                                                                                                   w����PJ8��!F���EH���q@-��0m���3�L�(�o@ym����#E��u�
		 ����_�0��q��r��Rt1�>;q�=Y.W� ���%3�[�����efsp�$��0?�r��靪S��*)�b �V/

		                                                                     9��|'���(dq�C*��!����פ��U�>l(G��8��)��	�(I�����a�3��B�1A��އ��>�t�{b�ǳ!M�~��h~<�ű�gxV�Ew�v@э�b;�5�UB�ڐ�,F�+j�']�,KrK�/^4w)��"�b��JH���5��c@𒒅�����=���.1�۹�&���_�Ub6DcX��1��e��֩�(�P���H�y��`�����[)���<��V��4޶=�0mPlw�o����n���c�Ã�B�C
	��*I�%����g�>-|M!���)���ɇk:��n��N=��������$�҈�-�4�þܬdj�:e%�ԉX{L�0�-�c�� t�-�)�q
	�� ,�3�Z��Sd��A�N���X�:���&P㣃1�_��T��`�Ds����H{1|M1��l(�	3��3L�
		                                                              ~��'=�_,%�;��J�������C�U�0�Gi1Χ�f<ҏ�\����u��nmB̢֯������������I(�k��/��X�;lS��۪���3��,p�D11�p�(�iZOؚ�������W��iD��+1C��8?��:������$;k۟;�e���j7���Bx��%�,ͦ��ؘpk�޲
```
#### Qu’est-ce que le Salted ?

	- C'est une technique de sécurité utilisée en cryptographie pour renforcer la protection des mots de passe ou d'autres données sensibles avant leur hachage

#### Peut-on utiliser cette clé telle quelle à l’avenir 

	- Oui, la clé privée chiffrée (privC.key) peut être utilisée à l'avenir, mais avec certaines précautions :

### 1.5. Utiliser une seule commande pour générer une clé privée chiffrer avec aes128 (priv.key)
```bash
	[it4@efrei-xmg4agau1 ~]$ openssl genrsa -aes128 -out priv.key 2048
	Enter PEM pass phrase: admin
	Verifying - Enter PEM pass phrase: admin
```
### 1.6. Générer la clé publique (pub.key)
```bash
	[it4@efrei-xmg4agau1 ~]$ openssl rsa -in priv.key -pubout -out pub.key
	Enter pass phrase for priv.key: admin
	writing RSA key
```
#### A-t-on besoin de spécifier la taille de la clé publique ? pour quelle raison ?

	- Non car la clé publique est dérivée de la clé privée existante. Sa taille est donc déjà déterminée par la taille de la clé privée générée précédemment

### 1.7. Créer deux fichiers texte 
```bash
	[it4@efrei-xmg4agau1 ~]$ vi Texte.txt
	[it4@efrei-xmg4agau1 ~]$ vi TexteLong.txt
```
### 1.8. Chiffrer Texte.txt et TexteLong.txt avec un cryptosystème symétrique de votre choix, et afficher le résultat
```bash
	[it4@efrei-xmg4agau1 ~]$ openssl enc -aes-256-cbc -salt -in Texte.txt -out Texte_chiffre.txt
	enter AES-256-CBC encryption password:
	Verifying - enter AES-256-CBC encryption password:
	*** WARNING : deprecated key derivation used.
	Using -iter or -pbkdf2 would be better.
	[it4@efrei-xmg4agau1 ~]$ openssl enc -aes-256-cbc -salt -in TexteLong.txt -out TexteLong_chiffre.txt
	enter AES-256-CBC encryption password:
	Verifying - enter AES-256-CBC encryption password:
	*** WARNING : deprecated key derivation used.
	Using -iter or -pbkdf2 would be better.
	[it4@efrei-xmg4agau1 ~]$ cat Texte_chiffre.txt
	Salted__]e��F�"�T���>��[����3��IL��8B��ψ�k{�I@u8�m6�C�;;_���h#l�����'}�LA|U���r�w
		                                                                         �%,�c��";C��Ҽ
	[it4@efrei-xmg4agau1 ~]$ cat TexteLong_chiffre.txt 
	Salted__µ��m������m'����b���TC��Ì�����%

	F������'�\�i�A~hdg}԰�o�=�b4JIe�/X���`TS��)�4c0=���p�o����3�k�K�M�m�8�#E�Yڏ�*�D��i)F-�Ww��;��uQ@��M͘�:)����;,�\3/KH\$�Xw�XD0���>=,0���/���|?\ل�D"I
		                                                                                                                                        ���AA�p$CA������|u�;M�����C��Z	�j�nC ��#Z_��b�H�h���p08G���O̠/4���8k��xU��\_�����f����+F��A��ӈ�,i|��X4I���tv%�����I�YB
```
### 1.9. Chiffrer Texte.txt avec la clé rsa générée précédemment et nomez le fichier résultant TexteC.txt
```bash
	[it4@efrei-xmg4agau1 ~]$ openssl pkeyutl -encrypt -pubin -inkey pub.key -in Texte.txt -out TexteC.txt
```	
Quelle est la taille du fichier Texte et celle de TexteC ?
```bash
	[it4@efrei-xmg4agau1 ~]$ ls -lh Texte.txt TexteC.txt
	-rw-r--r--. 1 it4 it4 256 Mar 17 13:22 TexteC.txt
	-rw-r--r--. 1 it4 it4  85 Mar 17 13:13 Texte.txt
```
### 1.10. Chiffrer TexteLong.txt avec la clé rsa générée précédemment
```bash
	[it4@efrei-xmg4agau1 ~]$ openssl pkeyutl -encrypt -pubin -inkey pub.key -in TexteLong.txt -out TexteLongC.txt
	Public Key operation error
	003E6952797F0000:error:0200006E:rsa routines:ossl_rsa_padding_add_PKCS1_type_2_ex:data too large for key size:crypto/rsa/rsa_pk1.c:133:
```
#### Quelle est votre conclusion ?

	- Le chiffrement RSA direct ne peut pas être utilisé pour des fichiers de grande taille. La taille maximale des données pouvant être chiffrées avec RSA est limitée par la taille de la clé.


## 2. Signature numérique
### 2.1. Signer le fichier Texte.txt et vérifier sa taille (TexteS.txt)
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl dgst -sha256 -sign priv.key -out TexteS.txt Texte.txt
    Enter pass phrase for priv.key:
    [it4@efrei-xmg4agau1 ~]$ wc -c < TexteS.txt
    256
```
### 2.2. Vérifier la signature avec la clé adéquate
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl dgst -sha256 -verify pub.key -signature TexteS.txt Texte.txt
    Verified OK
```

## 3. Fonction de hachage
### 3.1. Que fait une fonction de hachage, et dans quel but l’utilise-t-on ?
    - Une fonction de hachage est un algorithme qui transforme une donnée d'entrée de taille arbitraire en une donnée de sortie de taille fixe, appelée valeur de hachage, empreinte numérique ou condensé
    - On l'utilise pour vérifier l'intégrité des données
### 3.2. Hacher Texte.txt avec la fonction de votre choix
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl dgst -sha256 Texte.txt
    SHA2-256(Texte.txt)= 0bc054c2d206104ea9e0e7498a0f0964f9f67d473cbb2c2f0b38337d618a08c4
```
#### Chiffrement, signature, hachage dans quel ordre ?
    - Hachage --> Signature numérique --> Chiffrement

## 4. Certificats numériques
### 4.1. Quel est le rôle d’un certificat numérique ? Quelle est sa structure ?
    - Un certificat numérique est un document électronique utilisé pour garantir la sécurité des communications et des transactions en ligne. i.e, Authentification, Confidentialité, Intégrité et Non-répudiation.
    - Sa structure comprend plusieurs éléments, i.e, Version, Numéro de série, Algorithme de signature, Émetteur (Issuer), Validité, Sujet (Subject), Clé publique, Signature numérique.
### 4.2. Saisir le script get-cert.sh 
```bash
    [it4@efrei-xmg4agau1 ~]$ vi get-cert.sh
    [it4@efrei-xmg4agau1 ~]$ chmod +x get-cert.sh
```
### 4.3. Quel est le rôle de la commande s_client et de la commande sed
    - La commande s_client permet de se connecter au serveur spécifié et d'obtenir son certificat SSL/TLS
    - La commande sed est utilisée pour extraire spécifiquement la partie du certificat (entre les balises BEGIN CERTIFICATE et END CERTIFICATE) de la sortie générée par s_client
### 4.4. Exécuter ce script afin de récupérer le certificat d’un site internet de votre choix
```bash
    [it4@efrei-xmg4agau1 ~]$ ./get-cert.sh myefrei.fr
    -----BEGIN CERTIFICATE-----
    MIIGczCCBNugAwIBAgIRALn9sCJ4/xR5Fne2f8ghEeMwDQYJKoZIhvcNAQEMBQAw
    VjELMAkGA1UEBhMCRlIxDjAMBgNVBAoTBUdhbmRpMTcwNQYDVQQDEy5HYW5kaSBS
    U0EgRG9tYWluIFZhbGlkYXRpb24gU2VjdXJlIFNlcnZlciBDQSAzMB4XDTI0MDcw
    MzAwMDAwMFoXDTI1MDcxMjIzNTk1OVowFzEVMBMGA1UEAwwMKi5teWVmcmVpLmZy
    MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA7rRU86SNq94GzXnsU5s2
    WYUQJUq4izw+wd2Lzv1SFLAs2hFSsjEeLMgT3+xoBaQaZnd35kphj4QG1ym/wGxr
    QWevPL4jtPeRwOGoGPYez9G4BQujZZerpKn9C1K4KR/jxwmgSx7T62yoSCwuBTkp
    zeWAh9VdcVKhNUg8NAjCRr6vxO1eoK0dOGWdInalb5ux/6vljYSyvfB9GeHcEbcu
    +VN7nzw7L4WeJIp6MXT0+j5mbDEHSG6culkDiApyj98clFvHrfGWn/kEQPBFUG3C
    zfG/rFBcVcyts8GdWX5fCSgzU18Myxgk+5RxIP4J9U6OYQypBpBCbbe8V7Bpr+0g
    oQIDAQABo4IC+TCCAvUwHwYDVR0jBBgwFoAUgRGS3mYypbBbMz1lQ4X81AQt8a4w
    HQYDVR0OBBYEFDizU/2Z99b7ndkvO7DHNrx2PmiPMA4GA1UdDwEB/wQEAwIFoDAM
    BgNVHRMBAf8EAjAAMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjBJBgNV
    HSAEQjBAMDQGCysGAQQBsjEBAgIaMCUwIwYIKwYBBQUHAgEWF2h0dHBzOi8vc2Vj
    dGlnby5jb20vQ1BTMAgGBmeBDAECATCBgwYIKwYBBQUHAQEEdzB1ME4GCCsGAQUF
    BzAChkJodHRwOi8vY3J0LnNlY3RpZ28uY29tL0dhbmRpUlNBRG9tYWluVmFsaWRh
    dGlvblNlY3VyZVNlcnZlckNBMy5jcnQwIwYIKwYBBQUHMAGGF2h0dHA6Ly9vY3Nw
    LnNlY3RpZ28uY29tMCMGA1UdEQQcMBqCDCoubXllZnJlaS5mcoIKbXllZnJlaS5m
    cjCCAX4GCisGAQQB1nkCBAIEggFuBIIBagFoAHYA3dzKNJXX4RYF55Uy+sef+D0c
    UN/bADoUEnYKLKy7yCoAAAGQeVwFIwAABAMARzBFAiBfXfQwcntbIKdETpXB+tDF
    EzN6/HVNhDpRNb+qZH1L+QIhAN/FphopMk44tswN/ZJa+FGe5q4eeLO3ddz7susa
    10H6AHYADeHyMCvTDcFAYhIJ6lUu/Ed0fLHX6TDvDkIetH5OqjQAAAGQeVwE8gAA
    BAMARzBFAiADswkXLsIzYCDuM/6XotoP3iRSuVtlGFqzrcED2NqPOQIhAOnSCEf+
    PBNb6HwxbcXrB5AF8Upux/q6v+jqZOKJouymAHYAEvFONL1TckyEBhnDjz96E/jn
    tWKHiJxtMAWE6+WGJjoAAAGQeVwEyQAABAMARzBFAiB2PJBnzDMYXNHWhuNzOk2D
    kwoY6LFqhRuycrwlEBfz3gIhAJ3/VgxBb9Npzls7cHgKI262aZyLMFGgj5Oc8X5u
    5RiKMA0GCSqGSIb3DQEBDAUAA4IBgQBEa52g7sohwC+ANTSrFxLwDuxb1p0ggn09
    mDPyiw6KyKkBcLG9mc7V43VUHeoAL3x1FgW4ECAn5o91oamDG2dkbGE0sZmrn6v5
    AM9KJVoMMxX4W3hTceHOrcCw8gQgmGqQJ9ONPqMeuBRxlp8mTKiP0/mMQvBjungI
    51bGK4iaeP3OnTs9iQ5sKHmK/Ax/32GypOTC7mOvRU8XyZlSCw9BOThnTGlDiub4
    3XFULDAwOqBwIj1TpXvkPb/Wa+e/FOBKJHwKLkVgeAe/GTqjzUpnR5z0CTwiXiee
    zRftkoCeh9Y2iKDmB2F8Sfl8hoCfESK6RLYhoBDGxs9q7rKgU7EdpCDXVorggR4L
    fcAkoHwgP+5fXlfpFjnyvW4iAarswFmhKLXthg6/M/XtMdDr8gptCN7jZvSZktjM
    oLYeGQbkN1s0X5xBCvdbE8wS4BHElcqTck5TlUlt5cqhAcW+Li2cl+rSkZKml36z
    7pmV277N0xqjUDtwpoz7Z2nT2tOwsCs=
    -----END CERTIFICATE-----
```
#### Sous quel format est le certificat ?
    - Ce certificat est au format PEM (Privacy Enhanced Mail). On peut le reconnaître par les balises caractéristiques "-----BEGIN CERTIFICATE-----" et "-----END CERTIFICATE-----" qui encadrent le contenu du certificat encodé en Base64
### 4.5. Convertir le certificat au format DER
```bash
    [it4@efrei-xmg4agau1 ~]$ ./get-cert.sh myefrei.fr > myefrei_cert.pem
    [it4@efrei-xmg4agau1 ~]$ openssl x509 -outform der -in myefrei_cert.pem -out myefrei_cert.der
    [it4@efrei-xmg4agau1 ~]$ cat myefrei_cert.der 
    0�s0�۠���*�H����!�0
    0V1
       0	UFR10
                 U
    250712235959Z010URSA Domain Validation Secure Server CA 30

    �0�     *�H��    *.myefrei.fr0�"0
    ��T󤍫��y�S�6Y�%J��<>�݋��R�,�R�1,���h��fww�Ja���)��lkAg�<�#�������Ѹ
                                                                     �e�����
                                                                            R�)��	�K��l�H,.9)�倇�]qR�5H<�F����^��8e�"v�o����卄���}���.�S{�<;/��$�z1t��>fl1Hn��Y�
    r���[ǭ��@�EPm���P\Ṷ���Y~_	(3S_
                                        �$��q �	�N�a
                                                        ��Bm��W�i�� ����0��0U#0����f2��[3=eC���-�0U8�S�������/;��6�v>h�0U��0
                                                                                                                            U�00U%+0IU B0@04
                                                                                                                                            +�1�0%0+https://sectigo.com/CPSg�
                                                                                                                                                                             0�+w0u0+0�Bhttp://crt.sectigo.com/GandiRSADomainValidationSecureServerCA3.crt0+0�http://ocsp.sectigo.com0#U0��
                             *.myefrei.fr�
    myefrei.fr0�~
    +�y�n�jhv���4����2�ǟ�=P��:v
    �@b+���x�U.�Gt|���0�B�~N�4�y\�G0E �:Q5��.�3` �3�����$R�[eZ����ڏ9!�G�<[�|1m����Jn������d≢�v�N4�SrL�Ï?z��b���m0���&:�y\�G0E v<�g�3\�ֆ�s:M��
    �j�r�%��!��V
                Ao�i�[;px
    #n�i��0Q*�H��~n��0
    ��Dk����!�/�54���[֝ �}=�3��ȩp������uT�/|u� '�u���dla4�������J%Z
                                                                  3�[xSq�έ��� �j�'Ӎ>��q��&L�����B�c��V�+��x�Ν;=�l(y��
                                                                                                                     �a�����c�EOəR
                                                                                                                                  A98gLiC����qT,00:�p"=S�{�=��k��J$|
    .E`x�:��JgG��	<"^'��풀���6���a|I�|���"�D�!����jS�� �V���
                                                               }�$�| ?�_^W�9�n"���Y�(���3��1���
    ��f����̠��7[4_�A
    �[��ĕʓrNS�Im�ʡž.-���ґ���~�۾����P;p���gi��Ӱ�+ 
```
#### Quelle est la différence entre les deux formats ?
    - La principale différence entre les formats PEM et DER est l'Encodage, i.e, PEM (format texte encodé en base64, lisible avec un éditeur de texte standard) tandis que DER(format binaire, non lisible en texte brut.)
#### Quelle sont les autres formats possibles ?
    - CRT/CER
    - JKS
    - CSR/P10
    - KEY ...
### 4.6. Créer un certificat x509 auto signé
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365
    .........+.......+..+.+++++++++++++++++++++++++++++++++++++++++++++*.......+..+.+++++++++++++++++++++++++++++++++++++++++++++*....+........+...+.......+........+...+.......+............+.....+...+.......+......+............+..+............+...+.+...............+......+.....+................+............+....................+...+...+...............+....+...+........+.........+.+..+...+...............+............+.......+......+......+........+......+.+..................+...+..+.+...........+...+.+..+..........+.....+..........+........+....+......+...........+....+........................+......+.....+......+.+..+.......+...+..+.........+.+.....+.+...................................+....+...........+....+...+.....+......+.+......+...+..+...+...............+..........+.....+....+........+..................+.........+............+....+......+.....+.+..............+...+...+.......+...........+.......+............+........+....+...+......+.....+......................+...........................+...+...............+.....+............+.+..+.......+.....+.+.........+.....+......+.+......+.........+...+...+..+................+..+...+......+.............+...............+......+.....+....+..+.........+............+..........+.....+...+.+.....+....+.....+......+.............+..+................+..+......................+...........+...+.+......+..+......+......+....+..............+.......+..................+...+...+.....+.+...+.....+.+..............+.+...........+.+.....+...+.....................+..........+.....+......+..................+.........+.+.....+......+.+...+........................+..+....+..+.........+....+......+..............+.........+...+..........+..............+....+...........+..........+...+...........+................+.....+......+....+........+.+..+....+.........+......+.........+...........+...+......+...............+..........+.........+..+......+...+.+...........+..........+...............+........+....+..+.+.............................+...+..........+......+..+..........+..+.+..+...+....+..+.+.................+.+......+...............+...+...............+..+......+............+.+.....+.+......+..+...+............+............+...+.+.........+.................+.+........+............+............+.+..+.......+.....................+.....+.+..............+......+.............+.................+...+...+.............+.........+......+..+...+.+.....+....+.....+.+.....+.............+...+........+..........+............+.....+.......+..................+...+..............+.+.....+..........+......+...........+.........+.............+..+.............+.....................+.....+....+...+..+............+......+...+....+........+..........+...+.....+....+..+....+..+.........+......+....+........................+.....+......+...+.+.....+.+...........+....+......+.................+..........+..+................+...............+........+.+......+...............+.........+...+........+.......+.....+...+.......+..+...+...+...+.+...........+....+...............+.....+.............+..+......+.........+.............+..+.+...+......+.....+..........+......+...............+..............+..........+...+...............+...+.....+...+..........+...........+....+.....................+...+....................+......+............+..........+.................+..........+......+.........+.....+......+.........+......+..........+..+.......+........+....+.....+.+...+...........+.+.....+.......+......+.........+......+......+........+............+....+..+...+.+...+.....+..................+...+....+.....+.......+...............+......+........................+.....+.+..+..........+..+...+......+....+..+.+.....+............+.+.........+..........................+......+...+.......+......+..+...............+...+...+......+....+...............+...............+..+.......+...+..............................+...+....................+.+.........+.........+..+......+............+......+....+..................+...+.....+.......+.........+.....+......+++++
    ..+++++++++++++++++++++++++++++++++++++++++++++*.....+++++++++++++++++++++++++++++++++++++++++++++*.........+....+..+.........+...+.+.................+...+.........+..............................+......+.........+...+...+......+.+...+.........+...+.....+.......+......+.....+++++
    Enter PEM pass phrase: admin
    Verifying - Enter PEM pass phrase: admin
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [XX]:FR
    State or Province Name (full name) []:Nouvelle-Aquitaine
    Locality Name (eg, city) [Default City]:Bordeaux
    Organization Name (eg, company) [Default Company Ltd]:Toxic
    Organizational Unit Name (eg, section) []:Ghost
    Common Name (eg, your name or your server's hostname) []:Marafa
    Email Address []:it4@yopmail.com
```
#### Sous quel format est le certificat ?
    - Le certificat auto-signé généré est au format X.509
#### Quelles sont les étapes à suivre ?
    - Exécuter la commande : ```bash openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365 ```
    - Spécifier un mot de passe pour protéger la clé privée
    - Fournir les informations demandées lors de l'exécution de la commande, telles que le pays, l'état, la localité, l'organisation, et le nom commun (CN)

## 5. Tester les liaisons SSL
### 5.1. Lancer le serveur SSL d’OpenSSL en utilisant la commande s_server, sur le port 10000 en utilisant le certificat autosigné que vous avez généré précédemment
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl s_server -accept 10000 -cert cert.pem -key key.pem -www
    Enter pass phrase for key.pem:
    Using default temp DH parameters
    ACCEPT
```
### 5.2. Lancer Wireshark, puis une connexion avec le serveur à partir d’un client OpenSSL (s-client)
#### Expliquer les étapes de connexion ?
    - Client Hello 
    - Server Hello 
    - Certificate
    - Server Key Exchange
    - Server Hello Done :
    - Client Key Exchange
    - Change Cipher Spec
    - Finished
#### Quels sont les paquets interceptés par Wireshark ?
    - Paquets TCP pour établir la connexion (SYN, SYN-ACK, ACK)
    - Paquets TLS pour le handshake (Client Hello, Server Hello, etc.)
    - Paquets TLS chiffrés pour la communication après l'établissement de la session sécurisée

### 5.3. Télécharger le certificat d’un site web de votre choix, puis extraire le nom du CA
```bash
    [it4@efrei-xmg4agau1 ~]$ openssl s_client -connect www.example.com:443 -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM > example_cert.pem
    [it4@efrei-xmg4agau1 ~]$ openssl x509 -in example_cert.pem -noout -issuer
    issuer=C=US, O=DigiCert Inc, CN=DigiCert Global G3 TLS ECC SHA384 2020 CA1
```
### 5.4. Récupérer le certificat du CA, et vérifier la signature du certificat du site
```bash

```
### 5.5. Télécharger le script testssl.sh (github), puis tester un site web de votre choix
```bash
    [it4@localhost ~]$ git clone --depth 1 https://github.com/testssl/testssl.sh.git
    Cloning into 'testssl.sh'...
    remote: Enumerating objects: 108, done.
    remote: Counting objects: 100% (108/108), done.
    remote: Compressing objects: 100% (101/101), done.
    remote: Total 108 (delta 13), reused 49 (delta 7), pack-reused 0 (from 0)
    Receiving objects: 100% (108/108), 8.85 MiB | 3.29 MiB/s, done.
    Resolving deltas: 100% (13/13), done.
    [it4@localhost ~]$ cd testssl.sh
    [it4@localhost testssl.sh]$ chmod +x testssl.sh
    [it4@localhost testssl.sh]$ ./testssl.sh www.example.com

    #####################################################################
      testssl.sh version 3.2rc4 from https://testssl.sh/dev/
      (0c64e09 2025-03-17 10:26:57)

      This program is free software. Distribution and modification under
      GPLv2 permitted. USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

      Please file bugs @ https://testssl.sh/bugs/
    #####################################################################

      Using OpenSSL 1.0.2-bad (Sep 1 2022)  [~183 ciphers]
      on localhost:./bin/openssl.Linux.x86_64

    Testing all IPv4 addresses (port 443): 23.33.90.74 23.33.90.72
    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
     Start 2025-03-18 00:25:57        -->> 23.33.90.74:443 (www.example.com) <<--

     Further IP addresses:   23.33.90.72 2a02:26f0:9100:28::17c0:ed95 2a02:26f0:9100:28::17c0:ed88 
     rDNS (23.33.90.74):     a23-33-90-74.deploy.static.akamaitechnologies.com.
     Service detected:       HTTP

     Testing protocols via sockets except NPN+ALPN 

     SSLv2      not offered (OK)
     SSLv3      not offered (OK)
     TLS 1      not offered
     TLS 1.1    not offered
     TLS 1.2    offered (OK)
     TLS 1.3    offered (OK): final
     NPN/SPDY   h2, h2-14, http/1.1, http/1.0 (advertised)
     ALPN/HTTP2 h2, http/1.1 (offered)

     Testing cipher categories 

     NULL ciphers (no encryption)                      not offered (OK)
     Anonymous NULL Ciphers (no authentication)        not offered (OK)
     Export ciphers (w/o ADH+NULL)                     not offered (OK)
     LOW: 64 Bit + DES, RC[2,4], MD5 (w/o export)      not offered (OK)
     Triple DES Ciphers / IDEA                         not offered
     Obsoleted CBC ciphers (AES, ARIA etc.)            not offered
     Strong encryption (AEAD ciphers) with no FS       not offered
     Forward Secrecy strong encryption (AEAD ciphers)  offered (OK)


     Testing server's cipher preferences 

    Hexcode  Cipher Suite Name (OpenSSL)       KeyExch.   Encryption  Bits     Cipher Suite Name (IANA/RFC)
    -----------------------------------------------------------------------------------------------------------------------------
    SSLv2
     - 
    SSLv3
     - 
    TLSv1
     - 
    TLSv1.1
     - 
    TLSv1.2 (server order -- server prioritizes ChaCha ciphers when preferred by clients)
     xc02c   ECDHE-ECDSA-AES256-GCM-SHA384     ECDH 256   AESGCM      256      TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384            
     xc02b   ECDHE-ECDSA-AES128-GCM-SHA256     ECDH 256   AESGCM      128      TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256            
     xcca9   ECDHE-ECDSA-CHACHA20-POLY1305     ECDH 256   ChaCha20    256      TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256      
    TLSv1.3 (server order -- server prioritizes ChaCha ciphers when preferred by clients)
     x1302   TLS_AES_256_GCM_SHA384            ECDH 253   AESGCM      256      TLS_AES_256_GCM_SHA384                             
     x1303   TLS_CHACHA20_POLY1305_SHA256      ECDH 253   ChaCha20    256      TLS_CHACHA20_POLY1305_SHA256                       
     x1301   TLS_AES_128_GCM_SHA256            ECDH 253   AESGCM      128      TLS_AES_128_GCM_SHA256                             

     Has server cipher order?     yes (OK) -- TLS 1.3 and below


     Testing robust forward secrecy (FS) -- omitting Null Authentication/Encryption, 3DES, RC4 

     FS is offered (OK)           TLS_AES_256_GCM_SHA384 TLS_CHACHA20_POLY1305_SHA256 ECDHE-ECDSA-AES256-GCM-SHA384 ECDHE-ECDSA-CHACHA20-POLY1305 TLS_AES_128_GCM_SHA256 ECDHE-ECDSA-AES128-GCM-SHA256 
     Elliptic curves offered:     prime256v1 X25519 
     TLS 1.2 sig_algs offered:    ECDSA+SHA256 ECDSA+SHA384 ECDSA+SHA512 ECDSA+SHA224 ECDSA+SHA1 
     TLS 1.3 sig_algs offered:    ECDSA+SHA256 

     Testing server defaults (Server Hello) 

     TLS extensions (standard)    "server name/#0" "max fragment length/#1" "status request/#5" "EC point formats/#11" "application layer protocol negotiation/#16" "session ticket/#35" "supported versions/#43" "key share/#51" "next protocol/#13172"
                                  "renegotiation info/#65281"
     Session Ticket RFC 5077 hint 83100 seconds, session tickets keys seems to be rotated < daily
     SSL Session ID support       yes
     Session Resumption           Tickets: yes, ID: yes
     TLS clock skew               Random values, no fingerprinting possible 
     Certificate Compression      none
     Client Authentication        none
     Signature Algorithm          ECDSA with SHA384
     Server key size              EC 256 bits (curve P-256)
     Server key usage             Digital Signature, Key Agreement
     Server extended key usage    TLS Web Server Authentication, TLS Web Client Authentication
     Serial                       0AD893BAFA68B0B7FB7A404F06ECAF9A (OK: length 16)
     Fingerprints                 SHA1 310DB7AF4B2BC9040C8344701ACA08D0C69381E3
                                  SHA256 455943CF819425761D1F950263EBF54755D8D684C25535943976F488BC79D23B
     Common Name (CN)             *.example.com  (CN in response to request w/o SNI: a248.e.akamai.net )
     subjectAltName (SAN)         *.example.com example.com 
     Trust (hostname)             Ok via SAN wildcard and CN wildcard (SNI mandatory)
                                  wildcard certificate could be problematic, see other hosts at
                                  https://search.censys.io/search?resource=hosts&virtual_hosts=INCLUDE&q=455943CF819425761D1F950263EBF54755D8D684C25535943976F488BC79D23B
     Chain of trust               Ok   
     EV cert (experimental)       no 
     Certificate Validity (UTC)   304 >= 60 days (2025-01-15 00:00 --> 2026-01-15 23:59)
     ETS/"eTLS", visibility info  not present
     Certificate Revocation List  http://crl3.digicert.com/DigiCertGlobalG3TLSECCSHA3842020CA1-2.crl
                                  http://crl4.digicert.com/DigiCertGlobalG3TLSECCSHA3842020CA1-2.crl
     OCSP URI                     http://ocsp.digicert.com
     OCSP stapling                offered, not revoked
     OCSP must staple extension   --
     DNS CAA RR (experimental)    not offered
     Certificate Transparency     yes (certificate extension)
     Certificates provided        2
     Issuer                       DigiCert Global G3 TLS ECC SHA384 2020 CA1 (DigiCert Inc from US)
     Intermediate cert validity   #1: ok > 40 days (2031-04-13 23:59). DigiCert Global G3 TLS ECC SHA384 2020 CA1 <-- DigiCert Global Root G3
     Intermediate Bad OCSP (exp.) Ok


     Testing HTTP header response @ "/" 

     HTTP Status Code             200 OK
     HTTP clock skew              0 sec from localtime
     Strict Transport Security    not offered
     Public Key Pinning           --
     Server banner                (no "Server" line in header, interesting!)
     Application banner           --
     Cookie(s)                    (none issued at "/")
     Security headers             Cache-Control: max-age=2845
     Reverse Proxy banner         --


     Testing vulnerabilities 

     Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
     CCS (CVE-2014-0224)                       not vulnerable (OK)
     Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
     ROBOT                                     Server does not support any cipher suites that use RSA key transport
     Secure Renegotiation (RFC 5746)           supported (OK)
     Secure Client-Initiated Renegotiation     not vulnerable (OK)
     CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
     BREACH (CVE-2013-3587)                    potentially NOT ok, "gzip" HTTP compression detected. - only supplied "/" tested
                                               Can be ignored for static pages or if no secrets in the page
     POODLE, SSL (CVE-2014-3566)               not vulnerable (OK), no SSLv3 support
     TLS_FALLBACK_SCSV (RFC 7507)              No fallback possible (OK), no protocol below TLS 1.2 offered
     SWEET32 (CVE-2016-2183, CVE-2016-6329)    not vulnerable (OK)
     FREAK (CVE-2015-0204)                     not vulnerable (OK)
     DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                               no RSA certificate, thus certificate can't be used with SSLv2 elsewhere
     LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
     BEAST (CVE-2011-3389)                     not vulnerable (OK), no SSL3 or TLS1
     LUCKY13 (CVE-2013-0169), experimental     not vulnerable (OK)
     Winshock (CVE-2014-6321), experimental    not vulnerable (OK)
     RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)


     Running client simulations (HTTP) via sockets 

     Browser                      Protocol  Cipher Suite Name (OpenSSL)       Forward Secrecy
    ------------------------------------------------------------------------------------------------
     Android 6.0                  TLSv1.2   ECDHE-ECDSA-AES128-GCM-SHA256     256 bit ECDH (P-256)
     Android 7.0 (native)         TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     Android 8.1 (native)         TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     Android 9.0 (native)         TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Android 10.0 (native)        TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Android 11 (native)          TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Android 12 (native)          TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Chrome 79 (Win 10)           TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Chrome 101 (Win 10)          TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Firefox 66 (Win 8.1/10)      TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Firefox 100 (Win 10)         TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     IE 6 XP                      No connection
     IE 8 Win 7                   No connection
     IE 8 XP                      TLSv1.0   DES-CBC3-SHA                      No FS
     IE 11 Win 7                  TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     IE 11 Win 8.1                TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     IE 11 Win Phone 8.1          TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     IE 11 Win 10                 TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     Edge 15 Win 10               TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     Edge 101 Win 10 21H2         TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Safari 12.1 (iOS 12.2)       TLSv1.3   TLS_CHACHA20_POLY1305_SHA256      253 bit ECDH (X25519)
     Safari 13.0 (macOS 10.14.6)  TLSv1.3   TLS_CHACHA20_POLY1305_SHA256      253 bit ECDH (X25519)
     Safari 15.4 (macOS 12.3.1)   TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Java 7u25                    No connection
     Java 8u161                   TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     Java 11.0.2 (OpenJDK)        TLSv1.3   TLS_AES_256_GCM_SHA384            256 bit ECDH (P-256)
     Java 17.0.3 (OpenJDK)        TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     go 1.17.8                    TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     LibreSSL 2.8.3 (Apple)       TLSv1.2   ECDHE-ECDSA-CHACHA20-POLY1305     256 bit ECDH (P-256)
     OpenSSL 1.0.2e               TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     OpenSSL 1.1.0l (Debian)      TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     OpenSSL 1.1.1d (Debian)      TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     OpenSSL 3.0.3 (git)          TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)
     Apple Mail (16.0)            TLSv1.2   ECDHE-ECDSA-AES256-GCM-SHA384     256 bit ECDH (P-256)
     Thunderbird (91.9)           TLSv1.3   TLS_AES_256_GCM_SHA384            253 bit ECDH (X25519)


     Rating (experimental) 

     Rating specs (not complete)  SSL Labs's 'SSL Server Rating Guide' (version 2009q from 2020-01-30)
     Specification documentation  https://github.com/ssllabs/research/wiki/SSL-Server-Rating-Guide
     Protocol Support (weighted)  100 (30)
     Key Exchange     (weighted)  100 (30)
     Cipher Strength  (weighted)  90 (36)
     Final Score                  96
     Overall Grade                A
     Grade cap reasons            Grade capped to A. HSTS is not offered

     Done 2025-03-18 00:27:53 [ 117s] -->> 23.33.90.74:443 (www.example.com) <<--
```
#### Qu’est-ce que le Handshake simulation ?
    - C'est un processus permettant de tester et d'analyser les mécanismes d'établissement de connexion sécurisée entre deux systèmes informatiques. 

## Bonus
### Installer un serveur Apache, puis tester sa configuration avec le script testssl.sh
```bash
    [it4@localhost ~]$ sudo systemctl start httpd
    [it4@localhost ~]$ sudo systemctl enable httpd
    Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service → /usr/lib/systemd/system/httpd.service.
    [it4@localhost ~]$ sudo firewall-cmd --permanent --add-service=http
    success
    [it4@localhost ~]$ sudo firewall-cmd --permanent --add-service=https
    success
    [it4@localhost ~]$ sudo firewall-cmd --reload
    success
    [it4@localhost ~]$ cd testssl.sh
    [it4@localhost testssl.sh]$ sudo dnf install mod_ssl -y
    [it4@localhost testssl.sh]$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/private/localhost.key -out /etc/pki/tls/certs/localhost.crt
    ....+........+.............+..+....+........+...+....+...+.....+++++++++++++++++++++++++++++++++++++++*.......+..............+.+...+..+.........+.+..+..........+..+.........+.+......+.....+....+.........+.....+.+++++++++++++++++++++++++++++++++++++++*.......+......+.........+...+............+...+........+.......+..+....+.........+..+...+.+..............+.+..+.......+..+...+......+.....................+.+..+......+......+.+............+.....+.+......+...........+....+......+.....+....+.....+.+..+...............+......+.+...+.....+.............+.....+.........+..........+...+..+...+......+....+...+.....+...+....+..................+.........+...+..+......+....+...........+..........++++++
    .+..+++++++++++++++++++++++++++++++++++++++*..+.......+...+++++++++++++++++++++++++++++++++++++++*.+..+...+.+...+.....+......+......+.+.....+.......+..+..........+........+.+.....+.+......+..+.+..+.........+.........+.+........+............+.+..+.......+..............+......+.+..+.+.........++++++
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [XX]:Fr
    State or Province Name (full name) []:Nouvelle-Aquitaine
    Locality Name (eg, city) [Default City]:Bordeaux
    Organization Name (eg, company) [Default Company Ltd]:EFREI
    Organizational Unit Name (eg, section) []:M1Cyber
    Common Name (eg, your name or your server's hostname) []:Toxic
    Email Address []:it4@testmail.com
    [it4@localhost testssl.sh]$ sudo systemctl restart httpd
    [it4@localhost testssl.sh]$ ./testssl.sh localhost

    #####################################################################
      testssl.sh version 3.2rc4 from https://testssl.sh/dev/
      (0c64e09 2025-03-17 10:26:57)

      This program is free software. Distribution and modification under
      GPLv2 permitted. USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

      Please file bugs @ https://testssl.sh/bugs/
    #####################################################################

      Using OpenSSL 1.0.2-bad (Sep 1 2022)  [~183 ciphers]
      on localhost:./bin/openssl.Linux.x86_64

     Start 2025-03-18 00:43:34        -->> 127.0.0.1:443 (localhost) <<--

     rDNS (127.0.0.1):       localhost.
     Service detected:       HTTP

     Testing protocols via sockets except NPN+ALPN 

     SSLv2      not offered (OK)
     SSLv3      not offered (OK)
     TLS 1      not offered
     TLS 1.1    not offered
     TLS 1.2    offered (OK)
     TLS 1.3    offered (OK): final
     NPN/SPDY   not offered
     ALPN/HTTP2 http/1.1 (offered)

     Testing cipher categories 

     NULL ciphers (no encryption)                      not offered (OK)
     Anonymous NULL Ciphers (no authentication)        not offered (OK)
     Export ciphers (w/o ADH+NULL)                     not offered (OK)
     LOW: 64 Bit + DES, RC[2,4], MD5 (w/o export)      not offered (OK)
     Triple DES Ciphers / IDEA                         not offered
     Obsoleted CBC ciphers (AES, ARIA etc.)            offered
     Strong encryption (AEAD ciphers) with no FS       offered (OK)
     Forward Secrecy strong encryption (AEAD ciphers)  offered (OK)
```
