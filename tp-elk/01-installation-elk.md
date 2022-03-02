
# TP
# M1  - ELK

 
La stack ELK était au départ principalement utilisé pour traiter des fichiers de logs en temps réels.   
Le moteur de recherche Elasticsearch et Kibana permettent de retrouver et visualiser des anomalies facilement.    
Elasticsearch permet à la fois de rechercher rapidement et avec pertinence.   
La stack est maintenant utilisé dans une multitude de cas d’usage en particulier dans des processus d’ETL.   
Le processus standard consiste à :   
- Récupérer les données et les traiter
- Les stocker dans un moteur de recherche
- Les afficher et les analyser sous forme de graphe

Cas d’usage:
- Traitement de fichiers de logs applicatifs en temps réel
- Utilisation dans un processus d’ETL pour de l’informatique décisionnelle
- Supervision d’un parc informatique
- Configuration Management Database
- IOT Affichage des données issues d’objets connectés et senseurs

Un processus classique  du traitement de la donnée se deroule de la sorte :

1.	 Récupération du fichier source (données)
2.	Logstash effectue un processus d’ETL
        - Filtrage des données
        - Ajout de champs
        - Redirection des données traitées
3.	Elasticsearch stock les données
4.	Kibana permet de visualiser les données


## Logstash
Logstash décompose le traitement en 3 zones:
1. Les entrées
2. Les filtres
3. Les sorties
Il est très polyvalent car il est capable de traiter tout ce qui est assimilé à du texte. Il existe de nombreux plugins pour augmenter ses capacités. 


### Les entrées
Syntaxe
### Input à partir d'un fichier

```
input {
 file {
   path => ...
 }
}
```
 
### Input à partir d'une base de données MySQL

```
input {
 jdbc {
   jdbc_driver_library => "/path/to/mysql-connector-java-5.1.33-bin.jar"
   jdbc_driver_class => "com.mysql.jdbc.Driver"
   jdbc_connection_string => "jdbc:mysql://host:port/database"
   jdbc_user => "user"
   jdbc_password => "password"
   statement => "SELECT ..."
   jdbc_paging_enabled => "true"
   jdbc_page_size => "50000"
 }
}

```
 
 
Remarque: Lorsque l'on spécifie un input de type file, on ne se préoccupe pas du type de fichier, ce sont les filtres qui vont par la suite gérer les différents types de fichiers (csv, xml ...)

### Les filtres
Logstash à la capacité de filtrer tous les types possibles  de données.   
Il existe de nombreux plugins qui permettent de traiter vos données:
- extraction de données
- analyser les données
- nettoyer les données
- ajouter des champs
- supprimer des champs
- détecter du texte en fonction de patterns et parser ce texte

grok permet de créer des expressions régulières permettant de détecter des patterns dans les données et de stocker son contenu dans différents champs.    
Il est livré avec des patterns prédéfinis comme Syslog ou Apache. Il est bien sûr possible de définir ses propres patterns.
Pour déboguer:http://grokdebug.herokuapp.com/
Syntaxe
```
filter {
  # Lecture d'un fichier csv
  csv {
    	columns => ["col1","col2","col3"]
    	    separator => ";"
  }
}

filter {
 mutate {
   gsub => [
 	# Remplace tous les slashes par des underscore
 	"fieldname", "/", "_",
 	# Remplace les backslashes, point d'interrogation, dièse, et tiret
 	# par un point "."
 	"fieldname2", "[\\?#-]", "."
   ]
 }
}

```

 
### Les sorties
L’output peut servir à passer d’un mode de stockage à un autre. Par exemple, il est possible de convertir un fichier xml vers une base de données elasticsearch ou encore sortir un fichier sql correspondant.

#### Sortie vers la console

```
output {
 stdout { }
}
```
#### Sortie vers une base elasticsearch
 elasticsearch {
	  hosts => [ "address" ]
        index => "index-%{+YYYY.MM.dd}"
}

## Téléchargement et installation
Télécharger le répertoire zip et dézipper. http://bit.ly/3882oyq 
 
  
Dans le répertoire tp-config vous trouverez le fichier test.conf avec ce contenu


```
 input {
   stdin{}
}
output {
   stdout{}
}
```

A votre avis que signifie ce code ?

Ouvrez un terminal 

## Logstash
1-   Positionnez vous  dans le répertoire logstash
2-   Exécutez cette commande ./bin/logstash.bat -f ../tp-config/test.conf 

```
./bin/logstash.sh -f ../tp-config/test.conf 
```
  
 
Que remarquez-vous de nouveau ?

### Analyse des données de la SNCF 

