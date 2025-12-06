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

## 4. Connaissances C++ nécessaires

Pour simuler un trou noir en C++, il n'est pas nécessaire de maîtriser toute la Relativité Générale. On utilise des éléments fondamentaux du langage : mathématiques, structures de données et affichage graphique. Cette section détaille les compétences préalables requises pour programmer une telle simulation, en s'inspirant des techniques de ray tracing et des implémentations de simulations de trous noirs existantes.

---

### 4.1 Calcul scientifique de base

La lumière autour d'un trou noir ne se déplace pas en ligne droite : elle est courbée par la gravité intense. Pour simuler ce phénomène, nous avons besoin d'outils mathématiques essentiels en C++.

#### Vecteurs et matrices

La manipulation des **positions et directions** dans l'espace tridimensionnel est cruciale. Comme dans les techniques de ray tracing (voir [Ray Tracing in One Weekend](https://raytracing.github.io/books/RayTracingInOneWeekend.html)), les vecteurs 3D sont la base de tous les calculs géométriques.

Un **vecteur 3D** sert à représenter :

- une **position** dans l'espace (x, y, z),
- une **direction** de déplacement,
- une **force** ou une accélération.

Voici une structure de base pour un vecteur 3D :

```cpp
struct Vec3 {
    double x, y, z;
    
    Vec3 operator+(const Vec3& v) const { return {x + v.x, y + v.y, z + v.z}; }
    Vec3 operator-(const Vec3& v) const { return {x - v.x, y - v.y, z - v.z}; }
    Vec3 operator*(double k) const { return {x * k, y * k, z * k}; }
    Vec3 operator/(double k) const { return {x / k, y / k, z / k}; }
    
    double length() const { return sqrt(x*x + y*y + z*z); }
    Vec3 normalize() const { return *this / length(); }
};
```

Dans la simulation, chaque rayon lumineux se déplace selon une équation de ce type :

```cpp
position = position + direction * distance;
```

Sans vecteurs, on ne pourrait pas faire avancer le rayon dans l'espace courbé autour du trou noir.

#### Géométrie 3D : produits scalaires, normes, coordonnées sphériques

La géométrie 3D est essentielle pour calculer les trajectoires des rayons lumineux. Les concepts suivants sont fondamentaux :

- **Produit scalaire** : permet de déterminer l'angle entre deux vecteurs et de savoir si un rayon se rapproche ou s'éloigne du trou noir.

```cpp
double dot(const Vec3& a, const Vec3& b) {
    return a.x*b.x + a.y*b.y + a.z*b.z;
}
```

- **Norme d'un vecteur** : mesure la distance du rayon au centre du trou noir, essentielle pour calculer l'intensité de la courbure.

```cpp
double length(const Vec3& v) {
    return sqrt(v.x*v.x + v.y*v.y + v.z*v.z);
}
```

- **Coordonnées sphériques** : particulièrement utiles pour la métrique de Schwarzschild, elles permettent de représenter un point en fonction de sa distance radiale et de ses angles. La conversion entre coordonnées cartésiennes et sphériques est souvent nécessaire :

```cpp
// Conversion de coordonnées sphériques (r, θ, φ) à cartésiennes
Vec3 sphericalToCartesian(double r, double theta, double phi) {
    return {
        r * sin(theta) * cos(phi),
        r * sin(theta) * sin(phi),
        r * cos(theta)
    };
}
```

Ces fonctions sont indispensables pour calculer correctement la déviation des rayons lumineux dans l'espace-temps courbé.

#### Fonctions mathématiques : sqrt(), sin(), cos(), atan2()

Ces fonctions proviennent de la bibliothèque standard C++ `#include <cmath>` et sont la base de presque toutes les simulations physiques.

Rôle dans la simulation :

- `sqrt()` → calculer des distances dans l'espace (norme de vecteurs),
- `sin()` / `cos()` → générer des directions de rayons selon un angle, convertir entre coordonnées,
- `atan2()` → obtenir l'angle d'un vecteur (utile pour les conversions de coordonnées et certains effets visuels).

Ces fonctions sont utilisées à chaque étape du calcul des trajectoires des rayons lumineux.

### 4.2 Structures de données

Pour gérer efficacement une grande quantité de rayons lumineux et de pixels, on utilise des structures de données simples mais puissantes du C++.

#### Tableaux dynamiques : std::vector

Pour simuler une image, on lance un rayon par pixel. Par exemple, une image 800×600 contient **480 000 pixels**, ce qui signifie **480 000 rayons** à calculer.

On utilise donc un tableau dynamique :

```cpp
#include <vector>

std::vector<Ray> rays;
```

Pourquoi `std::vector` plutôt qu'un tableau statique ?

- on ne connaît pas toujours la taille à l'avance (résolution variable),
- `std::vector` gère automatiquement la mémoire (allocation/désallocation),
- il est facile à parcourir avec des boucles (`for (auto& ray : rays)`),
- il peut contenir un très grand nombre de rayons sans limite fixe.

