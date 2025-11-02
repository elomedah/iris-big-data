# TP MANIPULATION HIVE

Manipuler des commandes HQL (Hive Query Language) qui vous permettront de créer, charger, modifier, supprimer les données

Pré-requis : Effectuer le TP 01-install-hive-docker
Vous pourriez avec les commandes sur la ligne de commande beeline ou utiliser Intellij ou Dbeaver pour vous connecter à la base de donnée.

## Création de table

### Row format

ROW FORMAT est une clause qui définit comment Hive doit lire et écrire les données dans les fichiers. Elle spécifie la structure et le format de sérialisation des lignes de données.
Pourquoi ROW FORMAT ?
Hive stocke les données dans des fichiers (HDFS généralement). Ces fichiers peuvent avoir différents formats :

- CSV avec des virgules
- TSV avec des tabulations
- JSON
- Formats personnalisés

ROW FORMAT indique à Hive comment interpréter ces fichiers.

