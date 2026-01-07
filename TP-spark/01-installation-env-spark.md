# Spark Training
## Environnement local de développement Spark

- IDE : Pycharm https://www.jetbrains.com/pycharm/download/?section=windows
- Pour ceux qui ont windows, mettre la variable d'environnement HADOOP_HOME après avoir télécharger les winutils sur ce repository : https://github.com/steveloughran/winutils. Déezipper le contenu dans le répetoire par exemple C:/hadoop/
- Utiliser Java 8 ou 11 : https://www.openlogic.com/openjdk-downloads 
- Télécharger la version 3.10 de python : https://www.python.org/downloads/release/python-3100/ 
- Vérifier que les variables d'environnement suivantes sont bien configurées :
   - JAVA_HOME=Your_java_path_here
   - HADOOP_HOME=C:/hadoop/hadoop-3.0.0 
   - PYSPARK_PYTHON=python
 
## Installation de pyspark

Cloner le projet  spark-kata via git ou directement depuis pycharm :

```
git clone https://github.com/elomedah/spark-kata.git
```

Sur une ligne de commande creer une virtual env 

```
python -m venv .venv
```
Activer la virtual env .venv

```
source .venv/bin/activate 
```
ou sur windows 

```
.venv\\Scripts\\activate
```

Installer pyspark 3.5.1
```
pip install pyspark==3.5.1
```


## Lancer spark

Lancer le main depuis PyCharm.

<img width="2539" height="1281" alt="image" src="https://github.com/user-attachments/assets/f8aecd32-1a70-4c89-8206-191bfae6b4ab" />



