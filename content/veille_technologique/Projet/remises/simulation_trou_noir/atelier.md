+++
title = "Atelier "
weight = 4
[params]
  author = "Équipe : Natali P., Nermine G., Goufran A."
+++
# Construire une mini-simulation d’un trou noir


## Objectif d'atelier

L’objectif de cet atelier est d’apprendre à :

- créer un projet C++ dans un environnement reproductible (Docker + Dev Containers),
- compiler et exécuter une simulation 3D simple d’un trou noir,
- analyser le comportement des rayons lumineux dans un espace courbé,
- modifier progressivement le code pour comprendre comment les paramètres influencent la simulation.

À la fin de l'atelier, une version corrigée du projet d'exercices sera fournie.

---

## Scénario pédagogique

Pour cet atelier, on va utiliser un dépôt GitHub contenant la base du projet.  
Le but n'est pas de réécrire toute la simulation, mais de comprendre :

1. comment les rayons sont générés ;  
2. comment la gravité modifie leur direction ;  
3. comment C++ et OpenGL affichent le résultat.

Vous allez donc :

- **forker le projet** pour pouvoir le modifier librement,
- utiliser un **container Docker** pour garantir que tout fonctionne,
- compiler et exécuter la simulation,
- effectuer des exercices guidés.

---

## Étape 1 — Récupération du projet GitHub

Avant tout, vous devez **forker** ce dépôt :

**https://github.com/kavan010/black_hole**

Cela vous permet d’avoir votre propre copie modifiable.

Ensuite, clonez votre fork :

```bash
git clone https://github.com/<votre_nom>/black_hole
cd black_hole
```

---

## Étape 2 — Installation et configuration (Docker + VS Code)

Nous allons utiliser **Dev Containers** afin d’avoir un environnement Linux complet sans rien installer sur votre machine.

### 2.1 Ouvrir le dossier dans un conteneur

Dans VS Code :

1. Appuyez sur **CTRL + SHIFT + P**  
2. Tapez : **Dev Containers: Open Folder in Container**  
3. VS Code va construire l’image Docker automatiquement.

 *Vous pouvez aussi utiliser « Rebuild and Reopen in Container » si le container existe déjà.*

 ![Reopen et rebuild container](/Pictures/reopencontainer.png)

---

##  Dockerfile utilisé

Le projet utilise le Dockerfile suivant :

```dockerfile
FROM ubuntu:22.04

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    libglfw3-dev \
    libglew-dev \
    libglm-dev \
    libgl1-mesa-dev \
    xorg-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy project files
COPY . .

# Create build directory and build
RUN mkdir -p build && cd build && \
    cmake .. && \
    cmake --build .

# Set entry point
ENTRYPOINT ["./build/BlackHole3D"]
```

Ce Dockerfile installe toutes les dépendances nécessaires : CMake, OpenGL, GLFW, GLEW, GLM, Xorg…

---

##  Étape 3 — Compilation du projet

Une fois dans le Dev Container, exécutez les commandes suivantes :

```bash
cd /workspaces/black_hole_docker

mkdir -p build

cmake -B build -S .

cmake --build build

./build/BlackHole3D
```

Si tout fonctionne, la simulation s’ouvre avec une fenêtre graphique :
 ![Black Hole 3D](/Pictures/trounoir3d.png)



---

## Problème avec *vcpkg*

Certaines machines ne possèdent pas **vcpkg** (gestionnaire de packages C++ utilisé par plusieurs projets OpenGL).

Si vous obtenez des erreurs du type :

```
vcpkg not found  
missing GLFW / GLM / GLEW
```

- **Visual Studio Installer** :  téléchargez l'application et rendez-vous dans la page "modify".
- **Cocher les options** : dans les détails d'installation, assurez-vous que "vcpkg package manager" est coché et installez-le.

 ![VS installer](/Pictures/vcpkg.png)

---

## Exercices 

### Exercice 1 — Optimiser la performance de la simulation

La simulation peut devenir lente selon votre machine.  
L'objectif de cet exercice est d'identifier où se trouvent les principaux coûts de calcul et d'appliquer des optimisations simples pour améliorer la fluidité.

#### Tâche  
Modifiez les paramètres de calcul afin de réduire la charge GPU/CPU sans altérer visuellement la simulation.

#### Pistes de modification
- **Réduire la résolution interne de calcul** (`COMPUTE_WIDTH`, `COMPUTE_HEIGHT`)  
- **Augmenter légèrement le pas d'intégration** (`D_LAMBDA`)  
- **Réduire le nombre d'itérations** dans le shader (`steps`)

#### Fichiers à modifier
1. **`black_hole.cpp`** 
2. **`geodesic.comp`** 

#### Exemple de solution

```cpp
// black_hole.cpp (résolution interne réduite)
int COMPUTE_WIDTH  = 100;   // anciennement 200
int COMPUTE_HEIGHT = 75;    // anciennement 150
```

### Exercice 2 — Améliorer l'interactivité de la simulation 2D
> **Exécution**
>
> Toutes les fonctionnalités décrites dans cet exercice doivent être **implémentées dans le fichier `2D_lensing.cpp`**.
>
> Cet exercice concerne la simulation **Black Hole 2D**.  
> Après compilation, vous pouvez exécuter la version 2D avec la commande :
>
> ```bash
> ./build/BlackHole2D
> ```
>
> Assurez-vous que votre projet est correctement configuré et que le binaire `BlackHole2D` est généré dans le dossier `build/`.


