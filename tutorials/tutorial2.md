---
layout: tutorial
title: Un simple jeu en JavaScript
styles: ["../css/cloud9.css"]
---

Dans ce TD nous allons créer une interface pour jouer à
[Puissance 4](http://fr.wikipedia.org/wiki/Puissance_4). JavaScript et
quelques composants basiques de CSS3 seront suffisants à créer une
interface agréable et parfaitement fonctionnelle.

Le seules références nécessaires pour ce TD sont

- Le [cours](../classes/class2),
- Mozilla Developer Network :
  - <https://developer.mozilla.org/docs/CSS>,
  - <https://developer.mozilla.org/docs/JavaScript>,
- W3Schools : <http://www.w3schools.com/>,
- Les JSFiddles du prof : <http://jsfiddle.net/user/defeo/>.

Le site de questions/réponses
[StackOverflow](http://stackoverflow.com/) est à utiliser avec
précaution. Toutes les autres références sont dangereuses, et risquent
de vous pénaliser si elles ne sont pas utilisées à bon escient.


## Préparer son espace de travail

Puisque ce TD constitue une partie du projet sur lequel vous êtes
évalués, vous allez créer un nouvel espace de travail dans
Cloud9. Suivez ces instructions.

1. Cliquez sur le bouton *« Create new workspace »*, puis *« Clone
   from URL »* ;
2. Dans la case *« Source URL »* écrivez
   <https://github.com/defeo/aws-project.git> ;
3. Laissez les autres paramètres tels quels (*« Open and
   Discoverable »* et *« Custom »*) ;
4. Cliquez sur create.

Votre espace de travail va s'appeler *« aws-project »* ; vous pouvez
maintenant y accéder. Vous y trouverez quelques fichiers préparés par
le professeur, auxquels il ne faudra pas toucher.

Pour prendre moins de risques, allez dans l'onglet *« Preferences »*
(icône <span class="c9-icon preferences"></span>) et, dans la section *« General »*
cochez la case *« Enable Auto-Save »*.

**Note :** Si vous aviez déjà commencé à travailler sur ce TD dans un
autre espace de travail, plutôt qu'en créer un nouveau vous pouvez
importer les contenus préparés par le professeur en tapant les
commandes suivantes dans le terminal (en bas dans l'espace de travail,
tapez `F6` s'il n'est pas déjà ouvert).

~~~
git clone --no-checkout https://github.com/defeo/aws-project.git
mv aws-project/.git .
rmdir aws-project
git reset --hard
~~~
{:.bash}


## Le plateau en pur CSS3

Pour simplicité, nous allons commencer avec un plateau réduit, de
taille 2×3. 

1. Créez un document HTML5 valide contenant un `<div>` contenant
   tableau (`<table>`) de 3 lignes sur 2 colonnes, avec toutes les
   cases vides. Donnez un identifiant unique au `<div>`.

2. Liez une feuille de style à votre document HTML. Donnez une largeur
   (`width`) et une hauteur (`height`) fixes aux cases du tableau. En
   utilisant la propriété `border`, donnez des bordures bleues aux
   cases du tableau.
   
   Maintenant vous avez une grille presque jolie, mais carrée. À
   l'aide de la propriété `border-radius` faites en sorte que les
   cases deviennent des ronds, comme cela
   
   <div style="margin:auto;width:2em;height:2em;border-radius:2em;border:solid 0.2em blue"></div>
   
   Il reste une dernière touche de classe à donner : remplir les
   espaces blancs entre les cases. Pour cela, servez-vous
   judicieusement de `background-color` sur les différents éléments du
   tableau, pour obtenir un résultat comme cela
   
   <div style="background-color:blue;width:2.4em;margin:auto">
   <div style="background-color:white;width:2em;height:2em;border-radius:2em;border:solid 0.2em blue">
   </div></div>

3. Il est temps de jouer. Ajoutez des classes `joueur1` et `joueur2`
   sur les cases du tableau. Les cases seront remplies avec un jeton
   rouge ou jaune selon si elles appartiennent à l'une ou l'autre
   classe (utiliser à nouveau `background-color`).


## Unobtrusive JavaScript

On est maintenant prêts à écrire la vraie application interactive. On
pourrait faire un tableau 6×7 directement dans le code HTML, comme
dans la partie précédente, mais nous sommes trop paresseux pour taper
tous ces `<td></td>` à la main. À la place, nous allons générer
dynamiquement les éléments du tableau avec JavaScript. Ceci aura
plusieurs avantages, la possibilité de modifier facilement la taille
du tableau n'étant pas le dernier.

En faisant cela nous allons nous conformer à un principe très
populaire dans le développement web moderne : celui du
[*Unobtrusive JavaScript*](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
(JavaScript discret). Ce paradigme dit que la page HTML doit pouvoir
marcher même en l'absence de JavaScript, les éléments dynamiques étant
ajoutés seulement une fois que le JavaScript est chargé.

En l'occurrence, vu que notre page ne contient qu'un jeu interactif,
il faudra se limiter à afficher un message d'erreur lorsque JavaScript
n'est pas chargé.

1. Effacez le `<table>` du document HTML. Remplacez le `<div>` par un
   simple
   
	   <div id="...">Activez JavaScript pour jouer.</div>
   
   (mettez l'identifiant que vous avez choisi auparavant à la place
   des `...`).
   
2. Liez un fichier JavaScript à la page HTML. Pour éviter les
   problèmes de chargement du DOM, mettez la balise `<script>` après
   le `<div>`, vers la fin du `<body>`. Faites un affichage de
   contrôle avec `console.log` et testez avec la console que votre
   script est bien chargé.

3. Dans le JavaScript récupérez l'élément correspondant au `<div>`
   grâce à `document.querySelector`. En vous servant de
   
   - `document.createElement` pour créer des nouveaux nœuds,
   - la propriété `.innerHTML` et/ou la méthode `.appendChild` pour ajouter des nœuds au DOM,
   - deux boucles `for` imbriquées,
   
   remplacez le texte du div par un tableau 6×7. Pour une majeure
   flexibilité, vous pouvez faire en sorte que 6 et 7 soient des
   constantes stockées dans des variables JavaScript.

4. Il serait pénible d'utiliser l'API du DOM pour parcourir le plateau
   à chaque fois que l'on veut modifier la partie ou en vérifier une
   propriété. Il est plus pratique d'avoir un tableau JavaScript à
   deux dimensions qui pointe vers les cases (les `<td>`) du
   plateau. Modifiez la double boucle `for` de sorte à remplir un
   tableau à deux dimensions avec des pointeurs vers les
   `HTMLTableCellElement` correspondants aux cases du plateau.

5. Écrivez une fonction `set(row, column, player)` qui prend en entrée
   des numéros de ligne et de colonne et un code représentant le
   joueur 1 ou 2, et qui place un jeton de la couleur correspondante
   dans la case désignée. On vous rappelle que pour attribuer un
   élément du DOM à la classe `joueur1` il suffit d'utiliser
   l'attribut `.className`. Par exemple
   
	   var elt = document.querySelector('#mon-element');
	   elt.className = 'joueur1';
   
   Testez votre fonction à travers la console.

6. Ajoutez une variable `turn` qui indique à qui est le tour de jouer,
   initialisée au joueur 1 (adoptez la même convention que vous avez
   utilisée pour le paramètre `player`).

7. Écrivez une fonction `play(column)` qui

   - prend en entrée un numéro de colonne,
   - cherche la première ligne libre en partant du bas (s'il y en a),
   - y joue un pion de la couleur indiquée par la variable `turn`,
   - passe le tour en changeant la valeur de `turn`.
   
   Testez votre fonction à travers la console.


## Gestion des évènements

Il est maintenant temps de répondre aux clics. On veut que, lorsque
l'utilisateur clique dans une colonne (peu importe la ligne), la
fonction `play` soit appelée sur cette colonne.

On pourrait définir un gestionnaire d'évènements par `<td>`, mais ce
ne serait ni élégant, ni efficace : 42 gestionnaires d'évènements,
c'est très lourd, surtout pour un téléphone portable... Nous allons
plutôt définir un unique gestionnaire pour tout le plateau, et
utiliser l'objet de type `Event` pour savoir quelle case a généré
l'évènement.

Mais comment faire pour savoir à quelle colonne appartient un `<td>`
donné ? Parmi les nombreuses solutions à notre disposition (on
pourrait, par exemple, naviguer le DOM pour compter combien `<td>`
sœurs existent à gauche), l'API *data attributes* de HTML5 nous offre
la plus élégante.

1. Par la méthode `.addEventListener()`, définissez un gestionnaire
   pour l'évènement `click` sur le plateau (sur la balise `<table>`,
   par exemple). Pour l'instant, contentez vous de faire un
   
	   console.log(event.target)
   
   dans le gestionnaire (`event` étant l'objet évènement passé au
   gestionnaire). Testez votre gestionnaire à l'aide de la console.

2. Modifiez votre double boucle `for` en sorte que les balises `<td>`
   contiennent un attribut `data-column` égal à la colonne dans
   laquelle la balise se trouve. Vous avez deux façons de faire cela,
   selon la façon dont vous avez précédemment codé votre boucle :
   directement avec `.innerHTML` ou bien avec le champs `dataset`.
   
   ~~~
   // exemple de méthode utilisant innerHTML
   ma_ligne.innerHTML += '<td data-column="0"></td>';
   ~~~
   {:.javascript}
   
   ~~~
   // exemple utilisant dataset
   ma_ligne.appendChild(ma_case);
   ma_case.dataset['column'] = 0;
   ~~~
   {:.javascript}
   
   Modifiez le gestionnaire pour qu'il affiche dans la console la
   valeur de l'attribut `data-column`. Testez avec la console.

3. Modifiez votre gestionnaire pour qu'il réponde aux clics en jouant
   dans la colonne cliquée, ou bien en ne faisant rien lorsqu'on
   clique ailleurs (vous pouvez utiliser l'existence de la donnée
   `column` comme test pour savoir si on a bien cliqué sur une case).
   

## Touches finales (optionnel)

Vous pouvez aborder les points suivants dans n'importe quel
ordre. Suivez vos goûts ! (Pour le CC, il vous sera tout de même
demandé d'avoir développé les points 1 et 2).

1. Écrivez une fonction qui teste si un coup est gagnant (4 pions de
   la même couleur alignés verticalement, horizontalement ou
   diagonalement). Remarquez qu'il suffit de tester quatre directions
   autour de la dernière case jouée, et de s'éloigner d'au plus 3
   cases en chaque direction.

2. Modifiez votre application pur qu'à chaque nouveau coup elle teste
   s'il s'agit d'un coup gagnant, ou à défaut si le plateau a été
   complètement rempli (il s'agit d'une égalité dans ce cas). Lorsque
   la partie est terminée, l'interface affiche le gagnant. Cliquer à
   nouveau sur le plateau démarre alors une nouvelle partie.

3. En utilisant la propriété CSS `animation` (attention
   `-webkit-animation` pour Chrome), faites *flasher* l'alignement de
   4 pions gagnant. Vous pouvez consulter ce
   [guide aux animations CSS.](https://developer.mozilla.org/fr/docs/CSS/Animations_CSS)
   (aussi
   [en anglais](https://developer.mozilla.org/docs/CSS/CSS_Animations)).

4. Ajoutez des touches de style grâce à la propriété `box-shadow`.

4. Encapsulez votre code dans un objet JavaScript, de sorte à pouvoir
   avoir plusieurs plateaux dans la même page.

5. Testez votre application avec un téléphone portable et adaptez-la
   aux petits écrans. Si vous n'avez de smartphone, vous pouvez
   utiliser l'émulation de Chrome ou Firefox:
   
   - Dans Chrome, ouvrez les *dev tools*, puis tapez `ECHAP` ou
	 cliquez sur l'icône *ouvrir console*. Sélectionnez l'onglet
	 *Émulation/Emulation* (il peut être nécessaire d'activer
	 l'émulation dans les options des *dev tools*). Une liste de
	 téléphone portables à émuler vous est proposée.
   - Dans Firefox tapez `Shfit+Ctrl+M`, puis sélectionnez la
     résolution voulue.
   
   Il vous sera utile de consulter ce
   [guide au développement sur mobile](https://developer.mozilla.org/docs/Web/Guide/Mobile),
   et en particulier la partie sur
   [l'utilisation de la balise `viewport`](https://developer.mozilla.org/fr/docs/Mozilla/Mobile/Balise_meta_viewport)
   (en
   [anaglais](https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag))
