+++
title = "Notes de cours"
weight = 3
[params]
  author = "Équipe : Natali P., Nermine G., Goufran A."
+++

# Notes de cours — Simulation numérique d’un trou noir en C++

## 1. Introduction

Les **trous noirs** sont parmi les objets les plus mystérieux et impressionnants de l’Univers. Un trou noir est une région de l’espace où la matière est tellement comprimée que la **gravité** devient extraordinairement forte. Cette gravité est si intense qu’elle empêche même la **lumière** de s’échapper et c’est pourquoi les trous noirs apparaissent complètement noirs.

Un trou noir se forme généralement lorsqu’une **étoile massive** (étoile beaucoup plus lourde que le Soleil) arrive en fin de vie. En épuisant son carburant, elle ne peut plus résister à sa propre gravité et **s’effondre** sur elle-même. La matière est alors compressée dans un volume minuscule, créant un objet extrêmement dense. 

![Illustration d’un trou noir et de son environnement](/Pictures/intro1.png)

Un élément fondamental pour comprendre les trous noirs est la **relativité générale**, la théorie publiée par Albert Einstein en 1915. Cette théorie explique que la gravité n’est pas une simple force comme en physique classique, mais une conséquence de la **courbure de l’espace-temps** (l’espace-temps est un modèle qui combine l’espace et le temps en une seule “structure”). Une manière simple de visualiser cela est d’imaginer une nappe tendue : si on pose une boule lourde dessus, la nappe se déforme. Les objets plus petits roulent alors vers la boule, non parce qu’elle les “attire” directement, mais parce que la surface est déformée.

Les trous noirs créent une courbure si extrême que toutes les trajectoires possibles des objets et de la lumière sont dirigées vers leur centre.

![Schéma simplifié montrant la silhouette d’un trou noir et la lumière déformée autour de lui](/Pictures/intro2.png)

Même si on ne peut pas “voir” un trou noir directement (puisqu’il n’émet pas de lumière), on peut observer ses **effets** sur ce qui l’entoure :
- la lumière des étoiles d’arrière-plan qui se déforme,
- le gaz très chaud qui tourne autour de lui,
- ou les étoiles proches qui semblent accélérer de manière anormale.

Dans ce projet, nous voulons créer **une simulation numérique en C++** qui reproduit ces effets de manière simplifiée, afin d’aider des étudiants débutants à visualiser comment un trou noir influence la lumière.

Pour y arriver, nous devons d’abord comprendre quatre notions physiques de base : 
1. la **courbure de l’espace-temps** ;
2. les **géodésiques** ;
3. l’**horizon des événements** ;
4. la **singularité**.

## 2. Concepts physiques

### 2.1 Courbure de l’espace-temps

Dans la relativité générale, l’Univers n’est pas constitué d’un simple “espace” avec le temps séparé à côté. Les deux sont réunis dans une structure unique : **l’espace-temps**. Dès qu’un objet possède de la **masse** (quantité de matière), il déforme cette structure. C’est ce qu’on appelle la **courbure de l’espace-temps**.

![Schéma de la courbure de l’espace-temps par une masse centrale](/Pictures/courbure1.png)

Comme l’explique Sherpas Physique, une bonne analogie consiste à imaginer un drap ou une nappe élastique tendue :
- si on pose une petite bille dessus, la nappe se déforme à peine ;
- si on pose une boule de bowling, la nappe s’enfonce beaucoup.

Dans cette image :
- le drap représente l’espace-temps ;
- la boule représente un objet massif (une planète, une étoile ou un trou noir) ;
- la “cuvette” formée dans le drap représente la courbure.

Un objet qui veut aller “tout droit” sur cette surface doit en réalité suivre la forme déformée du drap. Vu de dessus, sa trajectoire semble courbée : c’est ce que nous interprétons comme l’effet de la gravité.

Près d’un trou noir, la masse est tellement concentrée que la courbure de l’espace-temps devient extrêmement profonde. Les lignes qui représentent l’espace-temps s’enfoncent presque verticalement, ce qui force tout ce qui passe à proximité, y compris la lumière, à tomber vers le centre si l’objet s’approche trop.

![Représentation plus prononcée de la courbure près d’un trou noir](/Pictures/courbure2.png)

Dans notre simulation, nous ne résolvons pas les équations complètes d’Einstein. À la place, nous **imitons** l’effet de la courbure en faisant dévier progressivement la direction des rayons lumineux en fonction de leur distance au centre du trou noir : plus ils sont proches, plus on les courbe.

---

### 2.2 Géodésiques

Pour décrire comment les objets se déplacent dans un espace courbé, la notion centrale est celle de **géodésique**.

> **Géodésique** : chemin le plus “droit possible” dans une surface ou un espace qui peut être courbé.

En géométrie :
- sur une feuille de papier (espace plat), une géodésique est une **ligne droite** ;
- sur une sphère (comme la Terre), une géodésique est un **grand cercle** (par exemple, l’équateur ou certaines trajectoires d’avion).

