# JavaFXGalactic

### Introduction

Dans ce tutoriel, nous allons créer un jeu simple en JavaFX où une fusée peut se déplacer à gauche et à droite, et tirer des projectiles lorsqu'on appuie sur la barre d'espace. Ce projet est parfait pour les débutants qui viennent de terminer un module sur la programmation orientée objet. Nous allons aborder chaque étape en détail.

![image](https://github.com/ramalhom/JavaFXGalactic/assets/3630367/15a9d10f-1c13-4c1c-97cc-ce8cc009bc58)


### Étape 1: Structurer le Projet

1. **Créer la structure de base**:
   - Nous allons créer les classes suivantes:
     - `Rocket`: Une classe représentant la fusée.
     - `Projectile`: Une classe pour les projectiles.

```plaintext
src
└── sample
    ├── Main.java
    ├── Rocket.java
    └── Projectile.java
```

### Étape 2: Créer la Classe `Rocket`

2. **Définir la fusée et ses mouvements**:

```java
package sample;

import javafx.scene.paint.Color;
import javafx.scene.shape.Rectangle;

public class Rocket extends Rectangle {
    public Rocket() {
        super(50, 50, Color.BLUE);  // Width: 50, Height: 50, Color: Blue
        setX(375);  // Centered horizontally
        setY(500);  // Positioned near the bottom
    }

    public void moveLeft() {
        if (getX() > 0) {
            setX(getX() - 10);
        }
    }

    public void moveRight() {
        if (getX() < 750) {
            setX(getX() + 10);
        }
    }
}
```

### Étape 3: Créer la Classe `Projectile`

3. **Définir les projectiles**:

```java
package sample;

import javafx.scene.paint.Color;
import javafx.scene.shape.Rectangle;

public class Projectile extends Rectangle {
    public Projectile(double x, double y) {
        super(5, 20, Color.RED);  // Width: 5, Height: 20, Color: Red
        setX(x);
        setY(y);
    }

    public void moveUp() {
        setY(getY() - 10);
    }
}
```

### Étape 4: Intégrer la Fusée et les Projectiles dans la Classe `Main`

4. **Ajouter la fusée et gérer les interactions clavier**:

```java
package sample;

import javafx.animation.AnimationTimer;
import javafx.scene.Scene;
import javafx.scene.layout.Pane;
import javafx.stage.Stage;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class Main extends Application {

    private Rocket rocket;
    private List<Projectile> projectiles = new ArrayList<>();

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        Pane root = new Pane();
        Scene scene = new Scene(root, 800, 600);

        rocket = new Rocket();
        root.getChildren().add(rocket);

        scene.setOnKeyPressed(event -> {
            switch (event.getCode()) {
                case LEFT -> rocket.moveLeft();
                case RIGHT -> rocket.moveRight();
                case SPACE -> shoot(root);
            }
        });

        primaryStage.setTitle("Rocket Game");
        primaryStage.setScene(scene);
        primaryStage.show();

        AnimationTimer timer = new AnimationTimer() {
            @Override
            public void handle(long now) {
                updateProjectiles(root);
            }
        };
        timer.start();
    }

    private void shoot(Pane root) {
        Projectile projectile = new Projectile(rocket.getX() + 22.5, rocket.getY());
        projectiles.add(projectile);
        root.getChildren().add(projectile);
    }

    private void updateProjectiles(Pane root) {
        Iterator<Projectile> iterator = projectiles.iterator();
        while (iterator.hasNext()) {
            Projectile projectile = iterator.next();
            projectile.moveUp();
            if (projectile.getY() < 0) {
                iterator.remove();
                root.getChildren().remove(projectile);
            }
        }
    }
}
```

### Étape 5: Exécuter le Programme

5. **Lancer le jeu**:
   - Exécutez la classe `Main`.
   - Utilisez les touches fléchées gauche et droite pour déplacer la fusée.
   - Appuyez sur la barre d'espace pour tirer des projectiles.

Vous avez maintenant un jeu simple en JavaFX où une fusée peut se déplacer et tirer des projectiles. Ce projet peut être amélioré en ajoutant des ennemis, des collisions, et des effets sonores. N'hésitez pas à expérimenter et à ajouter vos propres fonctionnalités !

### Étape 6: Challenge pour les Étudiants

Pour ajouter un défi et encourager les étudiants à améliorer leur projet, nous allons leur proposer quelques améliorations et fonctionnalités supplémentaires qu'ils peuvent implémenter. Voici les défis et quelques indications pour les aider sans leur donner directement la solution complète.

#### Challenge 1: Ajouter des Ennemis

**Objectif**: Créer des ennemis qui se déplacent de haut en bas et peuvent être détruits par les projectiles.

1. **Créer une classe `Enemy`**:
   - Cette classe doit représenter un ennemi avec une taille spécifique et une méthode pour le déplacement vertical.

**Indication**: Utilisez une forme géométrique pour représenter l'ennemi (par exemple, `Rectangle`) et une couleur distinctive.

2. **Générer des ennemis dans la classe `Main`**:
   - Ajoutez une méthode pour créer plusieurs ennemis à des positions aléatoires ou fixes en haut de la fenêtre.

**Indication**: Utilisez une liste pour stocker les ennemis et les ajouter au `Pane` principal.

3. **Déplacer les ennemis et gérer les collisions**:
   - Implémentez une méthode pour mettre à jour la position des ennemis à chaque frame et vérifiez les collisions avec les projectiles.

**Indication**: Utilisez `AnimationTimer` pour animer les ennemis et vérifiez les intersections entre les `Bounds` des projectiles et des ennemis.

#### Challenge 2: Ajouter un Système de Points

**Objectif**: Mettre en place un système de score qui augmente lorsque les ennemis sont détruits.

1. **Ajouter une variable de score**:
   - Déclarez une variable pour le score et affichez-la dans l'interface.

**Indication**: Utilisez un `Label` pour afficher le score et mettez-le à jour à chaque fois qu'un ennemi est détruit.

2. **Mettre à jour le score lors des collisions**:
   - Incrémentez le score chaque fois qu'un projectile touche un ennemi.

**Indication**: Ajoutez du code dans la méthode qui vérifie les collisions pour mettre à jour et afficher le nouveau score.

#### Challenge 3: Ajouter des Vies à la Fusée

**Objectif**: Ajouter un système de vies pour la fusée et terminer le jeu lorsque les vies sont épuisées.

1. **Ajouter une variable pour les vies**:
   - Déclarez une variable pour le nombre de vies et affichez-la dans l'interface.

**Indication**: Utilisez un `Label` pour afficher les vies restantes et mettez-le à jour lorsqu'un ennemi touche la fusée.

2. **Décrémenter les vies lors des collisions**:
   - Réduisez le nombre de vies lorsqu'un ennemi atteint la fusée et terminez le jeu si les vies tombent à zéro.

**Indication**: Ajoutez du code dans la méthode qui met à jour les ennemis pour vérifier les collisions avec la fusée et ajuster les vies en conséquence. Utilisez une condition pour vérifier si les vies sont à zéro et afficher un message de fin de jeu.

### Ressources Supplémentaires

1. **Documentation JavaFX**: https://openjfx.io/openjfx-docs/ Pour comprendre comment manipuler les éléments de l'interface utilisateur et les animations.
2. **Tutoriels en ligne**: Cherchez des exemples de jeux JavaFX simples pour vous inspirer.
3. **Forums et Communautés**: Posez des questions sur des forums comme Stack Overflow pour obtenir de l'aide sur des problèmes spécifiques.

### Conclusion

Ces défis permettront aux étudiants de renforcer leurs compétences en programmation orientée objet et en JavaFX. Encouragez-les à expérimenter et à apporter leur touche personnelle à chaque amélioration. N'hésitez pas à organiser des sessions de code en groupe pour qu'ils puissent partager leurs idées et solutions.