#### Objectif
Transformer la simulation statique en une application interactive en ajoutant des fonctions de contrôle (callbacks) et en optimisant les performances.

#### Contexte initial
La simulation de départ est complètement statique :
- Un seul rayon pré-défini
- Aucune interaction possible
- Pas de contrôle de la vue
- Performance non optimisée

#### Tâches à réaliser

##### 1. Implémenter les callbacks de contrôle

**A. Déplacement de la vue avec la souris**  
Dans `mouseCallback()`, complétez la conversion pixels → coordonnées monde :

```cpp
void mouseCallback(GLFWwindow* window, double xpos, double ypos) {
    if (engine.middleMousePressed) {
        double deltaX = xpos - engine.lastMouseX;
        double deltaY = ypos - engine.lastMouseY;
        
        // À compléter : convertir deltaX, deltaY en coordonnées monde
        double worldDeltaX = /* votre formule ici */ ;
        double worldDeltaY = /* votre formule ici */ ;
        
        engine.offsetX -= worldDeltaX;
        engine.offsetY += worldDeltaY;
    }
    engine.lastMouseX = xpos;
    engine.lastMouseY = ypos;
}
```

**B. Zoom avec la molette**  
Dans `scrollCallback()`, implémentez le zoom :

```cpp
void scrollCallback(GLFWwindow* window, double xoffset, double yoffset) {
    float zoomFactor = 1.1f;
    
    if (yoffset > 0) {
        engine.zoom *= zoomFactor;
    } else {
        engine.zoom /= zoomFactor;
    }
    
    // À compléter : ajuster width et height selon le zoom
    engine.width = /* formule avec zoom */ ;
    engine.height = /* formule avec zoom */ ;
}
```

**C. Contrôle des rayons avec le clavier**  
Dans `keyCallback()`, ajoutez ces fonctionnalités :

```cpp
void keyCallback(GLFWwindow* window, int key, int scancode, int action, int mods) {
    if (action == GLFW_PRESS) {
        if (key == GLFW_KEY_SPACE) {
            // Ajouter un rayon unique
            rays.push_back(Ray(vec2(-1e11, 3.276e10), vec2(c, 0.0f)));
            cout << "Rayon ajouté. Total : " << rays.size() << endl;
        }
        
        // À implémenter :
        // 1. Touche 'C' : Effacer tous les rayons
        // 2. Touche 'M' : Ajouter 20 rayons avec différents paramètres d'impact
        // 3. Touche 'R' : Ajouter des rayons en motif circulaire
        // 4. Touche 'V' : Réinitialiser la vue (zoom=1, offset=0)
    }
}
```

##### 2. Optimiser le rendu des rayons

Problème : Dans le code original, chaque rayon appelle `ray.draw(rays)` individuellement, ce qui est inefficace.

**AVANT (inefficace) :**

```cpp
for (auto& ray : rays) {
    ray.step(1.0f, SagA.r_s);
    ray.draw(rays);  // ← Appelé pour chaque rayon !
}
```

**APRÈS (optimisé) - à implémenter :**

```cpp
for (auto& ray : rays) {
    ray.step(1.0f, SagA.r_s);
}
// Dessiner TOUS les rayons une seule fois :
if (!rays.empty()) {
    /* Appelez la fonction draw une seule fois ici */
}
```

##### 3. Ajouter un compteur de FPS

Dans la fonction `main()`, ajoutez ce code pour mesurer les performances :

```cpp
auto lastTime = std::chrono::high_resolution_clock::now();
int frameCount = 0;

while(!glfwWindowShouldClose(engine.window)) {
    // ... simulation existante ...
    
    // À compléter : compteur de FPS
    frameCount++;
    auto currentTime = std::chrono::high_resolution_clock::now();
    
    // Calculez le temps écoulé en secondes
    auto elapsed = /* formule avec currentTime - lastTime */ ;
    
    if (elapsed >= 1.0) {
        cout << "FPS: " << frameCount << " | Rayons: " << rays.size() << endl;
        frameCount = 0;
        lastTime = currentTime;
    }
}
```

##### 4. Configurer les callbacks dans `main()`

Ajoutez cette configuration au début de `main()` :

```cpp
int main() {
    // Configuration des callbacks - à compléter :
    glfwSetCursorPosCallback(engine.window, /* fonction callback souris */);
    glfwSetMouseButtonCallback(engine.window, /* fonction callback bouton souris */);
    glfwSetScrollCallback(engine.window, /* fonction callback molette */);
    glfwSetKeyCallback(engine.window, /* fonction callback clavier */);

    // ... reste du code ...
}
```


---
### Récapitulatif des commandes importantes

| Action | Commande | Description |
|--------|----------|-------------|
| Compilation | `cmake --build build` | Recompile après modifications |
| Version 3D | `./build/BlackHole3D` | Simulation GPU avec shaders |
| Version 2D | `./build/BlackHole2D` | Simulation précise RK4 |
| Nettoyage | `rm -rf build && mkdir build` | Recommencer la compilation |
| Débogage | `cmake -B build -S . -DCMAKE_BUILD_TYPE=Debug` | Compiler avec symboles debug |

## Corrigé — Code final

**https://github.com/n4t4li/black_hole_docker**

---
