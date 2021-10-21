# Mongodb TP
## Introduction  

Rappels :

MongoDB est un système de gestion de bases de données (SGBD) qui gère plusieurs bases de données.   
Ce concept est identique dans un SGBD relationnel.

Chaque base contient plusieurs collections. La collection peut être vue comme une table.   

Chaque collection contient des documents. Le document peut être vu comme un tuple (une ligne) d’une table.   

## Installation

### Pré requis
Vous devez installer le système ubuntu sur votre machine
Si vous utilisez déja une distribution linux vous pouvez installer mongodb en suivant la documentation de votre distribution.

Documentation officielle d'installation sur ubuntu https://doc.ubuntu-fr.org/mongodb
Documentation officielle d'installation sur Mac https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/

### Installation et démarrage

##### Installation
Ouvrir le terminal ubuntu en tant qu'administrateur

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 68818C72E52529D4
sudo echo "deb http://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```

##### Démarrage

Lancer mongodb :
```
sudo service mongod start
```

Si la commande échoue, vous pouvez démarrer mongo manuellement en suivant les étapes suivantes. Dans ce cas mongo ne sera pas un service mais ne vous empêchera pas de suivre le TP.
```
sudo mkdir -p /data/db
sudo  nohup mongod & 
```

Pour vérifier que mongo est bien demarré vous pouvez exécuter cette commande.
La ligne de commande de mongo devrait s'activer.

```
mongo
```

Exécuter cette requête
```
>show dbs
```

## Commande de base

Connexion à une base : use nom_de_la_base. Cette commande possède une double fonction, elle assure la connexion si la base existe, elle la crée puis s’y connecte si la base n’existe pas encore. Une fois connecté, la commande db permet de connaître la base.

```
> use iris_user_db
switched to db iris_user_db
> db
iris_user_db
>
```

La syntaxe utilisée est une syntaxe JavaScript  

#### Les collections
MongoDB est dit schemaless (sans schéma de base de données), on ne précise rien de la structure des données qui seront insérées par la suite dans la collection.

###### Création de collections
Exécuter la commande suivante pour créer une collection.
```
db.createCollection('etudiant')
{ "ok" : 1 }
```
SQL : create table etudiant ... 

show collections permet de lister toutes les collections de la base courante.
```
show collections
```

###### Gérer des documents

Insertion des documents
 https://docs.mongodb.com/manual/tutorial/insert-documents/ 
 
```
db.etudiant.insert({'nom': 'Matthieu', 'prenom': 'Galet', 'age': 12, 'username' : 'matgal', 'sexe' : 'M'})
db.etudiant.insert({'nom': 'Dauv', 'prenom': 'Jeremie', 'age': 22, 'username' : 'jeremie', 'sexe' : 'M'})
db.etudiant.insert({'nom': 'Ali', 'prenom': 'Hassani', 'age': 25, 'username' : 'aliha', 'sexe' : 'M'})
db.etudiant.insert({'nom': 'Deves', 'prenom': 'Sara', 'age': 18, 'username' : 'devsara', 'sexe' : 'F'})
```
L'équivalent sql est : insert into etudiant(nom,prenom,age,username,sexe) values ( ...........)

Vous pouvez insérer plusieurs entrées dans la même requête en utilisant insertMany

```
db.etudiant.insertMany([
 {'nom': 'Simon', 'prenom': 'Pierre', 'age': 56, 'username' : 'simonp', 'sexe' : 'M', 'contact' : {'email': 'simon@iris.com'}},
 {'nom': 'Hilda', 'prenom': 'Anne', 'age': 32, 'username' : 'anned', 'sexe' : 'F', 'contact' : {'tel': '07342567893'}},
 {'nom': 'Dann', 'prenom': 'Yanne', 'username' : 'danni', 'sexe' : 'F', 'contact' : {'email': 'danni1992@iris.com', 'tel': '07342567893'}}
])
```

Que remarquez-vous concernant les différents types de données (format des chaînes de caractères, nombres) ?  
Est-ce que tous les attributs sont obligatoires ? 

###### Lister ou trouver les documents d’une collection
https://docs.mongodb.com/manual/tutorial/query-documents/

Récuper tous les étudiants
```
db.etudiant.find({})
```
L'équivalent sql est : select * from etudiant

Sélectionner uniquement les étudiantes
```
db.etudiant.find({'sexe' : 'F'})
```
L'équivalent sql est : select * from etudiant where sexe='F'


Sélectionner les étudiants de sexe masculin et majeurs

https://docs.mongodb.com/manual/reference/operator/query-comparison/#std-label-query-selectors-comparison 

```
db.etudiant.find({'sexe' : 'M', age: { $gte: 18 }})
```
L'équivalent sql est : select * from etudiant where sexe='M' and age >= 18   

Sélectionner les étudiants de sexe F ou les étudiants majeurs

```
db.etudiant.find( { $or: [{sexe: "F"}, {age:{ $gte: 18 } } ] } )
```
L'équivalent sql est : select * from etudiant where sexe='F' or age >= 18   

###### Mise à jour d'un document
https://docs.mongodb.com/manual/tutorial/update-documents/

```
db.etudiant.updateOne(
   { 'nom': "Dann" },
   {
     $set: { "age": 29}
   }
)
```
L'équivalent sql est : update etudiant set age=29 where nom='Dann'

###### Suppression d'un document
https://docs.mongodb.com/manual/tutorial/remove-documents/

```
db.etudiant.deleteOne( { nom: "Simon" } )
```

L'équivalent sql est : delete from etudiant where nom='Simon'

Que fait la commande deleteMany ? 
Sans exécuter ce code db.xxxx.deleteMany({}) (xxxx étant la collection) que se passerait-il ?

###### Suppression d'une collection
```
db.etudiant.drop()
```
En SQL : drop table etudiant

## Import de données et requetage

###### Import de données

https://docs.mongodb.com/database-tools/mongoimport/ 

```
$ wget https://raw.githubusercontent.com/elomedah/iris-2020/main/prix-nobel.json
```
La base de données sera nommée : prix_nobel_db
La collection sera nommée : laureat
La syntaxe
```
mongoimport --db my_db_name --collection collectionName --file fileName.json
```

1. Combien de documents ont été importés ?
2. Que fait la commande db.laureat.findOne()?
3. Quel laureat à gagner le prix nobel en 2000 en medecine ?
4. Afficher les lauréats 
5. Afficher uniquement les categories où le prix nobel est decerné 

7. Quelle année le premier prix nobel a été decerné ?
