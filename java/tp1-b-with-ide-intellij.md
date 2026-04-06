# TP1 : Installation de Java et premier programme avec IntelliJ IDEA

## Objectifs
- Installer le JDK (Déja effectué tp1.md)
- Installer IntelliJ IDEA
- Créer un premier projet Java
- Compiler et exécuter un programme Java avec IntelliJ IDEA

---

## 1. Installation du JDK (Déja effectué tp1.md)

### Télécharger 
- OpenJDK (recommandé) : https://adoptium.net/

### Installer
- Lancer l’installateur et suivre les étapes

### Vérifier l’installation
Ouvrir un terminal et exécuter :

```bash
java -version
javac -version
```

Résultat attendu : affichage des versions installées.

---

## 2. Installation de IntelliJ IDEA

### Télécharger
- Version Community : https://www.jetbrains.com/idea/download/

### Installer
- Lancer l’installateur
- Conserver les options par défaut

---

## 3. Lancer IntelliJ IDEA

Au premier démarrage :
- choisir l’interface par défaut
- accepter les paramètres standards
- vérifier que le JDK est détecté

Si aucun JDK n’est détecté :
- aller dans **File > Project Structure > SDKs**
- ajouter le dossier d’installation du JDK

---

## 4. Créer un premier projet Java

1. Ouvrir IntelliJ IDEA
2. Cliquer sur **New Project**
3. Choisir **Java**
4. Sélectionner le **JDK installé**
5. Donner un nom au projet, par exemple :

```text
TP1-HelloJava
```

6. Valider la création du projet

---

## 5. Créer une première classe Java

1. Dans le dossier `src`, faire clic droit
2. Choisir **New > Java Class**
3. Nommer la classe :

```text
HelloWorld
```

4. Saisir le code suivant :

```java
public class HelloWorld {

    public static void main(String[] args) {
        System.out.println("Hello, Java!");
    }

}
```

---

## 6. Exécuter le programme

Méthode 1 :
- cliquer sur l’icône de lecture à gauche de la méthode `main`

Méthode 2 :
- clic droit dans le fichier
- choisir **Run 'HelloWorld.main()'**

Résultat attendu :

```text
Hello, Java!
```

---

## 7. Travail demandé

1. Modifier le programme pour afficher votre nom
2. Ajouter une deuxième ligne affichant votre formation
3. Ajouter une troisième ligne affichant l’année universitaire
4. Exécuter le programme dans IntelliJ IDEA


---

## 8. Questions

1. Quel est le rôle du JDK dans IntelliJ IDEA ?
2. Quelle est la différence entre un projet Java et une classe Java ?
3. Quel est le rôle de la méthode `main` ?
4. Quelle différence y a-t-il entre exécuter un programme dans IntelliJ IDEA et en ligne de commande ?
