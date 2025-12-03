<<<<<<< HEAD
+++
title = "Veille technologique"
weight = 2
[params]
  author = "Équipe : Natali P., Nermine G., Goufran A."
+++

# Veille technologique — Simulation de trou noir en C++

## 1. Premier prompt LLM utilisé
(Coller votre premier vrai prompt ici.)

## 2. Réponse du modèle
(Coller la réponse générée.)

## 3. Analyse critique de la réponse
(Dire si c’est complet, exact, biaisé, etc.)

## 4. Sources retenues pour les notes de cours
(Expliquer la fiabilité, expertise scientifique, etc.)
> [!tip]- Trou noir : l’hypothèse de Stephen Hawking testée en labo | Trous noirs de labo
> {{< youtube O4wFxNl4uWM>}}

Dans cette vidéo, on comprend une partie importante de la théorie derrière les trous noirs, mais expliquée d’une façon beaucoup plus concrète. Au lieu de juste montrer des animations ou des dessins, la vidéo se passe dans un vrai laboratoire. On voit des machines, des lasers, des lentilles et des miroirs, et ca nous aide à imaginer comment les scientifiques testent des idées qui semblent impossibles à tester.
Ce que j’ai trouvé intéressant, c’est que la vidéo ne parle pas seulement de (c’est quoi un trou noir), mais aussi de comment on peut vérifier en labo certaines hypothèses de Stephen Hawking. On a vraiment l’impression d’être avec eux dans le laboratoire.
On a aussi aimé que ce ne soit pas une seule personne qui parle, plusieurs chercheurs expliquent chacun une partie, donc on ne s’ennuie pas. Les explications sont simples, même si le sujet est compliqué. On voit des schémas, des tests et des manipulations réelles, ce qui rend la théorie plus vivante.
En gros, cette vidéo nous a surtout aidées à voir la théorie appliquée en vrai, et ça complète bien les vidéos plus théoriques.

> [!tip]- Simuler des collisions de trous noirs grâce aux jeux vidéo | Trous noirs de labo
> {{< youtube ciKIdIofW_U>}}

Dans cette vidéo, j’ai aimé le fait qu’ils expliquent ce qu’est un trou noir, mais à partir d’une vraie simulation de jeu vidéo. Ça rend tout plus concret, parce qu’on ne regarde pas juste des dessins ou des théories. On voit une vraie scène simulée, avec des effets visuels, du son, et des animations qui montrent comment deux trous noirs pourraient se comporter quand ils se rapprochent.
La vidéo montre aussi en détail comment les collisions de trous noirs peuvent être visualisées, la lumière qui se déforme, l’espace-temps qui tourne, les formes étranges qui apparaissent autour. Ça aide beaucoup à imaginer ce qu’on va essayer de simuler dans notre projet.
Ce que j’ai aimé, c’est que ça motivate. On voit le résultat final, un rendu vraiment beau, et ça donne envie d’apprendre comment c’est fait. Les images de jeux vidéo montrent bien comment la théorie peut devenir quelque chose de visuel et presque réel. Ça complète super bien les vidéos plus scientifiques, parce qu’ici on comprend vraiment ce qu’on verra à l’écran quand on va programmer.

## 5. Article récent obligatoire
– Lien  
– Résumé  
– Importance  

## 6. Sources réseaux sociaux / YouTube
(Toujours critique, jamais sensationnaliste.)

## 7. Synthèse
(Conclusion sur l’état actuel de la technologie.)
=======
+++
title = "Veille technologique"
weight = 2
[params]
  author = "Équipe : Natali P., Nermine G., Goufran A."
+++

# Veille technologique — Simulation de trou noir en C++

## 1. Premier prompt LLM utilisé

Pour entamer notre recherche, nous avons choisi de comparer les résultats fournis par un modèle LLM (ChatGPT) avec ceux d’un moteur de recherche traditionnel. L’objectif était d’évaluer la pertinence, la précision et la profondeur de l’information générée automatiquement sur un sujet aussi technique que la simulation numérique d’un trou noir.

Voici le premier prompt envoyé au modèle :

> [!info]
> **Explique, en termes simples, comment on peut simuler numériquement un trou noir en C++ et quelles connaissances sont nécessaires pour comprendre la démarche.**