![Exemple de géodésiques sur une surface courbée](/Pictures/geo1.png)

Dans un espace-temps courbé par la gravité, les géodésiques sont les trajectoires naturelles que suivent les objets lorsqu’aucune autre force ne s’exerce sur eux. Pour la lumière, on parle de **géodésiques nulles** (ce sont les chemins que suivent les photons à la vitesse de la lumière).

Près d’un trou noir, ces géodésiques peuvent devenir :
- légèrement courbées (la lumière est simplement déviée),
- très fortement courbées (la lumière peut faire un ou plusieurs tours autour du trou noir),
- complètement piégées (la lumière tombe dans le trou noir).

![Schéma de rayons lumineux (géodésiques) passant à proximité d’un trou noir](/Pictures/geo2.png)

Dans une simulation numérique réaliste, on devrait résoudre les équations différentielles qui décrivent ces géodésiques. Cela demande beaucoup de mathématiques et de calculs. Dans notre projet pédagogique, nous utilisons une **approximation** :
- nous représentons chaque rayon lumineux par une petite flèche (un vecteur direction),
- à chaque étape de son trajet, nous modifions légèrement cette direction selon la distance au trou noir,
- le résultat final ressemble à une géodésique, sans avoir à appliquer toute la théorie mathématique.

---

### 2.3 Horizon des événements

L'**horizon des événements** est une notion essentielle pour comprendre ce qui rend un trou noir “noir”.

> **Horizon des événements** : frontière invisible autour du trou noir à partir de laquelle **plus rien ne peut s’échapper**, ni matière ni lumière.

Cette frontière est comme un “point de non-retour”. À l’intérieur de cette surface, la courbure de l’espace-temps est tellement forte que la **vitesse de libération** (la vitesse minimale nécessaire pour s’éloigner à l’infini) serait plus grande que la **vitesse de la lumière**, ce qui est interdit par les lois de la physique actuelles.

![Vue schématique d’un trou noir avec son horizon des événements](/Pictures/horizon1.png)

Pour un trou noir idéal, non en rotation, la distance correspondant à l’horizon des événements est liée à la masse du trou noir par le **rayon de Schwarzschild** (un rayon caractéristique qui dépend de la masse). Tout ce qui franchit cette frontière est irrémédiablement perdu pour l’Univers extérieur.

Dans notre simulation, l’horizon des événements joue un rôle très pratique :
- nous fixons un rayon minimal autour du centre ;
- si un rayon lumineux s’en rapproche trop (en dessous de ce rayon), nous considérons qu’il a traversé l’horizon ;
- dans ce cas, nous arrêtons le calcul de ce rayon et nous affichons un pixel **noir**.

![Représentation de la zone d’ombre et de l’horizon autour d’un trou noir](/Pictures/horizon2.png)

C’est ce mécanisme qui permet de créer numériquement l’ombre centrale du trou noir, similaire à celle observée dans les images réelles (par exemple l’image du trou noir supermassif M87*).

---

### 2.4 Singularité

Au centre d’un trou noir se trouve ce que l’on appelle la **singularité**.

> **Singularité** : région théorique où la densité de matière et la courbure de l’espace-temps deviennent infinies selon la relativité générale.

![Schéma illustrant la position de la singularité au centre du trou noir](/Pictures/singularite.png)

La singularité représente un point où nos lois actuelles de la physique ne fonctionnent plus correctement. Les quantités physiques comme la densité et la courbure deviennent infinies dans les équations, ce qui indique probablement qu’il manque encore une théorie plus complète, qui combinerait la relativité générale et la mécanique quantique.

Quelques points importants à retenir :
- la singularité n’est **jamais observable directement**, car elle est entièrement cachée derrière l’horizon des événements ;
- elle n’a pas de taille visible : on la considère souvent comme un point ou une région extrêmement petite ;
- elle marque la limite de validité de nos modèles actuels.

Dans notre projet de simulation pédagogique, nous ne tentons pas de représenter la singularité elle-même. Nous nous concentrons sur ce que nous pouvons décrire de manière plus simple :
- l’existence d’un **horizon des événements** ;
- la **forte courbure** de l’espace-temps autour du trou noir ;
- les trajectoires de la **lumière** (géodésiques approximées) qui en résultent.

En résumé, pour la simulation, il suffit de considérer qu’en dessous d’un certain rayon (l’horizon), tout est définitivement perdu, sans entrer dans les détails de ce qui se passe exactement au centre du trou noir.



## 3. Modélisation mathématique
### 3.1 Métrique de Schwarzschild
### 3.2 Déviation des rayons lumineux
### 3.3 Approximation utilisée

## 4. Implémentation en C++
### 4.1 Architecture du programme
### 4.2 Fonctions de calcul
### 4.3 Rendu (console ou graphique)

## 5. Limites du modèle

## 6. Conclusion

## 7. Sources
