# Spark Training
## Environnement local de développement Spark

- IDE : Pycharm https://www.jetbrains.com/pycharm/download/?section=windows
- Pour ceux qui ont windows, mettre la variable d'environnement HADOOP_HOME après avoir télécharger les winutils sur ce repository : https://github.com/steveloughran/winutils. Déezipper le contenu dans le répetoire par exemple C:/hadoop/
- Utiliser Java 8 ou 11
- Télécharger la version 3.10 de python : https://www.python.org/downloads/release/python-3100/ 
- Vérifier que les variables d'environnement suivantes sont bien configurées :
   - JAVA_HOME=Your_java_path_here
   - HADOOP_HOME=C:/hadoop/hadoop-3.0.0 
   - PYSPARK_PYTHON=python
 
## Installation de pyspark

Cloner le projet sample pyspark :


```
```

Sur une ligne de commande creer une virtual env 

```
python -m venv .venv
```
Activer la virtual env .venv

```
source .venv/bin/activate 
```

Installer pyspark 3.5.1
```
pip install pyspark==3.5.1
```


