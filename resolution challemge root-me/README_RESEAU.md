# Challenge Réseau

## 1. FTP - Authentification

En utilisant un décodeur de packet PCAP, on peut voir les lignes suivantes avec l'input USER et PASS en clair :
```
#8 4.216600 10.20.144.150 → 10.20.144.151 FTP 81 Request: USER cdts3500
#9 4.217350 10.20.144.151 → 10.20.144.150 FTP 91 Response: 331 Enter password.
#10 4.217630 10.20.144.150 → 10.20.144.151 TCP 66 35974 → 21 [PSH, ACK] Seq=16 Ack=114 Win=32648 Len=0 TSval=1657564500 TSecr=1657394000
#11 7.639420 10.20.144.150 → 10.20.144.151 FTP 81 Request: PASS cdts3500
```
Le mot de passe est donc **cdts3500**.

## 2. TELNET - authentification

Toujours en utilisant un décodeur de packet PCAP, on observe les lignes suivantes :
```
56	9.464208	192.168.0.1	192.168.0.2	TELNET	75	Telnet Data ...
Data: Password:
```
Puis en suivant les packets suivants, on le mot de pass parmis les packets suivants :
```
58	10.704378	192.168.0.2	192.168.0.1	TELNET	67	Telnet Data ...
Data: u
60	11.144054	192.168.0.2	192.168.0.1	TELNET	67	Telnet Data ...
Data: s
62	11.625626	192.168.0.2	192.168.0.1	TELNET	67	Telnet Data ...
Data: e
64	11.931320	192.168.0.2	192.168.0.1	TELNET	67	Telnet Data ...
Data: r
66	13.285963	192.168.0.2	192.168.0.1	TELNET	68	Telnet Data ...
Data: \r
```
Le mot de passe est donc **user**.

## 3. ETHERNET - trame

A l'aide d'un décodeur hexadécimal, on converti la trame en données lisibles et on trouve les données suivantes :
```
Hypertext Transfer Protocol
    GET / HTTP/1.1\r\n
    Authorization: Basic Y29uZmk6ZGVudGlhbA==\r\n
    Credentials: confi:dential
```
Le mot de passe est donc **confi:dential**.

## 4. Authentification twitter

A l'aide du décodeur de packet PCAP, on découvre dans le packet PCAP la requête faîte à Twitter :
```
Hypertext Transfer Protocol
    GET /statuses/replies.xml HTTP/1.1\r\n
    User-Agent: CFNetwork/330\r\n
    Cookie: _twitter_sess=BAh7CDoJdXNlcjA6B2lkIiVmZGQ2ODc5MTMwMWFhOTFiMWExZDViZmQwMGEz%250AOWNkMyIKZmxhc2hJQzonQWN0aW9uQ29udHJvbGxlcjo6Rmxhc2g6OkZsYXNo%250ASGFzaHsABjoKQHVzZWR7AA%253D%253D--ea12e7bc090d05202cd7e3f972c2b4414a97f657\r\n
        Cookie pair: _twitter_sess=BAh7CDoJdXNlcjA6B2lkIiVmZGQ2ODc5MTMwMWFhOTFiMWExZDViZmQwMGEz%250AOWNkMyIKZmxhc2hJQzonQWN0aW9uQ29udHJvbGxlcjo6Rmxhc2g6OkZsYXNo%250ASGFzaHsABjoKQHVzZWR7AA%253D%253D--ea12e7bc090d05202cd7e3f972c2b4414a97f657
    Accept: */*\r\n
    Accept-Language: en-us\r\n
    Accept-Encoding: gzip, deflate\r\n
    Authorization: Basic dXNlcnRlc3Q6cGFzc3dvcmQ=\r\n
        Credentials: usertest:password
    Connection: keep-alive\r\n
    Host: twitter.com\r\n
    \r\n
    [Full request URI: http://twitter.com/statuses/replies.xml]
    [HTTP request 1/1]
```
Le mot de passe est donc **password**.