---

## 2. Réponse du modèle (ChatGPT)

Voici la réponse générée par ChatGPT :

> [!info]
> **Simuler un trou noir numériquement en C++ est un défi très complexe qui nécessite une bonne compréhension de plusieurs domaines de la physique théorique et des méthodes numériques. Voici un aperçu simplifié des étapes et des connaissances nécessaires pour aborder un tel projet.**
>
> **1. Connaissances nécessaires**
>
> **a. Relativité générale**  
> Il faut comprendre la courbure de l’espace-temps, l’horizon des événements, le rayon de Schwarzschild et les équations de champ d’Einstein.
>
> **b. Mathématiques avancées**  
> Équations différentielles, tenseurs, géométrie différentielle, algèbre linéaire (matrices, vecteurs).
>
> **c. Programmation en C++**  
> Maîtrise du langage et de bibliothèques scientifiques comme Eigen, Boost, GSL.
>
> **d. Méthodes numériques**  
> Différences finies, Runge-Kutta, éléments finis, etc., pour approximer les équations physiques.
>
> **2. Étapes de simulation**
>
> **a. Modélisation du trou noir**  
> Choisir une métrique (Schwarzschild ou Kerr) et discrétiser l’espace-temps.
>
> **b. Résolution numérique des équations**  
> Approcher les équations d’Einstein avec des schémas numériques adaptés.
>
> **c. Implémentation en C++**  
> Construire un maillage, calculer des géodésiques et simuler des particules ou rayons lumineux.
>
> **d. Visualisation**  
> Utiliser OpenGL, SFML ou autre bibliothèque pour représenter les trajectoires ou la distorsion.
>
> **3. Exemple simplifié**
> Simuler l’effet du trou noir sur une particule avec la métrique de Schwarzschild, discrétiser le temps, intégrer les équations, et tracer la trajectoire.
>
> **4. Bibliothèques utiles**  
> Boost, Eigen, GSL, OpenGL/GLFW.
>
> **Conclusion**  
> Une simulation de trou noir en C++ requiert une bonne base en relativité, en mathématiques avancées et en méthodes numériques. Un modèle simplifié constitue généralement la meilleure approche de départ.

---

## 3. Analyse critique de la réponse

L’explication ci-dessus donne une bonne idée générale de ce que représente la simulation d’un trou noir en C++. Elle explique les grandes étapes comme la relativité générale, les méthodes numériques et le rôle de la programmation. Par contre, la réponse est assez générale et ne va pas très loin dans les détails mathématiques ou scientifiques, ce qui serait normalement attendu dans une étude plus avancée.
 
Pour avoir des informations plus précises et plus complètes, il serait préférable de faire des recherches supplémentaires sur Google, surtout dans des articles universitaires ou des ressources spécialisées sur la relativité numérique. Ces sources donnent des équations, des schémas, des explications techniques et des exemples concrets qui vont plus loin que cette présentation.

---

## 4. Sources retenues pour les notes de cours

Pour élaborer nos notes de cours, nous avons sélectionné plusieurs sources pédagogiques reconnues, chacune apportant une contribution spécifique aux concepts présentés. L’introduction générale sur les trous noirs s’appuie sur les explications de la NASA , une ressource conçue pour un public étudiant et offrant une présentation claire des notions fondamentales. Pour décrire la courbure de l’espace-temps, nous avons utilisé l’article pédagogique de Sherpas, qui propose une vulgarisation structurée de la relativité générale. Les concepts de géodésiques, d’horizon des événements et de singularité proviennent d’un ensemble de sources complémentaires : les pages techniques de Wikipédia, utilisées pour clarifier les définitions, et l’article de Space.com, qui fournit une explication accessible du rôle de l’horizon des événements dans la physique des trous noirs. L’ensemble de ces sources a été retenu pour leur lisibilité, leur pertinence pédagogique et leur cohérence avec le niveau attendu dans un cours d’introduction à la physique des trous noirs.

---

## 5. Article récent obligatoire

– Lien  
– Résumé  
– Importance  

---

## 6. Sources réseaux sociaux / YouTube

(Liens + explication critique)

---

## 7. Synthèse

(Conclusion générale)

>>>>>>> 55929445ae8de9fcaf6939187a2658a28c655ceb
