# TP2 — Types primitifs, opérateurs, allocation des variables et utilisation de `==`

## Objectifs

À la fin de cette séance, l'étudiant doit être capable de :

- déclarer et initialiser des variables de types primitifs ;
- utiliser les principaux opérateurs arithmétiques, relationnels et logiques ;
- comprendre où et comment les variables sont stockées pendant l'exécution ;
- utiliser correctement l'opérateur `==` avec les types primitifs ;
- résoudre un programme simple en expliquant chaque étape.

---

## 1. Rappels de cours

### 1.1 Types primitifs en Java

Java possède 8 types primitifs :

- `byte`
- `short`
- `int`
- `long`
- `float`
- `double`
- `char`
- `boolean`

Exemple :

```java
byte b = 10;
short s = 200;
int age = 25;
long population = 8000000L;
float price = 19.5f;
double pi = 3.14159;
char grade = 'A';
boolean valid = true;
```

### Remarques
- `long` nécessite souvent le suffixe `L`
- `float` nécessite le suffixe `f`
- `char` utilise des apostrophes simples
- `boolean` ne peut prendre que `true` ou `false`

---

### 1.2 Opérateurs

#### Opérateurs arithmétiques
- `+` addition
- `-` soustraction
- `*` multiplication
- `/` division
- `%` modulo

Exemple :

```java
int a = 10;
int b = 3;

System.out.println(a + b); // 13
System.out.println(a - b); // 7
System.out.println(a * b); // 30
System.out.println(a / b); // 3
System.out.println(a % b); // 1
```

#### Opérateurs relationnels
- `==`
- `!=`
- `>`
- `<`
- `>=`
- `<=`

Exemple :

```java
int x = 5;
int y = 8;

System.out.println(x == y); // false
System.out.println(x < y);  // true
```

#### Opérateurs logiques
- `&&` ET logique
- `||` OU logique
- `!` NON logique

Exemple :

```java
int age = 20;
boolean hasCard = true;

System.out.println(age >= 18 && hasCard); // true
```

---

### 1.3 Allocation des variables en mémoire

Pendant l'exécution :

- les variables locales primitives sont stockées dans la pile d'exécution (stack) ;
- les références d'objets sont aussi stockées dans la pile ;
- les objets eux-mêmes sont stockés dans le tas (heap).

Exemple :

```java
int x = 10;
double y = 3.14;
```

Ici, `x` et `y` sont des variables primitives locales : leurs valeurs sont stockées directement dans la mémoire de la méthode.

Exemple avec référence :

```java
String name = "Java";
```

- la variable `name` contient une référence ;
- l'objet chaîne de caractères est stocké ailleurs en mémoire.

Dans ce TP, l'opérateur `==` sera étudié d'abord avec les types primitifs.

---

### 1.4 Utilisation de `==`

#### Avec les types primitifs
`==` compare les valeurs.

```java
int a = 10;
int b = 10;

System.out.println(a == b); // true
```

