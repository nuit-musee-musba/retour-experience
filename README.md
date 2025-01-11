# Retour d'expérience

Ce document vise à collecter les différentes expériences/problèmes et solutions trouvées durant les nuits du MusBA.

Il peut être utile pour que d'année en année le rendu gagne en qualité et que chaque promotion en apprenne plus encore.

> [!IMPORTANT]
> Ce document est collaboratif, n'hésitez pas à **ajouter** vos expériences au fur et à mesure, à ajouter de nouvelles solutions, etc  
> **Évitez par contre de supprimer !**

# Problèmes rencontrés et leurs solutions
## Il n'y a pas de réseau sur l'ordinateur du MusBA
### En 2024
> [!IMPORTANT]
> Connecter le PC à internet demanderait bcp de sécurité/mises au normes donc infaisable pour l'instant.
> Les expériences actuelles et futures doivent impérativement être totalement hors-ligne

L'ordinateur du MusBA ne peut pas être connecté au réseau par mesure de sécurité

#### Solution
Faire une expérience totalement hors-ligne
- Installer les librairies sur le projet (par ex via npm, yarn, pnpm, ...)
- Stocker les images localement
- Ne pas dépendre d'api externe
- Etc.

## La mémoire sature au bout d'une ou plusieurs heures
### En 2024
> [Voir le ticket associé](https://github.com/nuit-musee-musba/experience-2024/issues/177)

Nous avons des expériences 3D avec sans doute des [fuites de mémoires](https://fr.wikipedia.org/wiki/Fuite_de_m%C3%A9moire): lorsqu'on allait sur une expérience 3D, puis qu'on y retournait,etc, on observait que le pc ramait de plus en plus.

Nous n'avons pas le temps de corriger le problème à la source ni d'être sûr que c'étaient les seules expériences avec des problèmes de mémoire.

#### Solution
> [!TIP]
> Sur chrome, chaque onglet est un processus. Fermer l'onglet permet de libérer la mémoire à coup sûr. Comme on a une fuite de mémoire et que l'onglet sature, on ouvre une expérience dans un nouvel onglet et on ferme l'onglet actuel. On bénéficie alors du système de Chrome qui vide la mémoire à coup sûr.

1. Ouvrir l'index/hub via un script pour que chrome autorise la fermeture de l'onglet via un script ([commit](https://github.com/nuit-musee-musba/experience-2024/pull/178/commits/2a555c4562b2564bd4f27578efa16685a5cb46cc))
2. Sur l'index/hub, ouvrir l'expérience dans un nouvel onglet et fermer l'onglet actuel ([commit 1](https://github.com/nuit-musee-musba/experience-2024/pull/178/commits/54fc342e67d6ce4927c189028ae294faba437c11) + [commit 2](https://github.com/nuit-musee-musba/experience-2024/pull/178/commits/1b2993b3b76f32aa0ac91f09b0b024f88f230736))


## On ne peut pas accéder à la table pour tester
### En 2024
La tablette n'est pas toujours accessible. En fait, nous y avons accès qu'une fois pour tout le projet, et 1 fois pour la mise en place finale.

#### Solution
1. Identifier ce qu'on ne peut tester qu'avec la tablette :
   2. Responsive
   3. Installation
   4. Tactile
   5. (rajouter si j'ai oublié des trucs)
2. Si un accès à la tablette, tester en priorité les points ci-dessus, le reste on peut le tester sur un pc classique.
3. Si pas d'accès, essayer de trouver un écran tactile **ou un vidéo projecteur** avec une taille ressemblante.
4. Si pas d'écran avec taille ressemblante, tester sur un pc en simulant la taille de l'écran avec [BetterDisplay](https://github.com/waydabber/BetterDisplay) ou via [l'inspecteur d'éléments](https://drive.google.com/file/d/13nn7Nf9MTph6T_OHQdIMjQydiNKbts94/view) 

## Le site n'est responsive que pour la table tactile (pas pour les ordi)
### En 2024

Nous priorisons l'affichage correct sur la table du MusBA, au détriment du responsive. Au final, on ne peut pas visiter l'expérience depuis un ordinateur. 

#### Alternative trouvée
Mettre en ligne un [site qui présente le projet avec des vidéos](https://nuit-du-musba-2024.netlify.app/).

#### Solution imaginable
> [!WARNING]
> Cette solution n'a pas été testée et est peut-être difficile à mettre en place
> Mettre en place un site qui présente le projet est une bonne alternative : pas de complexité additive sur l'expérience + présentation simple et maîtrisée (sans bug mdr) du projet pour le portfolio 
1. Utiliser des unités relatives (vh, vw pour le positionnement, rem pour la font, border, padding etc), proscrire les unités absolues
2. Changer la font-size du root en fonction de la taille de l'écran pour que les unités relatives évoluent en harmonie avec la taille de l'écran

## Le tactile de la table est sensible à tout contact (manche, cheveux, bracelet)
### En 2024
Les utilisateurs sont constamment gênés par leurs cheveux, leur manche etc qui appuie n'importe où.

#### Solution
> [!TIP]
> Ce fonctionnement est dû à la technologie tactile infrarouge de la tablette tactile : l'écran détecte les "ombres" sur l'écran plutôt que la pression. 

Tirons parti de la technologie tactile désastreuse de cet écran pour essayer de filtrer les entrées tactiles :
Récupérer la taille du touch event et filtrer s'il est trop gros/trop petit/pas rond ([Pull-request](https://github.com/nuit-musee-musba/experience-2024/issues/179#issue-2313725906))

## Utiliser la carte graphique sur PC du MusBA
### En 2024
Nous avons des expériences 3D ce qui nécessite une carte graphique.
L'ordinateur du MusBA a une bonne carte graphique, mais lorsqu'on l'allume en ayant la table branchée sur le port de la carte graphique, aucune image.

#### Solution
> [!WARNING]
> Solution peutêtre mal documentée

1. Il me semble qu'il faut brancher la table sur le port de la carte mère, pas sur la carte graphique
2. Ensuite, il faut dire à l'ordinateur d'utiliser la carte graphique pour Chrome
3. Étonnant mais fonctionnel, je crois qu'on arrive ainsi à utiliser la carte graphique sans être branché directement dessus (je dis peut-être des bêtises, en tout cas ça ne laguait plus).

## Des enfants éteignent l'écran de la table tactile
### En 2024
Lors de la soirée, des enfants ont appuyé sur le bouton "on/off" de l'écran (normal, il est illuminé, coloré, il faut absolument appuyer dessus).
Il faut donc rallumer l'écran soi-même (car une fois éteint l'enfant se désintéresse du bouton et n'appuie plus dessus).

#### Solution trouvée
[Désactiver le bouton en appuyant sur le bouton rond vert de la télécommande](https://github.com/nuit-musee-musba/experience-2024/issues/181#issuecomment-2245288527) 
