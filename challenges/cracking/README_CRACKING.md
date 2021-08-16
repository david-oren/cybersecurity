# Challenge cracking

## 1. ELF - 0 Protection

Nous avons utilisé la commande UNIX [`strings`](https://linux.die.net/man/1/strings) permettant de lire les strings d'un binaire. A partir de ça, nous avons repéré le mot de passe dans le après l'appel de la méthode `_-libc_start_main`:

```shell=bash
__libc_start_main
GLIBC_2.0
PTRh@
[^_]
%s : "%s"
Allocating memory
Reallocating memory
123456789
```

Le mot de passe est donc **123456789**.

## 2. ELF - x86 Basique

Nous avons utilisé le debbuger `gdb` pour récupérer le user et le mot de passe du programme. A l'aide de la commande `disassemble main`, nous récupérons le contenu de la fonction main en assembleur et nous avons remarqué la déclaration de variables DWORD PTR aux valeurs **0x80a6b19** et **0x80a6b1e**

```shell=bash
Dump of assembler code for function main:
   0x08048309 <+0>:	lea    ecx,[esp+0x4]
   0x0804830d <+4>:	and    esp,0xfffffff0
   0x08048310 <+7>:	push   DWORD PTR [ecx-0x4]
   0x08048313 <+10>:	push   ebp
   0x08048314 <+11>:	mov    ebp,esp
   0x08048316 <+13>:	push   ecx
   0x08048317 <+14>:	sub    esp,0x24
   0x0804831a <+17>:	mov    DWORD PTR [ebp-0xc],0x80a6b19
   0x08048321 <+24>:	mov    DWORD PTR [ebp-0x10],0x80a6b1e
```

Ensuite, grâce à la commande `x/s`, on peut récupérer la string correspondante, soit un username `john` et un password `the ripper`.

Le mot de passe trouvé avec les identifiants est donc **987654321**.

## 3. PE - 0 Protection