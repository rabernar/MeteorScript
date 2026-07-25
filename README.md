

MeteorScript est un mini-langage de scripting conçu pour les créateurs de jeux Wired.

Il permet de créer des systèmes avancés avec :
- des variables temporaires ;
- des calculs ;
- des conditions ;
- des déplacements de joueurs et mobis ;
- des manipulations d'états ;
- des systèmes de mini-jeux complets.

Son objectif est de simplifier les systèmes qui deviennent trop complexes avec les Wired classiques seuls.

---

# 📚 Sommaire

- [Introduction](#introduction)
- [Où utiliser MeteorScript](#où-utiliser-meteorscript)
- [Règles générales](#règles-générales)
- [Variables](#variables)
- [Variables automatiques](#variables-automatiques)
- [Commandes](#commandes)
- [Conditions](#conditions)
- [Fonctions](#fonctions)
- [Exemples](#exemples)
- [Bonnes pratiques](#bonnes-pratiques)

---

# 🚀 Introduction

Les Wired classiques sont très utiles pour déclencher des événements et créer des interactions simples.

Cependant, dès qu'un système demande :
- plusieurs calculs ;
- des scores ;
- des sauvegardes temporaires ;
- des choix aléatoires ;
- plusieurs conditions ;

MeteorScript devient beaucoup plus adapté.

Exemples d'utilisation :

✅ Compteur de joueurs  
✅ Classement  
✅ Jeu de dés  
✅ Checkpoints personnels  
✅ Mini-jeux avec équipes  
✅ Systèmes aléatoires  

---

# 📍 Où utiliser MeteorScript

| Wired | Utilisation |
|-|-|
| **Wired Action MeteorScript** | Exécute une commande |
| **Wired Condition MeteorScript** | Vérifie une condition |

L'ancien format : dans un Wired Action MeteorScript.

---

# ⚙️ Règles générales

- Les commandes sont sensibles aux majuscules.
- Les arguments sont séparés par des espaces.
- Les coordonnées utilisent le format :