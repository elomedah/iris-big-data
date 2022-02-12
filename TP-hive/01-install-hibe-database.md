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
* Effectuer les TP sur Hadoop (installation et prise en main)
* Connaître les bases du requêtage SQL


## Installation


### Télécharger

### Configurer
