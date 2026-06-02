# TP5 - Héritage et Classes Abstraites en Java

**Master 1 – Durée : 1 heure**

---

# Objectifs pédagogiques

À l'issue de ce TP, vous serez capable de :

- Comprendre le principe d'héritage en Java.
- Utiliser le mot-clé `extends`.
- Appeler un constructeur parent avec `super()`.
- Redéfinir des méthodes avec `@Override`.
- Concevoir une hiérarchie de classes cohérente.
- Utiliser une classe abstraite.
- Préparer l'utilisation du polymorphisme.

---

# Contexte

Une université souhaite développer un système de gestion de son personnel.

Plusieurs catégories de personnels existent :

- enseignants ;
- administrateurs ;
- chercheurs.

Tous possèdent des caractéristiques communes :

- identifiant ;
- nom ;
- salaire.

Cependant chaque catégorie possède également des informations spécifiques.

L'objectif de ce TP est de concevoir une hiérarchie de classes permettant de représenter ces différents types de personnels.

---

# Partie 1 – Analyse du problème

Avant d'écrire du code, répondre aux questions suivantes.

## Questions

1. Quels attributs sont communs à tous les personnels ?
2. Quels attributs sont spécifiques aux enseignants ?
3. Quels attributs sont spécifiques aux administrateurs ?
4. Quels attributs sont spécifiques aux chercheurs ?
5. Quelle classe pourrait représenter les caractéristiques communes ?

---

# Partie 2 – Création de la classe mère Employee

Créer une classe `Employee`.

## Attributs

```java
private int id;
private String name;
private double salary;
```

## Travail demandé

1. Créer un constructeur complet.
2. Créer les getters et setters.
3. Créer la méthode :

```java
public void displayInfo()
```

4. Tester la classe dans une classe Main.

## Questions

1. Pourquoi les attributs doivent-ils être privés ?
2. Quels avantages apporte la réutilisation d'une classe mère ?

---

# Partie 3 – Première sous-classe : Teacher

Créer une classe `Teacher` héritant de `Employee`.

## Attribut supplémentaire

```java
private String subject;
```

## Travail demandé

1. Utiliser le mot-clé `extends`.
2. Créer un constructeur complet.
3. Utiliser `super()` pour appeler le constructeur parent.

## Exemple

```java
Teacher teacher =
    new Teacher(
        1,
        "Eyram",
        1800,
        "English"
    );
```

## Questions

1. Quel est le rôle de `super()` ?
2. Que se passe-t-il si le constructeur parent n'est pas correctement appelé ?

---

# Partie 4 – Redéfinition de méthode

Modifier la classe Teacher.

## Travail demandé

Redéfinir la méthode :

```java
displayInfo()
```

afin d'afficher également la matière enseignée.

Utiliser l'annotation :

```java
@Override
```

## Exemple attendu

```text
Teacher
Id : 1
Name : Eyram
Salary : 1800
Subject : English
```

## Questions

1. Pourquoi utiliser @Override ?
2. Quels problèmes cette annotation permet-elle d'éviter ?

---

# Partie 5 – Deuxième sous-classe : Administrator

Créer une classe `Administrator`.

## Attribut supplémentaire

```java
private String department;
```

## Travail demandé

1. Hériter de Employee.
2. Créer le constructeur.
3. Utiliser super().
4. Redéfinir displayInfo().

## Questions

1. Quels éléments sont hérités ?
2. Quels éléments sont spécifiques ?

---

# Partie 6 – Troisième sous-classe : Researcher

Créer une classe `Researcher`.

## Attribut supplémentaire

```java
private int publications;
```

## Travail demandé

1. Hériter de Employee.
2. Créer le constructeur.
3. Redéfinir displayInfo().

## Questions

1. Pourquoi un chercheur est-il également un Employee ?
2. Quels avantages apporte l'héritage dans ce cas ?

---

# Partie 7 – Classe abstraite

Transformer Employee en classe abstraite.

## Travail demandé

Ajouter la méthode abstraite suivante :

```java
public abstract double calculateBonus();
```

## Questions

1. Peut-on instancier directement une classe abstraite ?
2. Quel problème résout une classe abstraite ?

---

# Partie 8 – Implémentation des bonus

Implémenter calculateBonus() dans chaque sous-classe.

## Règles

### Teacher

```text
10 % du salaire
```

### Administrator

```text
15 % du salaire
```

### Researcher

```text
5 % du salaire + 100 € par publication
```

## Travail demandé

Afficher le bonus calculé pour plusieurs employés.

---

# Partie 9 – Vérification de la hiérarchie

Créer les objets suivants.

```java
Teacher teacher = ...
Administrator admin = ...
Researcher researcher = ...
```

Afficher :

```java
displayInfo();
calculateBonus();
```

## Questions

1. Quelles méthodes sont héritées ?
2. Quelles méthodes sont redéfinies ?
3. Pourquoi chaque classe possède-t-elle son propre calcul de bonus ?

---

# Partie 10 – Diagramme UML

Dessiner le diagramme UML correspondant.

## Classes

- Employee
- Teacher
- Administrator
- Researcher

Le diagramme doit faire apparaître :

- attributs ;
- méthodes ;
- héritage ;
- méthode abstraite.

---

# Partie 11 – Analyse de conception

Répondre aux questions suivantes.

1. Quels avantages apporte l'héritage ?
2. Quels inconvénients peuvent apparaître lorsque l'on crée trop de niveaux d'héritage ?
3. Pourquoi dit-on que l'héritage représente une relation "est-un" ?
4. Dans quels cas faudrait-il préférer la composition ?

---

# Projet Final

Développer une application permettant de gérer le personnel d'une université.

## Fonctionnalités

### Gestion des employés

- ajouter ;
- supprimer ;
- rechercher.

### Gestion des enseignants

- affecter une matière.

### Gestion des administrateurs

- affecter un département.

### Gestion des chercheurs

- gérer le nombre de publications.

### Affichage

Afficher tous les employés avec :

- leurs informations ;
- leur catégorie ;
- leur bonus.

---

# Questions de réflexion 

1. Pourquoi Java interdit-il l'héritage multiple des classes ?

2. Quelle différence existe entre :

```java
extends
```

et

```java
implements
```

3. Quelle différence existe entre une classe abstraite et une interface ?

4. Quels concepts du polymorphisme pourraient être appliqués à cette hiérarchie ?

5. Dans une application réelle, quelles améliorations proposeriez-vous ?

---
