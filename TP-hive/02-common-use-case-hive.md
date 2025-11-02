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
    emails ARRAY<STRING>,           -- Liste d'emails
    preferences MAP<STRING, STRING> -- Clé-valeur
)
ROW FORMAT DELIMITED
LINES TERMINATED BY '\n'
FIELDS TERMINATED BY '|'           -- Colonnes séparées par |
COLLECTION ITEMS TERMINATED BY ',' -- Emails séparés par ,
MAP KEYS TERMINATED BY ':';        -- Clé:valeur avec :
STORED AS TEXTFILE;
```

