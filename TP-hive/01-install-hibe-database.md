# TP HIVE

## Introduction
Le framework open-source de la principale plateforme du Big Data, Hadoop, se révèle idéal pour le stockage et le traitement de quantités massives de données.    
Cependant, pour l’extraction de données, cette plateforme se révèle souvent complexe, chronophage et coûteuse.   
C’est pourquoi, la fondation Apache a développé une nouvelle alternative. Il s’agit de Apache Hive.  

Apache Hive est une infrastructure d’entrepôt de données (datawarehouse) intégrée sur Hadoop permettant l'analyse, le requêtage via un langage proche syntaxiquement de SQL ainsi que la synthèse de données.

Apache Hive prend en charge l'analyse des grands ensembles de données stockées dans Hadoop HDFS ou des systèmes de fichiers compatibles tels que Amazon S3. Il fournit un langage similaire à SQL appelée HiveQL7 avec le schéma lors de la lecture et de manière transparente convertit les requêtes en map/reduce, Apache Tez8 et jobs Spark. Tous les trois moteurs d'exécution peuvent fonctionner sur Hadoop YARN. Pour accélérer les requêtes, il fournit des index, y compris bitmap indexes9.

Par défaut, Hive stocke les métadonnées dans une base de données embarquée Apache Derby, et d'autres bases de données client / serveur comme MySQL peuvent éventuellement être utilisées.   

Il y a quatre formats de fichiers pris en charge par Hive: 
1. TEXTFILE
2. SEQUENCEFILE
3. ORC12
4. RCFile


## Pré requis

Pour effectuer ce TP vous devez au préalable :

