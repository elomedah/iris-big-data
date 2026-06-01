# TP4 - Programmation Orientée Objet Avancée en Java

**Master 1 – Durée : 2 heures**

---

# Objectifs pédagogiques

À l'issue de ce TP, vous serez capable de :

- concevoir des classes Java robustes ;
- manipuler les objets et les références ;
- utiliser correctement le mot-clé `this` ;
- appliquer les principes d'encapsulation ;
- redéfinir `toString()` et `equals()` ;
- protéger l'état interne d'un objet grâce au Defensive Copying ;
- utiliser les Records introduits dans les versions modernes de Java.

---

# Contexte

Vous devez développer un système simplifié de gestion d'un centre de formation international.

Le centre gère :

- des apprenants ;
- des formateurs ;
- des formations ;
- des adresses ;
- des inscriptions.

Toutes les classes devront respecter les bonnes pratiques de conception objet.

---

# Partie 1 – Classes, attributs et constructeurs

Créer une classe `Learner`.

## Attributs

- id
- firstName
- lastName
- age
- email

## Travail demandé

1. Définir les attributs.
2. Choisir une visibilité adaptée.
3. Créer un constructeur complet.
4. Créer une méthode permettant d'afficher les informations d'un apprenant.

## Questions

1. Pourquoi les attributs ne devraient-ils pas être publics ?
2. Quels avantages apporte l'utilisation d'un constructeur ?

---

# Partie 2 – Objets, références et cycle de vie

Créer le programme suivant.

```java
Learner l1 = new Learner(...);

Learner l2 = l1;

l2.setAge(35);
```

## Questions

1. Quel est l'âge de l'objet référencé par `l1` ?
2. Pourquoi ?
3. Quelle différence existe-t-il entre un objet et une référence ?
4. Que devient un objet lorsqu'aucune référence ne pointe vers lui ?

---

# Partie 3 – Le mot-clé this

Modifier le constructeur précédent.

## Exemple

```java
public Learner(String firstName, String lastName) {

    this.firstName = firstName;
    this.lastName = lastName;

}
```

## Questions

1. Quel est le rôle du mot-clé `this` ?
2. Pourquoi est-il utile lorsque les paramètres portent le même nom que les attributs ?

---

# Partie 4 – Encapsulation et contrôle d'accès

Modifier la classe `Learner`.

## Travail demandé

- rendre tous les attributs privés ;
- créer les getters ;
- créer les setters ;
- empêcher les âges négatifs ;
- empêcher les emails vides.

## Questions

1. Pourquoi l'encapsulation est-elle importante ?
2. Quels risques apparaissent lorsqu'un attribut est directement accessible ?

---

# Partie 5 – Une implémentation pertinente de toString()

Créer une représentation textuelle de l'objet.

## Exemple attendu

```text
Learner[id=1, firstName=Eyram, lastName=Edah, age=25]
```

## Travail demandé

Implémenter la méthode `toString()`.

## Questions

1. Pourquoi cette méthode est-elle utile ?
2. Que retourne la version héritée de la classe Object ?

---

# Partie 6 – Redéfinition de equals()

Deux apprenants sont considérés identiques lorsqu'ils possèdent le même identifiant.

## Travail demandé

Redéfinir la méthode `equals()`.

Tester ensuite les cas suivants.

```java
Learner l1 = ...
Learner l2 = ...

System.out.println(l1 == l2);
System.out.println(l1.equals(l2));
```

## Questions

1. Quelle différence existe entre `==` et `equals()` ?
2. Dans quels cas les deux méthodes peuvent-elles retourner le même résultat ?

---

# Partie 7 – Gestion d'une formation

Créer une classe `Course`.

## Attributs

- code
- title
- duration
- learners

## Travail demandé

Implémenter les fonctionnalités suivantes :

- ajouter un apprenant ;
- supprimer un apprenant ;
- rechercher un apprenant ;
- afficher la liste des apprenants.

## Contraintes

- éviter les doublons ;
- vérifier les valeurs invalides.

---

# Partie 8 – Encapsulation complète

Ajouter un getter pour récupérer la liste des apprenants.

## Première version

```java
public List<Learner> getLearners() {

    return learners;

}
```

## Questions

1. Quel problème pose cette implémentation ?
2. Un utilisateur externe peut-il modifier la liste interne ?
3. Pourquoi cela constitue-t-il une violation de l'encapsulation ?

---

# Partie 9 – Defensive Copying

Corriger le problème précédent.

## Travail demandé

Modifier :

- le constructeur ;
- les getters.

L'objectif est de protéger l'état interne de l'objet.

## Questions

1. Pourquoi le code suivant est-il dangereux ?

```java
this.learners = learners;
```

2. Dans quels contextes le Defensive Copying est-il indispensable ?

---

# Partie 10 – Classe Trainer

Créer une classe `Trainer`.

## Attributs

- id
- name
- speciality

## Travail demandé

Implémenter :

- constructeur ;
- getters ;
- setters ;
- toString() ;
- equals().

---

# Partie 11 – Records Java

Créer un record `Address`.

## Attributs

- street
- city
- country
- postalCode

## Exemple

```java
public record Address(
    String street,
    String city,
    String country,
    String postalCode
) {}
```

## Questions

1. Quelles méthodes sont générées automatiquement ?
2. Pourquoi les records sont-ils immutables ?
3. Dans quels cas préfère-t-on un record à une classe classique ?

---

# Partie 12 – Analyse UML

Dessiner le diagramme UML du système.

Classes :

- Learner
- Trainer
- Course
- Address

Identifier :

- attributs ;
- méthodes ;
- relations ;
- multiplicités.

---

# Projet Final

Créer une classe `TrainingCenter`.

## Attributs

```java
private String name;

private List<Course> courses;

private List<Learner> learners;

private List<Trainer> trainers;
```

## Fonctionnalités

### Gestion des apprenants

- ajout ;
- suppression ;
- recherche ;
- modification.

### Gestion des formateurs

- ajout ;
- suppression ;
- recherche.

### Gestion des formations

- création ;
- affectation d'un formateur ;
- inscription d'un apprenant ;
- désinscription d'un apprenant.

### Affichage

Afficher un résumé complet du centre.

---

# Questions de réflexion (niveau Master)

1. Pourquoi la méthode `equals()` devrait-elle généralement être accompagnée de `hashCode()` ?

2. Quels problèmes de conception peuvent apparaître lorsque des collections internes sont exposées ?

3. Dans quels cas utiliseriez-vous un Record plutôt qu'une classe classique ?

4. Quels principes SOLID sont visibles dans votre conception ?

5. Quels avantages apporte l'encapsulation pour la maintenance d'une application ?

---

# Bonus

Ajouter une méthode permettant de calculer :

- le nombre total d'apprenants ;
- le nombre total de formations ;
- le nombre moyen d'apprenants par formation.

Proposer ensuite une amélioration possible de l'architecture du système.
