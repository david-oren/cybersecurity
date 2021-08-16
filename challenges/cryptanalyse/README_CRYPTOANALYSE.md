# Challenger cryptanalyse

## Encodage - ASCII

La string est sous le format HEX.

```shell
Le flag de ce challenge est: 2ac376481ae546cd689d5b91275d324e
```

## Encodage - UU

Le format est dans le type: Uuencoding.

```shell
Very simple ;)
PASS = ULTRASIMPLE
```

## Hash - Message Digest 5

Le format est aussi dans le titre. Ainsi que le fait que c'est un algorithme dont nous avons déjà trouvé des collisions.

```shell
weak
```

## Hash - SHA-2

En connaissant l'algorithme SHA-2, nous savons qu'un hash de ce type est toujours de 64 caractères ainsi nous pouvons déduire qu'il y a un caractère en trop, un caractère intrigant est le "k" car il n'est pas possible d'avoir cette lettre dans un hash SHA-2.
Ainsi l'hash réel est:

```shell
96719db60d8e3f498c98d94155e1296aac105c4923290c89eeeb3ba26d3eef92
```

Et le SHA-1 est donc:

```shell
a7c9d5a37201c08c5b7b156173bea5ec2063edf9
```

## Chiffrement par décalage

Afin de trouver le bon flag on a essayé de faire un décalage simple d'ASCII ainsi nous avons réussi à obtenir une phrase cohérence à partir d'un décalage de 10.

```shell
Bra5o! Tu peu5 5alider a5ec le pass Yolaihu
```