* Installer une machine virtuelle ubuntu ( si vous êtes sur windows vous pouvez utiliser wsl https://docs.microsoft.com/fr-fr/windows/wsl/install )   
* Créer un user "iris" ( Vous devez travailler dans le repertoire /home/iris)
* Effectuer les TP sur Hadoop (installation et prise en main)
* Connaître les bases du requêtage SQL

Hive ne fonctionne pas avec java11 ni java9.

Vous devez donc installer java8

```
sudo apt install openjdk-8-jdk
```

## Installation
Créer le repertoire de travail tp-hive sous votre home ~

```
cd
mkdir tp-hive
cd tp-hive
```

### Télécharger et decompresser

```
wget https://apache.osuosl.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz
```
A la fin du téléchargement, dézipper le fichier et renommer le repertoire hive (pour faciliter l'accès)

```
tar -xzf apache-hive-3.1.2-bin.tar.gz 
mv apache-hive-3.1.2 hive
```

### Configurer

#### Variable et fichier de conf
Les variables d'environnement hive
Télécharger ce fichier https://raw.githubusercontent.com/elomedah/iris-big-data/master/TP-hive/hive-bash-var.sh et ajouter son contenu dans le fichier .bashrc   
Ce fichier contient une variable HIVE_HOME positionner par défaut sur /home/iris/tp-hive/hive

```
wget https://raw.githubusercontent.com/elomedah/iris-big-data/master/TP-hive/hive-bash-var.sh
cat hive-bash-var.sh >> ~/.bashrc
```

Télécharger le fichier de configurer hive-site.xml et déplacer le dans le dossier /home/iris/tp-hive/hive/conf

```
wget https://raw.githubusercontent.com/elomedah/iris-big-data/master/TP-hive/hive-site.xml

Remplacer les variables :
1. ${system:java.io.tmpdir par /tmp/hive
2. ${system:user.name} par l'utilisateur iris

sed -i 's/\${system:java.io.tmpdir}/\/tmp\/hive/g' /home/iris/tp-hive/hive/conf/hive-site.xml
sed -i 's/\${system:user.name}/iris/g' /home/iris/tp-hive/hive/conf/hive-site.xml

mv hive-site.xml /home/iris/tp-hive/hive/conf
```

#### Préparation stockage sur hadoop hdfs
Créer le repertoire /user/hive/warehouse dans hdfs. Ce repertoire est sera le repertoire du datawarehouse

```
hadoop fs -mkdir -p /user/hive/warehouse
```

Créer le repertoire temporaire tmp.

```
hadoop dfs -mkdir -p /user/tmp
```

Nous devons autoriser hive à écrire dans les répertoires que nous venons de créer

```
hadoop fs -chmod g+w /user/tmp
hadoop fs -chmod g+w /user/hive/warehouse
```

### Initialisation hive
Après l'installation d'Apache Hive, avant de commencer à utiliser Hive, vous devez initialiser la base de données Metastore avec le type de base de données que vous choisissez.    
Par défaut, Hive utilise la base de données Derby, vous pouvez également choisir n'importe quelle base de données RDBS pour Metastore.

```
Charger les variables à partir de .bashrc
source ~/.bashrc
Initialiser la base de donnée qui sera utilisée pour le Metastore
$HIVE_HOME/bin/schematool -initSchema -dbType derby
```

Vérifier la version hive

```
$HIVE_HOME/bin/hive --version

ou simplement

hive --version
```

La version affichée devrait être 3.2.1

### Console Hive CLI, Configuration beeline 

Ouvrir la console hive
```
hive
```

Sur la console qui s'affiche avec *hive>* afficher toutes les bases de données

```
show databases
```

Le résultat devrait ressembler au contenu suivant :

```
hive> show databases;
OK
default
Time taken: 0.277 seconds, Fetched: 1 row(s)
```

Il existe plusieurs limitations de Hive CLI, par conséquent, dans la nouvelle version, elle a été obsolète et a introduit Beeline pour se connecter à Hive.

Hive beeline peut être exécuté en mode intégré, ce qui est un moyen rapide d'exécuter certaines requêtes HiveQL, ceci est similaire à Hive CLI (ancienne version).

Utilisateur : *iris*
Mot de passe : *iris* 
```
beeline -u jdbc:hive2:// -n iris -p iris
```
Sur la console qui s'affiche avec *hive>* afficher toutes les bases de données
Afficher les databases 


Vous devez avoir le résultat suivant

```
0: jdbc:hive2://> show databases;
OK
+----------------+
| database_name  |
+----------------+
| default        |
+----------------+
1 row selected (0.052 seconds)
```

### Configuration hiveServer2

HiveServer2 (HS2) est une interface serveur qui permet aux clients distants d'exécuter des requêtes sur Hive et de récupérer les résultats (une introduction plus détaillée ici).    
L'implémentation actuelle, basée sur Thrift RPC (http://thrift.apache.org/docs/), est une version améliorée de HiveServer et prend en charge la concurrence et l'authentification multi-clients. Il est conçu pour fournir une meilleure prise en charge des clients API ouverts tels que JDBC et ODBC. 

Pour plus de détail : https://cwiki.apache.org/confluence/display/hive/setting+up+hiveserver2 

```
$HIVE_HOME/bin/hiveserver2
ou
$HIVE_HOME/bin/hive --service hiveserver2
```
Vous pouvez maintenant vous connecter à Hive à partir d'un serveur distant à l'aide de Beeline ou à partir d'applications Java, Scala, Python ou dbeaver à l'aide de la chaîne de connexion Hive JDBC

Sur un autre terminal ubuntu

```
beeline -u jdbc:hive2://127.0.0.1:10000 iris iris --showDbInPrompt=true
```

Vous devez avoir le résultat suivant 

```
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Connecting to jdbc:hive2://127.0.0.1:10000
Connected to: Apache Hive (version 3.1.2)
Driver: Hive JDBC (version 3.1.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
Beeline version 3.1.2 by Apache Hive
0: jdbc:hive2://127.0.0.1:10000>
```

### Tester les requêtes de base

#### Créer une database

```
CREATE DATABASE IF NOT EXISTS irisdata;

or

CREATE SCHEMA irisdata;

```
Afficher les databases

```
show databases
```

```
+----------------+
| database_name  |
+----------------+
| default        |
| irisdata       |
+----------------+
2 rows selected (0.087 seconds)
```

Sélectionner la base de donnée irisdata

```
use irisdata;
```

Sur la console

```
jdbc:hive2://127.0.0.1:10000 (irisdata)>
```
#### Créer une table 

Nous allons créer les tables sur la base default

```
 CREATE TABLE etudiant (nom VARCHAR(64), prenom VARCHAR(64), universite VARCHAR(128), age INT);
 
```
Afficher les tables

```
show tables
```

#### Insérer les données

```
 INSERT INTO TABLE etudiant (nom, prenom, universite, age, note) 
 values ('Anna', 'Simons','Iris Paris', 18), 
 ('Simo', 'Abel','Iris Reims', 20),
 ('Zemm', 'Patric','Iris Lyon', 18);
```
#### Sélectionner les données

```
SELECT * FROM etudiant
```

