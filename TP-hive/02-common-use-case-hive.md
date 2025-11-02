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

```