## 5. CISCO - Mot de passe

On trouve dans le fichier du challenge les ligne suivante :
```
username hub password 7 025017705B3907344E 
username admin privilege 15 password 7 10181A325528130F010D24
username guest password 7 124F163C42340B112F3830
```
En déchiffrant les mots de passe avec le "weak reversible algorithm", on obtient les mots de passe suivant pour hub, admin et guest :
```
6sK0_hub
6sK0_admin
6sK0_guest
```
On a donc déduit que le mot de passe enable était **6sK0_enable**.

## 6. IP - Time To Live

En analysant le packet récupéré, on peut voir que le TTL est incrémenté à chaque requête jusqu'à avoir une réponse au TTL 13 :
```
69	44.973782	24.6.126.218	198.173.244.32	ICMP	106	Echo (ping) request  id=0x0200, seq=9728/38, ttl=12 (no response found!)
71	49.252888	24.6.126.218	198.173.244.32	ICMP	106	Echo (ping) request  id=0x0200, seq=9984/39, ttl=13 (reply in 72) 
```
Le TTL est donc **13**.

## 7. LDAP - null bind

Le titre et l’énoncé nous apporte les informations suivantes :

LDAP : Nous avons affaire à un annuaire LDAP (challenge01.root-me.org:54013)
null bind : Ce dernier proposera surement une authentification anonyme (option -x de ldapsearch)
"anonymous se soit installé" : le coupable est identifié... retenons le pour tout à l’heure
"dc=challenge01,dc=root-me,dc=org" : sera notre point de départ pour les recherches
Pour chercher au sein d’une base LDAP on va se munir de ldapsearch, et confirmer notre point de départ en listant les contextes accessibles :

$ ldapsearch -x -h challenge01.root-me.org -p 54013 "(ObjectClass=*)" "namingContexts" -LLL
No such object (32)
Déception, pas d’informations supplémentaires par ici, l’ensemble a l’air bien foutu.... à moins que ?

On tente alors de faire une recherche à partir du point de base de l’énoncé, afin de retourner tous les enregistrements :

$ ldapsearch -x -h challenge01.root-me.org -p 54013 -b "dc=challenge01,dc=root-me,dc=org" -LLL
Insufficient access (50)
Il semble que les droits d’accès sur la base sont restreints et il sera alors compliqué de connaitre l’arborescence... mais maintenant souvenons nous qu’anonymous s’est installé, les droits d’accès n’ont pas dû être paramétrés comme il faut... Tentons alors l’unité d’organisation (OU) "anonymous" par hasard... :

$ ldapsearch -x -h challenge01.root-me.org -p 
54013 -b "ou=anonymous,dc=challenge01,dc=root-me,dc=org" -LLL
dn: ou=anonymous,dc=challenge01,dc=root-me,dc=org
objectClass: organizationalUnit
ou: anonymous

dn: uid=sabu,ou=anonymous,dc=challenge01,dc=root-me,dc=org
objectClass: inetOrgPerson
objectClass: shadowAccount
uid: sabu
sn: sabu
cn: sabu
givenName: sabu
mail: sabu@anonops.org
Gagné, le flag est sabu@anonops.org
## 8. SIP - Authentification

En regardant le fichier de log donné, on remarque la première ligne qui contient un `REGISTER` avec en fin de ligne **1234** :
```
172.25.105.3"172.25.105.40"555"asterisk"REGISTER"sip:172.25.105.40"4787f7ce""""PLAIN"1234
172.25.105.3"172.25.105.40"555"asterisk"INVITE"sip:1000@172.25.105.40"70fbfdae""""MD5"aa533f6efa2b2abac675c1ee6cbde327
172.25.105.3"172.25.105.40"555"asterisk"BYE"sip:1000@172.25.105.40"70fbfdae""""MD5"0b306e9db1f819dd824acf3227b60e07
```
Par réflexe, on essaie et ça fonctionne.

