# Initiation à Jetpack Compose

Je vais noter ici quelques idées pour aider à initier une personne au développement mobile et à Jetpack Compose. L'idée est de fournir de quoi faire un atelier de mois de 4h tout en mettant à disposition un produit fini que votre destinataire pourra emmener sur son smartphone.

## Idées de sujet
Voici trois idées simples d'applications que votre stagiaire (appelons-le comme ça) pourrait développer en une après-midi en utilisant Jetpack Compose pour le développement Android :

1. **Application de tâches à faire (To-Do List)** : Cette application permettra à l'utilisateur d'ajouter et de supprimer des tâches à faire. Cela permettra d'apprendre à gérer les entrées utilisateur, à créer une interface utilisateur avec Jetpack Compose et à manipuler une simple liste de données en mémoire.

2. **Application de chronomètre** : Le stagiaire peut créer une application de chronomètre avec des boutons pour démarrer, arrêter et réinitialiser le temps. C'est un bon moyen d'apprendre à gérer l'état de l'application, les événements d'interaction de l'utilisateur et le concept de mise à jour de l'interface utilisateur en temps réel.

3. **Jeu de Memory** : Un jeu simple de cartes à retourner, où l'utilisateur doit retrouver les paires d'images identiques. Cela permettrait d'apprendre à manipuler une grille de données, à gérer l'état du jeu, et à créer une expérience utilisateur interactive. Ce jeu pourrait être simplifié selon le temps disponible (par exemple, utiliser un petit nombre de cartes).

Chacune de ces idées donnerait une compréhension de base de l'architecture d'une application Android, de la création d'interfaces utilisateur avec Jetpack Compose, de la gestion de l'état et de la manipulation de données utilisateur.

## Avant tout, introduire Jetpack Compose
Jetpack Compose est un framework de création d'interfaces utilisateur pour Android, introduit par Google lors de la conférence Google I/O 2019. Il a été officiellement lancé en juillet 2021. Jetpack Compose a pour objectif de simplifier et d'accélérer le développement d'interfaces utilisateur sur Android en utilisant un paradigme déclaratif, ce qui est une rupture par rapport à l'approche impérative traditionnelle utilisée par le système de vues d'Android.

### Fonctionnement

Jetpack Compose est basé sur le concept de fonctions composables. Une fonction composable est une fonction spéciale qui peut créer, gérer et manipuler une interface utilisateur. Vous pouvez penser à chaque fonction composable comme à un bloc de construction de votre interface utilisateur. Les fonctions composables peuvent être imbriquées et combinées pour créer des interfaces utilisateur complexes.

Jetpack Compose utilise le compilateur Kotlin pour transformer ces fonctions composables en une interface utilisateur efficace. Lorsque l'état des données change, Jetpack Compose ne met à jour que les parties spécifiques de l'interface utilisateur qui dépendent de ces données, et non toute l'interface utilisateur, ce qui améliore les performances.

### Mentalité et inspirations

La mentalité derrière Jetpack Compose est d'apporter la simplicité et l'expressivité du développement moderne d'interfaces utilisateur à l'écosystème Android. Il est inspiré par d'autres frameworks déclaratifs tels que React pour JavaScript, SwiftUI pour iOS, et Flutter pour le développement mobile multiplateforme.

Le concept déclaratif signifie que vous décrivez l'état de l'interface utilisateur à un moment donné, et le système se charge de l'actualiser en fonction des modifications de l'état des données. Cela contraste avec l'approche impérative, où vous devez décrire explicitement comment l'interface utilisateur doit changer en fonction de différents événements.

### Comparaison avec l'approche traditionnelle Android

Dans l'approche traditionnelle de développement Android, vous définiriez votre interface utilisateur en utilisant XML pour créer une hiérarchie de vues, et vous utiliseriez ensuite Java ou Kotlin pour manipuler ces vues et gérer les interactions de l'utilisateur.

Jetpack Compose simplifie ce processus en vous permettant de créer votre interface utilisateur entièrement en Kotlin, en utilisant un style de programmation plus moderne et expressif. Il offre une intégration plus étroite avec les fonctionnalités de Kotlin, comme les coroutines pour la gestion de la concurrence.

Cela ne signifie pas que vous devriez immédiatement remplacer toutes vos vues Android traditionnelles par Jetpack Compose. Google maintient le support des vues Android et continue de les développer. Cependant, Jetpack Compose est clairement la direction vers laquelle Google se dirige pour le développement d'interfaces utilisateur sur Android à l'avenir.


