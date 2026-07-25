# Wired classiques — Guide simple

Les Wired, c'est le système de base pour donner du comportement à une room : un **déclencheur** lance l'action, une **condition** (optionnelle) filtre quand ça doit se passer, et un **effet** fait quelque chose. Ce guide liste les trois familles avec une explication simple de chacune.

## Sommaire

- [Déclencheurs](#déclencheurs)
- [Effets](#effets)
- [Conditions](#conditions)

## Déclencheurs

Un déclencheur, c'est ce qui démarre la pile Wired.

| Déclencheur | Se déclenche quand... |
| --- | --- |
| L'utilisateur marche sur un mobi | un joueur se tient debout ou assis sur le mobi choisi |
| L'utilisateur quitte le mobi | un joueur descend du mobi sur lequel il était |
| Le Bot atteint un utilisateur | le Bot sélectionné arrive jusqu'à un joueur |
| Le Bot atteint un mobi | le Bot sélectionné arrive jusqu'au mobi choisi |
| L'utilisateur prononce le mot de passe | un joueur écrit un mot précis dans le chat |
| Le mobi change d'état | on double-clique sur le mobi sélectionné |
| L'utilisateur entre dans l'appart | un joueur entre dans la room |
| L'effet se répète | à intervalle régulier, toutes les X secondes |
| Minuteur | X secondes après que le minuteur ait été relancé |
| Score obtenu | le score défini est atteint |
| Fin de partie | une partie de Battle Banzai se termine |
| La partie commence | un chrono démarre dans la room |
| Collision mobi/utilisateur | un mobi en mouvement (via Fuite ou Poursuite) percute un joueur — le joueur touché devient le déclencheur |
| Périodique long | comme "l'effet se répète", mais pour des intervalles plus longs |

## Effets

Un effet, c'est l'action réellement exécutée.

**Sur les mobis**

| Effet | Ce que ça fait |
| --- | --- |
| Modifie l'état du mobi | change l'état comme un double-clic |
| Déplace et tourne le mobi | change sa position et son orientation |
| Réinstalle le mobi dans son état initial | remet le mobi comme au départ |
| Change la direction du mobi | fait pivoter le mobi si son chemin est bloqué ; peut déclencher une collision si un joueur est à côté |
| Poursuite | fait avancer les mobis vers le joueur le plus proche sur la même ligne/colonne — peut déclencher une collision |
| Fuite | éloigne le mobi d'une case par rapport au joueur le plus proche |

**Sur les Bots**

| Effet | Ce que ça fait |
| --- | --- |
| Téléporte le Bot vers un mobi | déplace instantanément le Bot |
| Le Bot se déplace vers un mobi | fait marcher le Bot jusqu'au mobi |
| Le Bot suit l'utilisateur | le Bot suit le joueur déclencheur |
| Le Bot change de vêtements | modifie l'apparence du Bot |
| Le Bot parle à tous les utilisateurs | le Bot parle/crie pour toute la room |
| Le Bot parle ou murmure à l'utilisateur | le Bot s'adresse seulement au joueur déclencheur |
| Le Bot donne un objet | le Bot remet un objet choisi |

**Sur le joueur**

| Effet | Ce que ça fait |
| --- | --- |
| Téléporte vers un mobi | téléporte le joueur déclencheur ; si plusieurs mobis sont sélectionnés, la destination est tirée au hasard |
| Expulser utilisateur | éjecte le joueur qui correspond aux critères |
| Affiche un message | montre un message privé, visible seulement par celui qui a déclenché |

**Jeu / équipes**

| Effet | Ce que ça fait |
| --- | --- |
| Donne des points | ajoute des points à l'équipe du joueur déclencheur ; tu peux limiter combien de fois ça peut arriver |
| Donner score | attribue un score à une équipe définie à l'avance |
| Rejoindre équipe | fait entrer le joueur déclencheur dans une équipe définie |
| Quitter équipe | fait sortir le joueur déclencheur de son équipe |

**Autres**

| Effet | Ce que ça fait |
| --- | --- |
| Exécuter piles Wired | déclenche jusqu'à 5 piles Wired en même temps, peu importe leurs propres conditions/déclencheurs |
| Réinitialise le minuteur | relance le/les minuteur(s) |
| Verrou : effet aléatoire | empêche que deux effets aléatoires tournent en même temps |
| Verrou : effet cyclique | empêche que deux effets cycliques tournent en même temps |

## Conditions

Une condition filtre si l'effet doit vraiment se déclencher.

**Conditions positives**

| Condition | Vrai si... |
| --- | --- |
| Membre du groupe | la room appartient à un groupe et le joueur en fait partie |
| Doit être recouvert par un mobi | un des mobis sélectionnés a un autre mobi posé dessus |
| L'utilisateur a un objet | le joueur tient l'objet choisi en main |
| Membre d'une équipe | le joueur fait partie de l'équipe définie |
| Il y a un utilisateur sur chaque mobi | tous les mobis sélectionnés ont quelqu'un dessus |
| L'état et la position des mobis correspondent | les mobis sélectionnés sont dans l'état ET à la position attendus |
| Le type de mobi correspond | le mobi déclencheur est bien un de ceux sélectionnés (ou du même type) |
| Moins de X secondes après la relance du minuteur | le minuteur a été relancé il y a moins de X secondes |
| Plus de X secondes après la relance du minuteur | le minuteur a été relancé il y a plus de X secondes |
| L'utilisateur est sur l'un des mobis sélectionnés | le joueur se trouve sur un des mobis choisis |
| Nombre d'utilisateurs dans la salle | le nombre de joueurs présents est dans la fourchette définie |

**Conditions négatives** *(l'inverse des précédentes)*

| Condition | Vrai si... |
| --- | --- |
| N'a PAS de mobis dessus | aucun des mobis sélectionnés n'a un autre mobi posé dessus |
| Il n'y a PAS d'utilisateur dessus | personne ne se trouve sur les mobis sélectionnés |
| PAS membre du groupe | c'est une room de groupe et le joueur n'en fait pas partie |
| PAS membre de l'équipe | le joueur ne fait pas partie de l'équipe définie |
| Les états des mobis NE correspondent PAS | les états des mobis sélectionnés sont différents de ceux attendus |
| Les types de mobi NE correspondent PAS | le mobi déclencheur n'est ni sélectionné ni du même type |
| L'utilisateur n'est PAS sur l'un des mobis | le joueur déclencheur n'est sur aucun des mobis choisis |
| La limite d'utilisateurs n'est PAS atteinte | le nombre de joueurs dans la room est en dehors des limites définies |

---

💡 En résumé : **déclencheur** = ce qui lance la pile, **condition** = le filtre optionnel, **effet** = ce qui se passe pour de vrai. Combine-les pour construire des mécaniques plus fines qu'un simple "si X alors Y".