Cette approche est similaire à celle utilisée dans les implémentations de ray tracing, où chaque pixel de l'image finale correspond à un rayon calculé.

#### Structures simples : Ray { position, direction, couleur }

Un rayon lumineux est représenté par une structure simple qui encapsule toutes ses propriétés :

```cpp
struct Color {
    unsigned char r, g, b;
};

struct Ray {
    Vec3 position;    // Position actuelle du rayon
    Vec3 direction;      // Direction de propagation (vecteur normalisé)
    Color color;         // Couleur finale du pixel
};
```

Rôle de cette structure :

- `position` → où se trouve le rayon à un instant donné dans l'espace,
- `direction` → vers où il se déplace (vecteur unitaire),
- `color` → couleur finale du pixel qu'il représente dans l'image.

Chaque pixel final de l'image correspond à un rayon simulé. Cette structure permet de suivre l'évolution de chaque rayon à travers l'espace-temps courbé, en mettant à jour sa position et sa direction à chaque étape du calcul.

Sans cette structure, il serait impossible de suivre correctement l'évolution de la lumière dans la simulation, notamment lorsque les rayons sont déviés par la courbure de l'espace-temps autour du trou noir.

### 4.3 Affichage graphique

Une fois que tous les pixels ont été calculés, il faut afficher l'image finale. Il existe deux approches principales : un rendu texte simple (ASCII) pour le débogage, ou un rendu graphique avec OpenGL pour une visualisation réaliste.

#### Rendu console (ASCII) — option simple

Méthode très simple pour valider rapidement la logique de la simulation : afficher des caractères en fonction de la luminosité ou de la distance à l'horizon du trou noir.

```cpp
char pixelFor(double intensity) {
    if (intensity < 0.2) return ' ';  // Très sombre
    if (intensity < 0.4) return '.';  // Sombre
    if (intensity < 0.6) return '*';   // Moyen
    if (intensity < 0.8) return 'o';  // Clair
    return '@';                        // Très clair
}
```

Ce type de rendu permet de valider la logique de la simulation sans utiliser d'interface graphique complexe. C'est utile pour les premières étapes de développement et le débogage.

#### Rendu graphique avec OpenGL — rendu accéléré par GPU

Pour un rendu plus réaliste et performant, on utilise **OpenGL** pour envoyer l'image finale au GPU (carte graphique) et l'afficher dans une fenêtre. Cette approche est utilisée dans des implémentations pour visualiser les simulations de trous noirs.

On suppose qu'on a déjà rempli un tableau de pixels avec les résultats de la simulation :

```cpp
std::vector<Color> image(width * height);
// ... calcul de chaque pixel ...
```

Après avoir calculé chaque pixel, on peut envoyer cette image à la carte graphique :

```cpp
glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB,
             width, height, 0,
             GL_RGB, GL_UNSIGNED_BYTE,
             image.data());
```

**Pourquoi OpenGL ?**

- **Performance** : rendu accéléré par GPU, essentiel pour des simulations en temps réel,
- **Flexibilité** : permet d'afficher des images complètes à l'écran avec des animations fluides,
- **Standard** : bibliothèque largement utilisée et bien documentée.

**Important** : OpenGL ne fait **pas** la simulation physique. Il sert uniquement à afficher l'image calculée par votre programme C++. Tous les calculs de trajectoires des rayons lumineux, de courbure de l'espace-temps et de déviation gravitationnelle doivent être effectués dans votre code C++ avant l'affichage.

## 5. Limites du modèle

## 6. Conclusion

## 7. Sources

### Concepts physiques

- NASA, *Black Holes*, [imagine.gsfc.nasa.gov](https://imagine.gsfc.nasa.gov/science/objects/black_holes1.html) — Introduction aux trous noirs pour étudiants

- Sherpas Physique, *Courbure de l'espace-temps*, [sherpas.com](https://sherpas.com/p/physique/courbure-espace-temps.html) — Explication de la courbure de l'espace-temps et de son effet sur le mouvement des objets

- Space.com, *What is a black hole event horizon (and what happens there)?*, [space.com](https://www.space.com/black-holes-event-horizon-explained.html) — Explication de l'horizon des événements et de la singularité

- Wikipedia, *Schwarzschild metric* — Métrique de Schwarzschild et coordonnées sphériques

### Implémentation technique

- Peter Shirley, *Ray Tracing in One Weekend*, [raytracing.github.io](https://raytracing.github.io/books/RayTracingInOneWeekend.html) — Techniques de base du ray tracing en C++

- 20k, *Schwarzschild Black Hole Simulation*, [20k.github.io](https://20k.github.io/c%2B%2B/2024/05/31/schwarzschild.html) — Implémentation C++ d'une simulation de trou noir de Schwarzschild

- Code source de référence : [github.com/20k/20k.github.io](https://github.com/20k/20k.github.io/blob/master/code/schwarzschild/main.cpp) — Exemple d'implémentation complète