> **A lire**
> 
> Cet article: [Jetpack Compose: Rendu et Recomposition](https://medium.com/just-tech-it-now/jetpack-compose-9a11a94d01b4)

## Jeu de Memory (je vous laisse le soin de préparer les autres exemples)
Avant de commencer, gardez à l'esprit que l'apprentissage du développement logiciel est un processus itératif. Par conséquent, n'hésitez pas à répéter et à revenir sur les étapes précédentes à mesure que vous avancez.

Voici une ébauche de code de base pour chaque étape à partir de la 3. Notez que le code est simplifié et ne contient pas de gestion d'erreurs ou de complexités supplémentaires pour faciliter la compréhension.

Avant toute chose, parlons de la donnée de base: la carte

```kotlin
data class Card(
    val id: Int,
    val image: Painter,
    var isFaceUp: Boolean = false,
    var isMatched: Boolean = false
)
```

Dans cette classe, nous avons :

- `id` : Un identifiant unique pour chaque carte.
- `image` : L'image à afficher lorsque la carte est face visible.
- `isFaceUp` : Un booléen indiquant si la carte est face visible.
- `isMatched` : Un booléen indiquant si la carte a été mise en correspondance.

`Painter` est une classe de la bibliothèque Jetpack Compose utilisée pour dessiner des images. Vous pouvez créer une instance de `Painter` en utilisant l'une des méthodes de la bibliothèque comme `rememberImagePainter` qui peut charger une image à partir d'une URL ou d'une ressource.

Notez que, pour simplifier, je considère que toutes les images sont déjà chargées et disponibles. En réalité, vous auriez besoin de gérer le chargement des images, probablement en utilisant une bibliothèque comme Coil ou Glide.

Enfin, vous auriez besoin de créer une méthode pour initialiser votre jeu, qui pourrait ressembler à quelque chose comme ceci :

```kotlin
fun initializeGame(): List<Card> {
    val images = // chargez vos images ici
    val cards = images.flatMap { image -> listOf(Card(image = image), Card(image = image)) }
    cards.shuffle()
    return cards
}
```

Cette méthode crée deux cartes pour chaque image (car chaque paire doit avoir deux cartes identiques), les mélange, puis renvoie la liste de cartes.


1. **Création de l'interface utilisateur (UI) du jeu** :

    **Création du plateau de jeu** : Commencez par créer une grille qui représente le plateau du jeu. Vous pouvez utiliser le composant LazyVerticalGrid de Jetpack Compose pour cela.

    ```kotlin
    @Composable
    fun GameBoard(cards: List<Card>, onCardClick: (Card) -> Unit) {
        val rows = cards.chunked(4)  // Modifiez cela en fonction de la taille de votre grille
        Column {
            for (row in rows) {
                Row {
                    for (card in row) {
                        GameCard(card, onCardClick)
                    }
                }
            }
        }
    }
    ```

    **Création d'une carte de jeu** : Créez un composant pour représenter une carte du jeu. Cette carte doit avoir deux états (cachée/visible) et afficher une image lorsqu'elle est retournée.

    ```kotlin
    @Composable
    fun GameCard(card: Card, onCardClick: (Card) -> Unit) {
        Box(modifier = Modifier.clickable { onCardClick(card) }) {
            if (card.isFaceUp) {
                Image(card.image)
            } else {
                // Vous pouvez afficher une image par défaut ou laisser vide
            }
        }
    }
    ```

2. **Gestion de l'état du jeu** : Définissez un modèle pour représenter l'état du jeu. Cela pourrait être une liste de cartes, chaque carte ayant un identifiant unique, une image et un état (cachée/visible). Utilisez les méthodes remember et mutableStateOf pour suivre l'état des cartes.

    ```kotlin
    @Composable
    fun GameViewModel() {
        var cards by remember { mutableStateOf(initializeGame()) }
        var flippedCards by remember { mutableStateOf(listOf<Card>()) }

        fun onCardClick(card: Card) {
            // À compléter en fonction de la logique du jeu
        }

        GameBoard(cards, onCardClick)
    }
    ```

3. **Gestion des interactions de l'utilisateur** : Ajoutez des interactions à chaque carte pour qu'elle puisse être retournée lorsque l'utilisateur clique dessus. Vous devez également vérifier si deux cartes ont été retournées et si elles correspondent. Si elles correspondent, elles restent visibles. Si elles ne correspondent pas, elles sont retournées face cachée après un délai.

    ```kotlin
    fun onCardClick(card: Card) {
        if (flippedCards.size < 2 && card !in flippedCards) {
            card.isFaceUp = !card.isFaceUp
            flippedCards = if (card.isFaceUp) {
                flippedCards + listOf(card)
            } else {
                flippedCards.filter { it.id != card.id }
            }

            if (flippedCards.size == 2) {
                if (flippedCards[0].image == flippedCards[1].image) {
                    flippedCards[0].isMatched = true
                    flippedCards[1].isMatched = true
                } else {
                    // Pseudo-code : attendez un moment avant de cacher à nouveau les cartes
                    delay(time = 1.second) {
                        flippedCards[0].isFaceUp = false
                        flippedCards[1].isFaceUp = false
                    }
                }
                flippedCards = listOf()
            }
        }
    }
    ```

4. **Finalisation du jeu** : Pour terminer, ajoutez un message de victoire lorsque toutes les paires ont été trouvées. Vous pouvez également ajouter un bouton pour réinitialiser le jeu et recommencer.

    ```kotlin
    @Composable
    fun GameViewModel() {
        var cards by remember { mutableStateOf(initializeGame()) }
        var flippedCards by remember { mutableStateOf(listOf<Card>()) }

        fun onCardClick(card: Card) {
            // Code de la fonction onCardClick
        }

        GameBoard(cards, onCardClick)

        if (cards.all { it.isMatched }) {
            Text("Félicitations, vous avez gagné !")
        }
    }
    ```

Le passage d'une étape à l'autre se fait principalement par la discussion et l'explication. Par exemple, après avoir créé l'UI, vous pouvez expliquer que pour que l'UI soit interactive, elle doit être capable de réagir à l'état du jeu, d'où la nécessité de gérer l'état. Une fois l'état géré, vous pouvez introduire les interactions de l'utilisateur pour modifier cet état. Enfin, vous pouvez introduire l'idée d'une fin de jeu pour donner un but à l'interaction de l'utilisateur.

Ce sont des concepts fondamentaux du développement de logiciels qui sont utilisés dans de nombreuses applications, pas seulement dans le développement Android ou avec Jetpack Compose, donc cela vaut la peine de prendre le temps de les expliquer correctement.

## Test sur un smartphone

Cette étape consiste à installer et à tester l'application sur le smartphone du stagiaire. Voici un guide pas à pas sur la façon de procéder.

**Activer le mode développeur sur le smartphone**

1. Ouvrez l'application "Paramètres" sur le téléphone Android.
2. Faites défiler vers le bas et sélectionnez "À propos du téléphone".
3. Faites défiler vers le bas jusqu'à l'option "Numéro de build" ou "Version du logiciel" (selon la version d'Android et le fabricant du téléphone, cela peut être dans un sous-menu supplémentaire comme "Informations sur le logiciel").
4. Appuyez sept fois sur "Numéro de build" ou "Version du logiciel". Un message apparaîtra indiquant que vous êtes maintenant un développeur.

**Activer le débogage USB sur le smartphone**

1. Retournez à l'écran principal des paramètres.
2. Faites défiler vers le bas et sélectionnez "Options pour les développeurs".
3. Activez "Débogage USB".

**Connecter le téléphone à l'ordinateur**

1. Connectez le smartphone à l'ordinateur avec un câble USB.
2. Une invite devrait apparaître sur le téléphone demandant l'autorisation de déboguer USB. Cochez la case "Toujours autoriser à partir de cet ordinateur" et appuyez sur "OK".

**Exécuter l'application sur le téléphone à partir d'Android Studio**

1. Dans Android Studio, cliquez sur le bouton "Run" dans la barre d'outils (ou appuyez sur Maj+F10).
2. Dans la fenêtre "Select Deployment Target", choisissez le téléphone connecté et cliquez sur "OK".
3. Android Studio installera l'application sur le téléphone et l'exécutera.

Si tout s'est bien passé, le stagiaire devrait voir l'application s'exécuter sur son téléphone.

N'oubliez pas de rappeler au stagiaire que le mode développeur et le débogage USB sont des fonctionnalités puissantes qui doivent être utilisées avec précaution. Ils doivent s'assurer de les désactiver lorsqu'ils n'en ont pas besoin, en particulier le débogage USB, qui peut présenter des risques de sécurité s'il est laissé activé.

## Publier son code sur Github

Bien sûr, voici un guide étape par étape pour créer un compte GitHub, initialiser un dépôt, y publier votre code et comprendre les bases des branches et des commits.

**Créer un compte GitHub**

1. Rendez-vous sur le site web de GitHub (https://github.com/).
2. Cliquez sur le bouton "Sign up" en haut à droite de la page.
3. Remplissez les détails requis : nom d'utilisateur, adresse e-mail et mot de passe. Suivez les instructions pour vérifier votre compte et acceptez les conditions d'utilisation.
4. Une fois votre compte créé, connectez-vous à GitHub.

**Créer un nouveau dépôt**

1. Une fois connecté, cliquez sur le bouton "+" dans le coin supérieur droit et sélectionnez "New repository".
2. Donnez un nom à votre dépôt, par exemple "memory-game". Vous pouvez également ajouter une description si vous le souhaitez.
3. Choisissez si vous voulez que votre dépôt soit public ou privé.
4. Cliquez sur "Create repository".

**Publier votre code sur GitHub**

1. Sur votre machine locale, naviguez jusqu'au répertoire du projet Android dans le terminal.
2. Initialisez un nouveau dépôt git avec la commande : `git init`.
3. Ajoutez tous les fichiers du projet au dépôt avec la commande : `git add .`.
4. Faites un premier commit avec la commande : `git commit -m "Initial commit"`.
5. Liez votre dépôt local au dépôt GitHub avec la commande : `git remote add origin https://github.com/<your-username>/memory-game.git` (remplacez `<your-username>` par votre nom d'utilisateur GitHub).
6. Envoyez votre code sur GitHub avec la commande : `git push -u origin master`.

**Comprendre les branches et les commits**

Dans git, une branche est essentiellement une unique version de votre code. La branche par défaut est la branche "master" (ou "main" dans les nouveaux dépôts).

Un commit est une "instantané" de votre code à un moment donné. Chaque fois que vous faites un changement que vous voulez enregistrer, vous "committez" ce changement. Cela vous permet de suivre les modifications de votre code au fil du temps et de revenir à un état précédent si nécessaire.

L'idée de base du workflow git est que vous créez une nouvelle branche pour chaque nouvelle fonctionnalité ou correction de bug sur laquelle vous travaillez. Une fois que vous avez terminé votre travail sur cette branche, vous pouvez la fusionner dans la branche master.


``` mermaid
---
title: Example git-flow
---
gitGraph
   commit tag: "v1.0"
   branch develop
   checkout develop
   commit
   branch feature/one
   checkout feature/one
   commit
   commit
   checkout develop
   merge feature/one
   branch feature/two
   checkout feature/two
   commit
   checkout develop
   merge feature/two
   checkout main
   merge develop tag: "v1.1"
```

Ce diagramme Mermaid illustre un exemple de flux de travail Git appelé git-flow. Voici comment il se décompose :

- `commit tag: "v1.0"` : un commit est fait sur la branche principale (main), qui est marqué avec le tag "v1.0". C'est probablement la version actuellement en production.
- `branch develop` : une nouvelle branche "develop" est créée. C'est généralement l'endroit où toutes les nouvelles fonctionnalités sont intégrées avant d'être fusionnées sur "main" pour une nouvelle release.
- `checkout develop` : on se positionne sur la branche "develop".
- `commit` : un commit est fait sur la branche "develop". Cela peut être une préparation pour les nouvelles fonctionnalités à venir.
- `branch feature/one` : on crée une nouvelle branche "feature/one" pour la première nouvelle fonctionnalité.
- `checkout feature/one` : on se positionne sur la branche "feature/one".
- `commit` : on fait deux commits sur "feature/one". Cela représente le développement de la première nouvelle fonctionnalité.
- `checkout develop` et `merge feature/one` : on retourne sur "develop" et on y fusionne la branche "feature/one". La première fonctionnalité est donc terminée et intégrée à la branche de développement.
- On répète un processus similaire pour la seconde nouvelle fonctionnalité sur une branche "feature/two".
- `checkout main` et `merge develop tag: "v1.1"` : une fois que toutes les nouvelles fonctionnalités sont terminées et intégrées sur "develop", on retourne sur la branche "main" et on y fusionne "develop". C'est la création de la nouvelle version, marquée avec le tag "v1.1".

Ainsi, chaque fonctionnalité est développée sur sa propre branche, ce qui permet de travailler sur plusieurs fonctionnalités en parallèle sans qu'elles ne se mélangent. Une fois qu'une fonctionnalité est terminée, elle est fusionnée dans "develop". Lorsque toutes les fonctionnalités prévues pour la prochaine version sont prêtes, "develop" est fusionnée dans "main" et une nouvelle version est publiée. 

Cela correspond à ce que j'ai expliqué précédemment : la branche "main" (ou "master") contient le code actuellement en production, et on crée une nouvelle branche pour chaque nouvelle fonctionnalité ou correction de bug. Une fois que le travail sur cette branche est terminé, on la fusionne dans la branche principale.


N'oubliez pas de rappeler au stagiaire que les dépôts publics sont visibles par tout le monde sur internet, il doit donc s'assurer de ne pas y publier de données sensibles.

Rappelez-vous de bien expliquer chaque étape et d'encourager le stagiaire à poser des questions pour clarifier ses doutes. L'objectif n'est pas seulement de créer un jeu, mais aussi de comprendre les concepts de base du développement Android avec Jetpack Compose.