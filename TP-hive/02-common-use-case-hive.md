# TP MANIPULATION HIVE

Manipuler des commandes HQL (Hive Query Language) qui vous permettront de créer, charger, modifier, supprimer les données

Pré-requis : Effectuer le TP 01-install-hive-docker
Vous pourriez avec les commandes sur la ligne de commande beeline ou utiliser Intellij ou Dbeaver pour vous connecter à la base de donnée.

## Création de table

### Row format

ROW FORMAT est une clause qui définit comment Hive doit lire et écrire les données dans les fichiers. Elle spécifie la structure et le format de sérialisation des lignes de données.   
Hive stocke les données dans des fichiers (HDFS généralement). Ces fichiers peuvent avoir différents formats :

- CSV avec des virgules
- TSV avec des tabulations
- JSON
- Formats personnalisés

ROW FORMAT indique à Hive comment interpréter ces fichiers.


#### ROW FORMAT DELIMITED (Simple)

Pour les fichiers texte avec des séparateurs simples :

```
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','        -- Sépare les colonnes
LINES TERMINATED BY '\n'        -- Sépare les lignes
```
#### ROW FORMAT SERDE (Avancé)

Un SerDe (Serializer/Deserializer) est une bibliothèque qui sait lire/écrire un format spécifique.

```
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.JsonSerDe'
```

Exemple
```
CREATE TABLE employees (
    emp_id INT,
    emp_name STRING,
    department STRING,
    salary DOUBLE,
    emails ARRAY<STRING>,
    preferences MAP<STRING, STRING>
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '|'
COLLECTION ITEMS TERMINATED BY ','
MAP KEYS TERMINATED BY ':'
LINES TERMINATED BY '\n'
STORED AS TEXTFILE;
```

Insert des données dans la table employees

```
INSERT INTO TABLE employees VALUES 
(1, 'Alice Dupont', 'Engineering', 75000.50, 
 array('alice@company.com', 'alice.d@work.com'), 
 map('theme', 'dark', 'lang', 'fr', 'notifications', 'on')),
 
(2, 'Bob Martin', 'Sales', 65000.00, 
 array('bob@company.com'), 
 map('theme', 'light', 'lang', 'en')),
 
(3, 'Carol Smith', 'Marketing', 70000.75, 
 array('carol@company.com', 'c.smith@personal.com', 'carol.s@work.com'), 
 map('lang', 'es', 'theme', 'dark')),
 
(4, 'David Lee', 'Engineering', 80000.00, 
 array('david@company.com'), 
 map('notifications', 'off', 'lang', 'en')),
 
(5, 'Emma Wilson', 'HR', 68000.25, 
 array('emma@company.com', 'e.wilson@work.com'), 
 map('theme', 'light', 'lang', 'fr', 'timezone', 'CET'));
```

#### Description de la table

Pour voir l'ensemble des paramètres utilisés pour créer la table 

```
SHOW CREATE TABLE employees; 
```

Vous remarquerez que les données sont stockées /opt/hive/data/warehouse/employees

Sur un autre terminal connectez-vous au terminal sh
```
docker exec -it hive4 sh
```

```
hadoop fs -ls /opt/hive/data/warehouse/employees
```

Afficher le contenu des fichiers 

```
hadoop fs -cat /opt/hive/data/warehouse/employees/*
```

### Table externe vs Table Interne

1. Table Interne (Managed Table)

- Hive gère complètement les données et les métadonnées
- Lors du DROP TABLE, les données sont supprimées du HDFS
- Les données sont stockées dans le warehouse Hive par défaut
- Hive a le contrôle total du cycle de vie des données

2. Table Externe (External Table)

- Hive gère uniquement les métadonnées
- Lors du DROP TABLE, seules les métadonnées sont supprimées, les données restent dans HDFS
- Les données peuvent être dans n'importe quel répertoire HDFS
- Utilisée quand les données sont partagées entre plusieurs systèmes

La table précedente qu'on a créé est une table interne. En supprimant la table vous verez que les données aussi seront supprimées.   

Faire une sauvegarde du repertoire /opt/hive/data/warehouse/employees
```
hadoop fs -mkdir -p /opt/hive/data/warehouse/external
hadoop fs -cp -p /opt/hive/data/warehouse/employees /opt/hive/data/warehouse/external
```

Supprimer la table employees
```
DROP TABLE employees;
```
Vous remarquerez que le repertoire /opt/hive/data/warehouse/employees a été aussi supprimé
```
hadoop fs -ls -p /opt/hive/data/warehouse/
```

Créons de nouveau la table employees comme une table EXTERNE.

```
CREATE EXTERNAL TABLE employees (
    emp_id INT,
    emp_name STRING,
    department STRING,
    salary DOUBLE,
    emails ARRAY<STRING>,
    preferences MAP<STRING, STRING>
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '|'
COLLECTION ITEMS TERMINATED BY ','
MAP KEYS TERMINATED BY ':'
LINES TERMINATED BY '\n'
STORED AS TEXTFILE
LOCATION '/opt/hive/data/warehouse/external/employees/';
```

Faire un select pour vérifier que la table a été bien créée

```
SELECT * FROM employees
SHOW CREATE TABLE employees
```

Supprimer la nouvelle table employees.
Que remarquez vous ? 

#### Table externe sur des données existantes   

Télecharger le fichier data-01-employees.csv

```
wget https://raw.githubusercontent.com/elomedah/iris-big-data/refs/heads/master/TP-hive/data-01-employees.csv
```
Déplacer le fichier dans le repertoire /opt/hive/data/warehouse/external/employees/   
Créer de nouveau la table employees    

Faites des rêquetes simples 

```
SELECT count(*) FROM employees;
```

Que remarquez vous ?

### A vous de jouer !

- Partionning
- Stored As
- LOAD DATA INPATH