Nous allons analyser les données portant sur les retards de la SNCF.

1. Aller dans le dossier logstash/tp-config
2. Ouvrir le nouveau fichier de configuration logstash-sncf.conf(Ce fichier contiendra la configuration permettant de lire le fichier regularite-mensuelle-tgv-short.csv placé dans le dossier tp-data)
3. Lancer logstash avec la nouvelle configuration et analyser le résultat
```
./bin/logstash.bat -f ../tp-config/logstash-sncf.conf
```
#### Questions

- Décrivez les données ? 
- Quel est le type des données ? 
- La date est elle au bon format ? 



4. Rajouter un filtre qui permettrait de compléter le champ date avec le premier jour du mois et de le convertir en timestamp afin de pouvoir par la suite l'analyser.   
Le plugin *mutate* permet de modifier la valeur d'un champ. On peut récupérer la valeur du champ avec *%{champ}*

```
mutate {
 replace => { "champ" => "valeur" }
}
```
 
La conversion en timestamp s'effectue à l'aide du plugin date.
```
date {
 match => [ "champ", "YYYY-MM-dd" ] #On sélectionne le pattern du champ afin d'éviter les erreurs de cast
 timezone => "UTC"
}
```
5. Caster les valeurs numériques avec logstash pour qu’elasticsearch les prennent en compte.   
 Maintenant que les résultats correspondent à ce que l'on veut (format et type), on souhaite charger les données dans la base elasticsearch.
 
6.	Modifier le fichier d'entrée pour utiliser le fichier regularite-mensuelle-tgv.csv
7.	Modifier le fichier de sortie pour utiliser une base elasticsearch
```
output {
  elasticsearch {
       hosts => [ "localhost" ]
        index => "sncf"
       }
}
```

## Elasticsearch
8. Démarrer elasticsearch
- Aller dans le répertoire elasticsearch
- Lancer le programme elasticsearch

```
./bin/elasticsearch.sh
```

 
Pour vérifier que elasticsearch est bien démarré, cliquez sur ce lien http://localhost:9200/    
  
Nous allons maintenant envoyer les données sur elasticsearch 

```
./bin/logstash.sh -f ../tp-config/logstash-sncf-elastic.conf
```
 
Après l’ajout des données, vous pouvez les voir via l’url suivante:   
http://localhost:9200/sncf/_search?pretty   

## Kibana
Nous allons démarrer maintenant kibana qui va nous permettre de visualiser les données.
 
Kibana se compose de plusieurs onglets :
- Discover : Cet onglet permet de faire des recherches via la barre dédiée et de voir les résultats bruts stockés dans la base elasticsearch
- Visualize : Cet onglet permet de réaliser différents types de graphes à partir d'une requête enregistrée ou à partir d'une nouvelle requête
- Dashboard : Cet onglet permet de regrouper plusieurs graphes réalisés au préalable dans l'onglet “visualize” sur une même page
 
Il est possible de restreindre la plage de recherche temporelle à l'aide du bouton à l’extrême droite de la barre de menu. Par défaut, la valeur est de 15 minutes.   
Il est très important de configurer le bon type de données lorsque l'on ajoute des données à une base elasticsearch.    
En effet, les chaînes de caractères sont analysées par la suite mot par mot en coupant les phrases sur les espaces et en enlevant la ponctuation. À chaque mot correspond un pourcentage d'apparition utilisé pour tirer les données selon leur pertinence.   
Le moteur de recherche utilisé est **APACHE LUCENE** (https://lucene.apache.org/ , https://fr.wikipedia.org/wiki/Lucene)

On retrouve cette analyse dans kibana avec deux champs disponibles sur les champs texte :   
- nom_du_champ : Valeur du champ analysé par elasticsearch
- nom_du_champ.raw : Valeur brute du champ (on conserve la phrase entière)
Lorsque l'on effectue des graphes, il faut donc faire attention au nom du champ que l'on utilise pour ne pas biaiser les résultats
 

## Visualisation
Vous pouvez accéder à Kibana via l’adresse suivante:
http://localhost:5601/app/kibana#/home?_g=()  

Avant de commencer, il faut configurer l'index qui va être utilisé par la suite.   
La première chose à faire est d'aller sélectionner la plage de temps et de sélectionner les 10 dernières années.  

### Questions

- Que remarquez-vous ? Adapter la période en conséquence.   
- Afficher sous la forme d'un camembert les gares de départ où il y a le plus de retard   
- C’est à vous de jouer. Explorer les données en créant une visualisation qui fait parler les données (storytelling).
- Explorer l'API de requetage https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html
   Exemple : Menu Dev Tools
```
GET /sncf/_search
```
