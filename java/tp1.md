# TP1 — Installation Java et premier programme

## Objectifs
- Installer le JDK
- Vérifier l'installation
- Compiler et exécuter un programme Java en ligne de commande

---

## 1. Installation du JDK

### Télécharger
- OpenJDK (recommandé) : https://adoptium.net/
- Oracle JDK : https://www.oracle.com/java/technologies/downloads/

### Installer
- Lancer l’installateur et suivre les étapes

---

## 2. Vérification de l’installation

Ouvrir un terminal et exécuter :

```bash
java -version
javac -version
```

Résultat attendu : affichage des versions installées.

---

## 3. Configuration (si nécessaire)

### Windows
```bash
setx JAVA_HOME "C:\Program Files\Java\jdk-XX"
setx PATH "%JAVA_HOME%\bin;%PATH%"
```

### Linux / macOS
```bash
export JAVA_HOME=/usr/lib/jvm/jdk-XX
export PATH=$JAVA_HOME/bin:$PATH
```

---

## 4. Création du programme

Créer un fichier :

```bash
HelloWorld.java
```

Contenu :

```java
public class HelloWorld {

    public static void main(String[] args) {
        System.out.println("Hello, Java!");
    }

}
```

---

## 5. Compilation

```bash
javac HelloWorld.java
```

Fichier généré :
```bash
HelloWorld.class
```

---

## 6. Exécution

```bash
java HelloWorld
```

Résultat attendu :
```bash
Hello, Java!
```

---

## 7. Travail demandé

1. Modifier le programme pour afficher votre nom
2. Ajouter une deuxième ligne affichant votre formation
3. Compiler et exécuter le programme

---

## 8. Questions

1. Quelle est la différence entre `javac` et `java` ?
2. Que contient le fichier `.class` ?
3. Pourquoi le nom du fichier doit correspondre au nom de la classe ?

---

