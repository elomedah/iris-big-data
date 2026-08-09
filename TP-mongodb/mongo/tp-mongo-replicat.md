# Mongodb cluster configuration

MongoDB est une base de données à usage général conçue pour le Web. Entre autres choses, il offre une haute disponibilité lorsqu'il est utilisé dans des clusters, également appelés Replica Set .    
Un  Replica Set est un groupe de serveurs MongoDB, appelés nœuds, contenant une copie identique des données. Si l'un des serveurs tombe en panne, les deux autres reprendront la charge pendant que celui en panne redémarre.
![image](https://github.com/user-attachments/assets/7e86a510-55e4-4a22-b755-a146f1c48fe2)

Documentation officielle :    
https://www.mongodb.com/resources/products/compatibilities/deploying-a-mongodb-cluster-with-docker   
https://www.mongodb.com/docs/manual/replication/#replication 
# Pré requis
- Les bases de MongoDb (Tp1)
- Docker

# Configuration d'un cluster mongodb

Pour configurer un cluster mongodb nous allons l'effectuer en 3 étapes

1- Créez un réseau Docker   
2- Démarrez trois instances de MongoDB   
3- Lancez le replica set   

## Creation d'un réseau docker

La première étape consiste à créer un réseau Docker.    
Ce réseau permettra à chacun de vos conteneurs exécutés sur ce réseau de se voir.  
Pour créer un réseau, exécutez la commande suivante
```
docker network create mongoCluster
```

Après avoir exécuté la commande, vous devriez voir l'identifiant du réseau que vous venez de créer.

![image](https://github.com/user-attachments/assets/05ce8ee7-1b1c-4808-8ce0-35ed64f0659c)

Notez que cette commande ne doit être exécutée qu’une seule fois. Si vous redémarrez vos conteneurs ultérieurement, vous n'aurez pas besoin de recréer ce réseau.

## Démarrage des instances mongodb

Comme le tp precédent pour démarrer une instance, executer la commande suivante

```
docker run -d --rm -p 27017:27017 --name mongo1 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --bind_ip 0.0.0.0
```

Ici, vous dites à Docker de démarrer un conteneur avec les paramètres suivants :

-d indique que ce conteneur doit s'exécuter en mode détaché (en arrière-plan).   
-p indique le mappage de port. Toute requête entrante sur le port 27017 de votre machine sera redirigée vers le port 27017 du conteneur.   
--name indique le nom du conteneur. Cela deviendra le nom d'hôte de cette machine.   
--network indique quel réseau Docker utiliser. Tous les conteneurs du même réseau peuvent se voir.   
mongo:5 est l'image qui sera utilisée par Docker. Cette image est la version 5 du serveur MongoDB Community (maintenue par Docker). Vous pouvez également utiliser une image personnalisée MongoDB Enterprise.   
Le reste de cette instruction est la commande qui sera exécutée une fois le conteneur démarré. Cette commande crée une nouvelle instance mongod prête pour un replica set.   

Si la commande a été exécutée avec succès, vous devriez voir une longue chaîne hexadécimale représentant l'ID du conteneur.   

![image](https://github.com/user-attachments/assets/bcda20c2-2123-4e83-8bbe-4b1e4d36afb3)

Démarrez deux autres conteneurs. Vous devrez utiliser un nom différent et un port différent pour ces deux-là.   

Démarrez le deuxième (Le démarrage devrait être rapide étant donné que vous avez déja l'image docker mongo:5   
```
docker run -d --rm -p 27018:27017 --name mongo2 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --bind_ip 0.0.0.0
```
Démarrez le troisième

```
docker run -d --rm -p 27019:27017 --name mongo3 --network mongoCluster mongo:5 mongod --replSet myReplicaSet --bind_ip 0.0.0.0
```

Pour vérifier que vous avez les 3 instances correctement démarrées, exécutez la commande suivante :

```
docker ps
```
![image](https://github.com/user-attachments/assets/9f687f5b-940e-440a-923c-8eb6d8bbe472)


## Initialisation du replica set

L’étape suivante consiste à créer le replica set réel avec les trois membres.      
Pour ce faire, vous devrez utiliser le shell MongoDB (tp1).   
Cependant, si vous n’avez pas l’outil installé sur votre ordinateur portable, il est possible d’utiliser mongosh disponible dans les conteneurs avec la commande docker exec.   

Pour windows : Pour éviter une erreur docker executer d'abord  commande suivante et ensuite fermer la console et ouvrer une nouvelle console   


```
echo "alias docker='winpty docker'" >> ~/.bashrc
```

```
docker exec -it mongo1 mongosh --eval "rs.initiate({
 _id: \"myReplicaSet\",
 members: [
   {_id: 0, host: \"mongo1\"},
   {_id: 1, host: \"mongo2\"},
   {_id: 2, host: \"mongo3\"}
 ]
})"
```

Cette commande indique à Docker d'exécuter l'outil mongosh dans le conteneur nommé mongo1. mongosh essaiera ensuite d'évaluer la commande rs.initiate() pour initialiser le replica set.   

Dans le cadre de l'objet de configuration transmis à rs.initiate(), vous devrez spécifier le nom du replica set (myReplicaSet, dans ce cas), ainsi que la liste des membres qui feront partie du replica set.      
Les noms d'hôtes des conteneurs sont les noms des conteneurs tels que spécifiés par le paramètre --name dans la commande docker run.    

Si la commande a été exécutée avec succès, vous devriez voir un message de la CLI mongosh indiquant:

```
{ ok: 1 }
```

## Test and verification du Replica Set

Vous devriez maintenant avoir un replica set en cours d'exécution.   
Si vous souhaitez vérifier que tout a été configuré correctement, vous pouvez évaluer l'instruction rs.status(). Cela vous fournira l'état de votre replica set, y compris la liste des membres.

```
docker exec -it mongo1 mongosh --eval "rs.status()"
```

### Connexion à l'instance mongo1

Vous pouvez également vous connecter à votre cluster à l'aide de MongoDB Compass   (https://www.mongodb.com/products/tools/compass)  pour créer une base de données et ajouter des documents.   
Vous pourriez téléchargez les données de parcoursup : https://data.enseignementsup-recherche.gouv.fr/pages/parcoursupdata/?disjunctive.fili
- Créer la base de données : universite
- Créér la collection : parcourssup
- Importer les données
- Faire des rêquetes simples sur la base de données


Notez que les données sont créées à l'intérieur du stockage du conteneur et seront détruites lorsque les conteneurs seront supprimés du système hôte.    
Pour vérifier que votre replica set fonctionne, vous pouvez essayer d'arrêter l'un des conteneurs avec docker stop et essayer de lire à nouveau à partir de votre base de données.

```
docker stop mongo1
```
Les données seront toujours là. Vous pouvez voir que le cluster est toujours en cours d'exécution en utilisant rs.status() sur le conteneur mongo2.

```
docker exec -it mongo2 mongosh --eval "rs.status()"
```
