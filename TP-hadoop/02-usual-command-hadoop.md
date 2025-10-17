# Hadoop commandes usuelles

## Généralité

Il existe 2 syntaxes possibles:

```
hadoop : syntaxe du type hadoop fs <commande>
hdfs : syntaxe du type hdfs dfs <commande>
```
hadoop fs : Tout type de système de fichiers ( hdfs, S3, local, ....)   
hdfs dfs : Uniquement hdfs   

## Créer un dossier dans HDFS

```
Commande :
hadoop fs -mkdir [-p] <paths>
Exemple :
hadoop fs mkdir / monDossier
hadoop fs mkdir /user/monDossier1 /user/monDossier2 /user/monDossier3
L’option -p est nécessaire si le dossier parent n’existe pas lors de la création d’un sous répertoire.
```

## Lister le contenu d’un dossier

```
Commande : hadoop fs -ls <args>
Exemple : hadoop fs -ls /user
```

## Exporter un ou plusieurs fichiers de HDFS au local:

```
Commande: hadoop fs -get [-ignorecrc] [-crc] <src> <localdst> 
Exemple : hadoop fs -get /user/monDossier/monFichier.txt /home
```

## Charger un ou plusieurs fichiers du local à HDFS:

```
Commande : hadoop fs -put  <localsrc> ... <dst>
Exemple : hadoop fs -put /home/monFichier.txt /user/monDossier
```

## Exporter un ou plusieurs fichiers de HDFS au local

```
Commande: hadoop fs –copyToLocal [-ignorecrc] [-crc] URI <localdst>  
Exemple : hadoop fs -copyToLocal /user/monDossier/monFichier.txt /home
```

## Charger un ou plusieurs fichiers du local à HDFS
```
Commande : hadoop fs –copyFromLocal <localsrc> URI  
Exemple : hadoop fs -copyFromLocal /home/monFichier.txt /user/monDossier
```

## Déplacer un ou plusieurs fichiers dans HDFS

```
Commande : hadoop fs -mv URI [URI ...] <dest>  
Exemple : hadoop fs -mv /user/monDossier1/monFichier.txt  /user/monDossier2
```

## Copier un ou plusieurs fichiers dans HDFS

```
Commande : hadoop fs -cp [-f] URI [URI ...] <dest>  
Exemple : hadoop fs -cp /user/monDossier1/monFichier.txt  /user/monDossier2
```

## Afficher le contenu d’un fichier

```
Commande: hadoop fs -cat URI [URI ...]
Exemple : hadoop fs -cat /user/monFichier.txt
```

## Pour voir la description d’une commande  ainsi que ses paramètres
```
Commande: hadoop fs -help 
Exemple: hadoop fs -help stat
```
