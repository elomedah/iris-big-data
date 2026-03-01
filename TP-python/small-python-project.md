# Projet Python : Système de Gestion d'une Bibliothèque

## Objectif du projet

Développer une application en Python permettant de gérer une bibliothèque.

Ce projet permet de pratiquer :

- Les variables
- Les structures de données (listes, dictionnaires, tuples)
- Les structures de contrôle (if, for, while)
- Les fonctions
- La Programmation Orientée Objet (POO)

##  Utilisation obligatoire du gestionnaire de version (Git)

Ce projet doit être réalisé en utilisant un gestionnaire de version :

- GitHub
- GitLab
- Bitbucket

### Règles obligatoires

1. Le projet doit être hébergé sur un dépôt distant.
2. Les étudiants doivent effectuer des **commits réguliers** tout au long du développement.
3. Chaque commit doit avoir un message clair et explicite.
4. Le dépôt ne doit pas contenir un seul commit final.


## Fonctionnalités attendues

### Partie 1 : Version simple (sans POO)

- Ajouter un livre
- Afficher tous les livres
- Rechercher un livre
- Supprimer un livre
- Menu interactif

### Partie 2 : Version orientée objet (POO)

Créer les classes suivantes :

#### Classe `Livre`
- id
- titre
- auteur
- disponible
- méthode `emprunter()`
- méthode `retourner()`

#### Classe `Utilisateur`
- nom
- liste des livres empruntés
- emprunter un livre
- retourner un livre

#### Classe `Bibliotheque`
- liste des livres
- ajouter un livre
- supprimer un livre
- rechercher un livre
- afficher les livres

Vous allez créer un système de gestion d’une bibliothèque qui contient **plusieurs types de livres** :

- Livre classique
- Livre audio
- Bande dessinée
- Ebook
