# MeteorScript

Mini-langage de scripting pour les créateurs de jeux Wired. Il permet de stocker des valeurs temporaires, faire des calculs, vérifier des conditions, déplacer des joueurs ou des mobis, manipuler des états, et construire des logiques plus souples qu'avec les Wired classiques seuls.

## Sommaire

- [Pourquoi MeteorScript](#pourquoi-meteorscript)
- [Où l'utiliser](#où-lutiliser)
- [Règles simples](#règles-simples)
- [Variables](#variables)
- [Variables automatiques](#variables-automatiques)
- [Commandes d'action](#commandes-daction)
- [Conditions](#conditions)
- [Fonctions](#fonctions)
- [Erreurs possibles](#erreurs-possibles)
- [Exemples pratiques](#exemples-pratiques)
- [Bonnes pratiques](#bonnes-pratiques)
- [Limites connues](#limites-connues)
- [Aide-mémoire](#aide-mémoire)

## Pourquoi MeteorScript

Les Wired classiques restent parfaits pour déclencher des évènements, organiser une pile, déplacer des mobis visuellement ou utiliser des comportements déjà prêts. MeteorScript devient utile dès qu'une logique demande des **variables**, des **calculs**, des **comparaisons** ou **plusieurs branches**.

Il est déjà préférable aux Wired classiques pour :

- un compteur global ou par joueur
- un score calculé à partir de plusieurs valeurs
- une condition basée sur la position du joueur
- un checkpoint personnel
- une logique de mini-jeu avec plusieurs étapes
- une sélection aléatoire simple
- un système qui demanderait beaucoup de SuperWired copiés-collés

À terme, MeteorScript doit remplacer les systèmes Wired complexes ou difficiles à maintenir par du code simple, lisible et plus performant.

## Où l'utiliser

| Mobi | Sert à quoi |
| --- | --- |
| **Wired Action MeteorScript** | Exécute une commande MeteorScript |
| **Wired Condition MeteorScript** | Vérifie une condition MeteorScript |

L'ancien format `meteorscript:` dans certains SuperWired existe encore pour compatibilité, mais il est **déconseillé** pour les nouvelles créations : les Wired dédiés sont plus lisibles et plus simples à debug.

## Règles simples

- Une **Action** exécute une seule commande, une **Condition** vérifie une seule condition.
- Les commandes sont sensibles à la casse : `toUpper` ✅, `toupper` ❌.
- Les arguments sont séparés par des espaces, sans guillemets magiques (`"texte"` reste du texte avec des guillemets).
- Certaines commandes prennent tout le reste de la ligne comme valeur (ex : `set var1 Bonjour tout le monde`).
- Les variables sont **temporaires** : elles vivent tant que la room ou le joueur est chargé (la persistance arrivera avec **MeteorScript DB**).
- Coordonnées de room : `X Y`, parfois `Z`. Ids de mobis : les vrais ids posés dans la room.

## Variables

| Variable | Portée | Exemple | Usage typique |
| --- | --- | --- | --- |
| `var1` à `var64` | Room (partagée entre tous les joueurs) | `var1` | Score global, étape du jeu, état d'une porte |
| `usrvr1` à `usrvr32` | Joueur déclencheur | `usrvr1` | Score perso, checkpoint perso, choix du joueur |

⚠️ Sans joueur dans le contexte du Wired, une commande sur `usrvrX` ne fait rien.

Les variables s'écrivent **sans `%`** :

```text
set var1 10
set usrvr1 %username%
set var2 calc(var1 + 5)
```

Utiliser une variable dans une valeur la remplace par son contenu : si `var2` vaut `rouge`, `set var1 var2` met `rouge` dans `var1`, pas le texte `var2`.

> 💡 Initialisez vos variables numériques avant de les utiliser dans `calc` — une variable vide n'est pas un nombre.

## Variables automatiques

Fournies par le serveur, elles s'écrivent **avec `%`**.

**Room** (extrait) : `%roomId%`, `%roomName%`, `%roomOwner%`, `%roomPvpState%`, `%roomBankDiams%`, `%timestamp%`, `%maxInt%`...

**Joueur/entité déclencheuse** (extrait) : `%username%`, `%userId%`, `%userPosX/Y/Z%`, `%userFigure%`, `%userDiams%`, `%hasRights%`, `%userLvl%`, `%points%`...

**Déclencheur** : `%data%` — dépend du trigger (id de mobi, pseudo, texte, ou `undefined`). Testez avec un montage simple si vous ne connaissez pas sa valeur.

## Commandes d'action

*(dans un Wired Action MeteorScript)*

**Variables**

| Commande | Effet | Exemple |
| --- | --- | --- |
| `set varX valeur` | Remplace une variable de room | `set var1 25` |
| `set usrvrX valeur` | Remplace une variable joueur | `set usrvr1 checkpoint_2` |
| `replace varX ancien nouveau` | Remplace un morceau de texte | `replace var1 rouge bleu` |
| `erase varX texte` | Supprime un morceau de texte | `erase var1 _old` |
| `concat` / `append varX valeur` | Ajoute du texte à la fin | `concat var1 _fin` |
| `trim varX` | Retire les espaces | `trim var1` |
| `toUpper` / `toLower varX` | Change la casse | `toUpper var1` |

**Mobis**

| Commande | Effet | Exemple |
| --- | --- | --- |
| `movetobyid furniId X Y` | Déplace le mobi vers `X Y` | `movetobyid 123456 5 8` |
| `setstatebyid furniId state` | Met l'état d'un mobi | `setstatebyid 123456 1` |
| `addstatebyid furniId value` | Ajoute une valeur à l'état actuel | `addstatebyid 123456 1` |

**Déplacement**

| Commande | Effet | Exemple |
| --- | --- | --- |
| `moveto X Y` | Déplace le contexte actuel (mobis sélectionnés > mobi de l'évènement > entité déclencheuse) | `moveto 10 12` |
| `warpto X Y [Z]` | Téléporte le joueur déclencheur | `warpto 10 12 1.5` |

> Pour être explicite : `movetobyid` pour un mobi précis, `warpto` pour un joueur.

**Joueur**

| Commande | Effet |
| --- | --- |
| `forcesit` / `forcelay` | Force le joueur à s'asseoir/s'allonger |
| `teleport roomId` | Demande l'entrée dans une autre room |

**Paramètres de jeu** (rooms de jeux custom) : `setPvpAuto`, `setMaxZombies value` (1-50), `setMinZombies value` (1-25), `setSpawnFreq value` (1-30), `setMaxBoxes value` (1-100).

**Debug** : `echo texte` envoie un whisper `DEBUG: ...` au joueur — pratique pour tester, à éviter dans un jeu public (risque de spam).

## Conditions

*(dans un Wired Condition MeteorScript)*

**Comparaisons**

```text
var1 == 10        # égal
var1 != fini       # différent
%userPosX% < 10    # plus petit
var1 >= 1          # plus grand ou égal
```

**Présence sur une case**

```text
playerAt(5,8)
noPlayerAt(10,12)
```

**Types de valeur**

```text
isInt(var1)
isBool(%hasRights%)
!isNull(var2)      # inversion avec !
```

**Recherche dans un texte** — préférez `indexOf` à l'ancien `(in)` déconseillé :

```text
indexOf(%username%;Admin) != -1
```

## Fonctions

Utilisables dans les commandes **et** les conditions (et lues par les Wired classiques quand leur texte passe par MeteorScript).

**Nombres**

```text
rand(1,6)          -> nombre aléatoire entre 1 et 6
calc(2 + 3 x 4)     -> 14 (arrondi)
calc2(5 / 2)        -> 2.5 (garde les décimales)
round(2.6) / floor(2.9) / ceil(2.1)
```

**Texte** — avec `%username%` = `Meteor` :

```text
length(%username%)            -> 6
charAt(%username%;0)          -> M
substr(%username%;0;3)        -> Met
indexOf(%username%;e)         -> 1
typeof(var1)                  -> int / float / bool / str / null
```

**Mobis**

```text
getFurniState(123456)
getFurniCoords(123456)
```

Renvoient un texte d'erreur (ex : `InvalidFurniIdException`) si le mobi ou la room est invalide.

## Erreurs possibles

| Erreur | Cause typique |
| --- | --- |
| `SyntaxError` | Syntaxe incomprise (ex : `calc(2 + )`) |
| `NumberFormatException` | Nombre attendu mais valeur invalide |
| `IndexOutOfBoundsException` | Position hors de la taille du texte |
| `InvalidRoomException` | Room non disponible dans le contexte |
| `InvalidFurniIdException` | Id de mobi inexistant dans la room |
| `InvalidPositionException` | Mobi introuvable sur le plan de la room |
| `null` | Valeur vide ou absente |

⚠️ Une condition qui compare directement une erreur compare du texte — testez toujours vos ids de mobis avant de vous y fier.

## Exemples pratiques

**Compteur global**

```text
# Au lancement du jeu
set var1 0

# Action à chaque déclenchement
set var1 calc(var1 + 1)

# Condition pour déclencher autre chose
var1 >= 10
```

**Checkpoint par joueur**

```text
set usrvr1 checkpoint_2
```
```text
usrvr1 == checkpoint_2
```

**Ouvrir un passage si la case est libre**

```text
# Condition
noPlayerAt(8,10)
# Action
movetobyid 123456 8 10
```

**Récompense aléatoire**

```text
set var1 rand(1,3)
```
puis trois conditions séparées : `var1 == 1`, `var1 == 2`, `var1 == 3`.

**Déplacer les mobis sélectionnés vs un mobi précis**

```text
moveto 5 7          # déplace les mobis sélectionnés dans le Wired
movetobyid 123456 5 7  # déplace un mobi précis, peu importe la sélection
```

## Bonnes pratiques

- **Restez lisible** : séparez calcul et condition plutôt que tout empiler au même endroit.
- **Réservez des plages de variables** par rôle, par exemple :

  | Variables | Rôle |
  | --- | --- |
  | `var1` à `var10` | État global du jeu |
  | `var11` à `var20` | Scores |
  | `var21` à `var30` | Timers et cooldowns |
  | `usrvr1` à `usrvr5` | Progression du joueur |
  | `usrvr6` à `usrvr10` | Choix temporaires |

- **Vérifiez avant d'agir** : `%hasRights% == true`, `noPlayerAt(5,5)`, `isInt(var1)`...
- **Évitez le spam** : pas de changement d'état à chaque tick, pas de boucles sans pause, pas d'`echo` en public.
- **Préférez les Wired dédiés** à l'ancien préfixe `meteorscript:` dans les SuperWired.

## Limites connues

- Variables temporaires uniquement (persistance à venir avec **MeteorScript DB**).
- Commandes sensibles à la casse.
- Pas de vrai système de guillemets pour les arguments.
- `meteorscript:` gardé pour compatibilité mais déconseillé.
- `moveto` dépend du contexte — préférez `movetobyid`/`warpto` pour être explicite.
- `(in)` déconseillé pour les textes — utilisez `indexOf(...) != -1`.
- Un mauvais id de mobi renvoie un texte d'erreur, pas un crash.
- Trop de scripts lourds/fréquents peuvent atteindre les limites Wired de la room.

## Aide-mémoire

```text
set var1 10
set var1 calc(var1 + 1)
set usrvr1 %username%

var1 >= 10
usrvr1 == Cyb
%hasRights% == true
noPlayerAt(5,8)

movetobyid 123456 5 8
setstatebyid 123456 1
warpto 10 12
teleport 456

rand(1,6)
calc(2 + 3 x 4)
length(%username%)
indexOf(%username%;a)
```

---

Le meilleur réflexe avec MeteorScript : **faites petit, testez, puis assemblez**. Les gros systèmes deviennent beaucoup plus faciles à corriger quand chaque Wired a un rôle clair.
