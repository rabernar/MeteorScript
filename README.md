# MeteorScript — Guide simple

MeteorScript, c'est un petit langage de script pour créer des jeux Wired plus poussés. Concrètement, ça sert à retenir des valeurs, faire des calculs, tester des conditions, bouger des joueurs ou des mobis, et gérer des états — bref, tout ce que les Wired classiques ne savent pas bien faire tout seuls.

## Sommaire

- [Quand l'utiliser](#quand-lutiliser)
- [Comment le placer dans une room](#comment-le-placer-dans-une-room)
- [À savoir avant de commencer](#à-savoir-avant-de-commencer)
- [Les variables](#les-variables)
- [Les variables auto (fournies par le serveur)](#les-variables-auto-fournies-par-le-serveur)
- [Les commandes (Actions)](#les-commandes-actions)
- [Les conditions](#les-conditions)
- [Les fonctions utiles](#les-fonctions-utiles)
- [Si ça renvoie une erreur](#si-ça-renvoie-une-erreur)
- [Exemples concrets](#exemples-concrets)
- [Conseils pour ne pas se planter](#conseils-pour-ne-pas-se-planter)
- [Ce que MeteorScript ne fait pas (encore)](#ce-que-meteorscript-ne-fait-pas-encore)
- [Aide-mémoire rapide](#aide-mémoire-rapide)

## Quand l'utiliser

Garde les Wired classiques pour tout ce qui est simple : déclencher un évènement, organiser une pile, bouger un mobi visuellement. Passe à MeteorScript dès que t'as besoin de :

- compter un score (global ou par joueur)
- calculer un score à partir de plusieurs valeurs
- vérifier où se trouve un joueur
- sauvegarder une étape/checkpoint pour un joueur
- gérer un mini-jeu à plusieurs étapes
- tirer un truc au hasard
- remplacer un empilement énorme de SuperWired qui fait la même chose en boucle

## Comment le placer dans une room

Utilise ces deux mobis :

| Mobi | À quoi il sert |
| --- | --- |
| **Wired Action MeteorScript** | Lance une commande |
| **Wired Condition MeteorScript** | Vérifie une condition |

Il existe aussi une ancienne méthode (`meteorscript:` dans un SuperWired) gardée pour ne pas casser les vieux jeux, mais ne t'en sers pas pour du neuf : les deux mobis dédiés sont plus clairs à relire et à corriger.

## À savoir avant de commencer

- Une Action = une seule commande. Une Condition = une seule vérification.
- Respecte les majuscules dans les noms de commandes (`toUpper`, pas `toupper`).
- Les mots dans une commande sont séparés par des espaces, pas de guillemets spéciaux.
- Certaines commandes prennent tout ce qui suit comme texte, espaces compris (ex : `set var1 Salut tout le monde`).
- **Les variables s'effacent** quand la room ou le joueur se décharge — pour l'instant rien n'est permanent (ça viendra avec MeteorScript DB, prévu plus tard).
- Les positions dans une room, c'est `X Y` (parfois `Z` pour la hauteur).

## Les variables

Deux types, selon qui les utilise :

| Type | Qui la voit | Exemple | Pour quoi faire |
| --- | --- | --- | --- |
| `var1` → `var64` | Toute la room | `var1` | score commun, avancée du jeu, état d'une porte |
| `usrvr1` → `usrvr32` | Un seul joueur | `usrvr1` | score perso, checkpoint, choix du joueur |

Si deux joueurs touchent à la même `varX`, ils modifient la même valeur — attention aux conflits. À l'inverse, une `usrvrX` ne marche que s'il y a bien un joueur qui déclenche le Wired.

Pas de `%` pour écrire une variable :

```text
set var1 10
set usrvr1 %username%
set var2 calc(var1 + 5)
```

Si tu réutilises une variable dans une commande, elle est remplacée par sa valeur — pas par son nom. Donc si `var2` contient `rouge`, écrire `set var1 var2` va mettre `rouge` dans `var1`.

💡 Pense à donner une valeur de départ à tes variables numériques avant de faire un calcul dessus, sinon `calc` ne saura pas quoi en faire.

## Les variables auto (fournies par le serveur)

Contrairement aux tiennes, celles-ci s'écrivent **avec** `%` et sont déjà remplies par le jeu.

**Sur la room** : `%roomId%`, `%roomName%`, `%roomOwner%`, `%roomPvpState%`, `%roomBankDiams%`, `%timestamp%`, `%maxInt%`, etc.

**Sur le joueur qui déclenche** : `%username%`, `%userId%`, `%userPosX%` / `%userPosY%` / `%userPosZ%`, `%userFigure%`, `%userDiams%`, `%hasRights%`, `%userLvl%`, `%points%`, etc. (elles n'existent que s'il y a bien un joueur dans le contexte du Wired)

**`%data%`** : contient ce que le trigger envoie — un id de mobi, un pseudo, un texte... ça dépend totalement du Wired qui l'a déclenché. Fais un test rapide si t'es pas sûr de ce qu'il contient.

## Les commandes (Actions)

*(à mettre dans un Wired Action MeteorScript)*

**Modifier une variable**

| Commande | Ce que ça fait | Exemple |
| --- | --- | --- |
| `set varX valeur` | Change la valeur | `set var1 25` |
| `replace varX ancien nouveau` | Remplace un bout de texte | `replace var1 rouge bleu` |
| `erase varX texte` | Enlève un bout de texte | `erase var1 _old` |
| `concat` / `append varX valeur` | Ajoute du texte à la fin | `concat var1 _fin` |
| `trim varX` | Enlève les espaces autour | `trim var1` |
| `toUpper` / `toLower varX` | Change la casse | `toUpper var1` |

(Ça marche pareil avec `usrvrX` à la place de `varX`.)

**Bouger ou modifier un mobi précis**

| Commande | Ce que ça fait | Exemple |
| --- | --- | --- |
| `movetobyid furniId X Y` | Déplace ce mobi à cette position | `movetobyid 123456 5 8` |
| `setstatebyid furniId state` | Change son état | `setstatebyid 123456 1` |
| `addstatebyid furniId value` | Ajoute une valeur à son état actuel | `addstatebyid 123456 1` |

**Déplacements généraux**

| Commande | Ce que ça fait |
| --- | --- |
| `moveto X Y` | Déplace ce que le Wired a sous la main : les mobis sélectionnés, ou sinon le joueur qui a déclenché |
| `warpto X Y [Z]` | Téléporte le joueur qui a déclenché |

Pour éviter les surprises : `movetobyid` si tu veux viser un mobi précis, `warpto` si tu veux bouger un joueur.

**Sur le joueur**

- `forcesit` / `forcelay` — force à s'asseoir / s'allonger
- `teleport roomId` — demande à entrer dans une autre room (passage direct si même proprio, sinon demande classique)

**Réglages de jeu custom** : `setPvpAuto`, `setMaxZombies` (1-50), `setMinZombies` (1-25), `setSpawnFreq` (1-30), `setMaxBoxes` (1-100).

**Pour tester** : `echo texte` t'envoie un message privé de debug. Pratique en test, à retirer avant de publier le jeu (ça peut spammer).

## Les conditions

*(à mettre dans un Wired Condition MeteorScript)*

**Comparer deux valeurs**

```text
var1 == 10     # égal
var1 != fini   # différent
var1 >= 1      # plus grand ou égal
```

**Vérifier si une case est occupée**

```text
playerAt(5,8)      # vrai si quelqu'un est dessus
noPlayerAt(10,12)  # vrai si personne
```

**Vérifier le type d'une valeur**

```text
isInt(var1)
isBool(%hasRights%)
!isNull(var2)   # le ! inverse le test
```

**Chercher un texte dans un autre**

```text
indexOf(%username%;Admin) != -1
```

(Évite l'ancien `(in)`, il est peu fiable — `indexOf` est plus sûr.)

## Les fonctions utiles

Elles marchent dans les commandes ET les conditions.

**Pour les chiffres**

```text
rand(1,6)        -> tire un nombre entre 1 et 6
calc(2 + 3 x 4)   -> 14 (résultat arrondi)
calc2(5 / 2)      -> 2.5 (garde les décimales)
```

**Pour le texte** (exemple avec `%username%` = `Meteor`)

```text
length(%username%)     -> 6
charAt(%username%;0)   -> M
substr(%username%;0;3) -> Met
```

**Pour les mobis**

```text
getFurniState(123456)
getFurniCoords(123456)
```

Si l'id du mobi n'existe pas, ça renvoie un message d'erreur plutôt que de planter.

## Si ça renvoie une erreur

| Tu vois... | Ça veut dire... |
| --- | --- |
| `SyntaxError` | La commande est mal écrite |
| `NumberFormatException` | Un nombre était attendu, autre chose est arrivé |
| `IndexOutOfBoundsException` | Tu cherches un caractère qui n'existe pas dans le texte |
| `InvalidFurniIdException` | Ce mobi n'existe pas dans la room |
| `InvalidPositionException` | Le mobi n'a pas de position valide |
| `null` | La valeur est vide |

Teste toujours tes ids de mobis avant de t'y fier — une erreur est traitée comme du texte, pas comme un vrai état.

## Exemples concrets

**Un compteur qui augmente**

```text
set var1 0                  # au démarrage
set var1 calc(var1 + 1)     # à chaque déclenchement
var1 >= 10                  # condition pour la suite
```

**Retenir une étape par joueur**

```text
set usrvr1 checkpoint_2
```
```text
usrvr1 == checkpoint_2
```

**Ouvrir un passage si personne n'est dessus**

```text
noPlayerAt(8,10)            # condition
movetobyid 123456 8 10      # action
```

**Tirer une récompense au hasard**

```text
set var1 rand(1,3)
```
Puis fais trois conditions séparées : `var1 == 1`, `var1 == 2`, `var1 == 3`.

## Conseils pour ne pas se planter

- Sépare bien calcul et vérification plutôt que tout mettre au même endroit — plus simple à relire plus tard.
- Organise tes variables par bloc, par exemple :

  | Variables | Rôle |
  | --- | --- |
  | `var1` à `var10` | État du jeu |
  | `var11` à `var20` | Scores |
  | `usrvr1` à `usrvr5` | Progression du joueur |

- Vérifie avant d'agir : `%hasRights% == true`, `noPlayerAt(5,5)`...
- Évite de tout faire à chaque tick ou de spam `echo` en public.
- Passe par les Wired dédiés plutôt que l'ancien préfixe `meteorscript:`.

## Ce que MeteorScript ne fait pas (encore)

- Pas de sauvegarde permanente — tout s'efface au rechargement (à venir avec MeteorScript DB).
- Sensible aux majuscules.
- Pas de vrais guillemets pour les arguments.
- `moveto` dépend du contexte — préfère `movetobyid`/`warpto` si tu veux être sûr de viser la bonne cible.

## Aide-mémoire rapide

```text
set var1 10
set var1 calc(var1 + 1)
var1 >= 10
noPlayerAt(5,8)
movetobyid 123456 5 8
warpto 10 12
rand(1,6)
calc(2 + 3 x 4)
indexOf(%username%;a)
```

---

Le réflexe à garder : teste petit, vérifie que ça marche, puis assemble. C'est plus simple à corriger quand chaque Wired a un rôle bien précis.
