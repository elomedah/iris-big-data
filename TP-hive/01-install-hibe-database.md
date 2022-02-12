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

### Démarrer
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
```

La version affichée devrait être 3.2.1
