##  Grille de notation des projets

- Installation de l’environnement de travail (selon votre projet vous devez au minimum avoir installé les outils nécessaires pour la réalisation du  projet)
- Documentation sur le projet (comme pour chaque projet, vous devez vous documenter sur le problème à résoudre et savoir si des solutions clés en main sont disponibles) 
- Autonomie et travail en équipe (votre capacité à résoudre les problèmes que vous aurez tout le long du projet) 
- Développement Python ( chaque projet comprend une partie d'écriture du code en Python ) 
- Présentation orale et une démo devrait être préparées


# Projet 1 : Analyse de données

Analyse de données à l'aide des librairies Python

En adoptant une démarche orientée Analyse de données vue en cours, vous devez choisir un dataset de données
- Nettoyer les données
- Explorer les données
- Raconter l'histoire des données (storytelling)

# Projet 2 : Traitement des données Hadoop MapReduce

Écrire un programme MapReduce en s'inspirant de l'exemple du WordCount vu en cours.    

Votre programme écrit en Python doit prendre en paramètre deux paramètres supplémentaires 
  - un paramètre pour préciser la ou les colonnes sur laquelle/lesquelles on aimerait effectuer l'aggragation (GroupBy) 
  -  le second paramètre une mesure sur laquelle on aimerait agréger les données. 

Le résultat devrait être stocker naturellement sur Hadoop Hdfs.    

Vous devez mettre en place également un programme permettant de visualiser les données ainsi traitées de manière interactive.    


# Projet 3 : Traitement de données Spark

Écrire un programme Spark qui prend en entrée un fichier au format csv ou json.    
Le programme doit être écrit en Python.    

Ce programme doit 
- Nettoyer les données
- Pre-agréger les données si nécessaire
- Stocker le résultat après traitement sur HDFS
- Mettre en place un programme permettant de visualiser les données traitées

# Projet 4 : Traitement de données temps réel

Le but de ce projet consistera à récupérer les données provenant d'Internet en temps réel.    
Exemple : 

- Utiliser l'Api de Twitter pour récupérer tous les tweets contenant un HashTag spécifique ou concernant une entreprise. 
- Utiliser l'Api de la SNCF pour récupérer les données liées aux conditions de circulation et  temps réel. 

L'utilisation de Kafka sera très apprécié.    

Une fois les données récupérées, vous devez les stocker sur HDFS. 

Le programme doit être écrit en Python. 