#### Avec les références
`==` compare les adresses mémoire, pas le contenu.

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2); // false
```

Pour comparer le contenu des chaînes, on utilise `.equals()` :

```java
System.out.println(s1.equals(s2)); // true
```

---

## 2. Travail préparatoire

Avant de commencer les exercices, écrire et exécuter le programme suivant :

```java
public class TestPrimitifs {
    public static void main(String[] args) {
        int age = 22;
        double moyenne = 14.75;
        char groupe = 'B';
        boolean admis = true;

        System.out.println("Age = " + age);
        System.out.println("Moyenne = " + moyenne);
        System.out.println("Groupe = " + groupe);
        System.out.println("Admis = " + admis);
    }
}
```

### Questions
1. Identifier le type de chaque variable.
2. Indiquer quelles variables sont primitives.
3. Expliquer ce qui est affiché à l'écran.

---

## 3. Exercices

### Exercice 1 — Déclaration et initialisation

Écrire un programme `Exercice1.java` qui déclare les variables suivantes :

- un âge ;
- une taille en mètres ;
- une note ;
- un caractère représentant une section ;
- une variable booléenne indiquant si l'étudiant est boursier.

Afficher toutes les variables avec des messages explicites.

### Exemple d'affichage attendu

```text
Age : 21
Taille : 1.78
Note : 15.5
Section : A
Boursier : false
```

---

### Exercice 2 — Calculs avec opérateurs

Écrire un programme `Exercice2.java` qui :

1. déclare deux entiers `a` et `b` ;
2. calcule :
   - leur somme ;
   - leur différence ;
   - leur produit ;
   - leur quotient entier ;
   - leur reste de division ;
3. affiche les résultats.

### Exemple

```java
int a = 17;
int b = 5;
```

### Affichage attendu

```text
Somme = 22
Difference = 12
Produit = 85
Quotient = 3
Reste = 2
```

### Question
Pourquoi le quotient vaut-il `3` et non `3.4` ?

---

### Exercice 3 — Comparaisons avec `==`

Écrire un programme `Exercice3.java` qui compare plusieurs variables primitives avec `==`.

### Données de départ

```java
int x = 12;
int y = 12;
double z = 12.0;
char c1 = 'A';
char c2 = 'B';
boolean test1 = true;
boolean test2 = false;
```

### Travail demandé

Afficher le résultat des comparaisons suivantes :

- `x == y`
- `x == z`
- `c1 == c2`
- `test1 == test2`

### Questions
1. Pourquoi `x == y` est-il vrai ?
2. Pourquoi `c1 == c2` est-il faux ?
3. Que compare exactement `==` pour les types primitifs ?

---

### Exercice 4 — Allocation des variables

Considérer le programme suivant :

```java
public class Exercice4 {
    public static void main(String[] args) {
        int age = 20;
        double note = 13.5;
        boolean success = true;
        String nom = "Alice";
    }
}
```

### Travail demandé

Pour chaque variable :

1. indiquer son type ;
2. préciser si c'est un type primitif ou une référence ;
3. expliquer où sa valeur est stockée pendant l'exécution.

### Tableau à compléter

| Variable | Type     | Primitif / Référence | Zone mémoire |
|----------|----------|----------------------|--------------|
| age      |          |                      |              |
| note     |          |                      |              |
| success  |          |                      |              |
| nom      |          |                      |              |

---

### Exercice 5 — Programme concret

Écrire un programme `Bulletin.java` permettant de manipuler les informations d'un étudiant.

#### Variables à utiliser
- `String nom`
- `int age`
- `double noteCC`
- `double noteDS`
- `double moyenne`
- `boolean admis`

### Travail demandé

1. déclarer et initialiser les variables ;
2. calculer la moyenne :

```java
moyenne = (noteCC + noteDS) / 2;
```

3. déterminer si l'étudiant est admis avec la règle :
   - admis si `moyenne >= 10`

4. afficher un compte rendu complet.

### Exemple de sortie attendue

```text
Nom : Karim
Age : 22
Note CC : 14.0
Note DS : 11.0
Moyenne : 12.5
Admis : true
```

### Questions
1. Quel est le type de `moyenne` ?
2. Pourquoi `admis` est-il de type `boolean` ?
3. Quels opérateurs sont utilisés dans ce programme ?

---

## 4. Résolution détaillée d'un programme concret

### Énoncé

On souhaite écrire un programme qui :

- stocke le prix unitaire d'un produit ;
- stocke une quantité ;
- calcule le prix total ;
- vérifie si le total dépasse 100 ;
- affiche les résultats.


### Analyse détaillée

#### Ligne 1
```java
double prixUnitaire = 24.5;
```
- déclaration d'une variable `double` ;
- cette variable contient un nombre décimal ;
- la valeur est stockée directement comme type primitif.

#### Ligne 2
```java
int quantite = 5;
```
- déclaration d'un entier ;
- `int` est adapté car la quantité ne contient pas de décimales.

#### Ligne 3
```java
double total = prixUnitaire * quantite;
```
- opérateur utilisé : `*` ;
- Java convertit automatiquement `quantite` en `double` pour effectuer le calcul ;
- le résultat est stocké dans `total`.

#### Ligne 4
```java
boolean remise = total > 100;
```
- opérateur utilisé : `>` ;
- le résultat de la comparaison est un booléen ;
- `remise` vaut `true` si le total dépasse 100, sinon `false`.

#### Lignes d'affichage
```java
System.out.println(...);
```
- affichage des valeurs calculées ;
- permet de vérifier le bon déroulement du programme.

### Résultat obtenu

```text
Prix unitaire : 24.5
Quantite : 5
Total : 122.5
Remise applicable : true
```

### Ce qu'il faut retenir
- `double` est utilisé pour les montants décimaux ;
- `int` est utilisé pour les quantités entières ;
- une expression relationnelle produit un `boolean` ;
- les opérateurs permettent de calculer et de prendre des décisions.

---

## 5. Questions de synthèse

1. Quelle différence existe-t-il entre `int` et `double` ?
2. Pourquoi `float` nécessite-t-il le suffixe `f` ?
3. Que compare `==` avec des types primitifs ?
4. Quelle différence entre `==` sur primitifs et `==` sur objets ?
5. Où sont stockées les variables primitives locales ?
6. Quel type choisir pour représenter un prix ? Pourquoi ?

---
