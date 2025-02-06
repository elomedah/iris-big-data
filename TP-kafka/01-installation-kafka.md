# Kafka Training
Ce TP est inspiré du site officiel de kafka (https://kafka.apache.org/quickstart)    
Dans ce TP, nous allons installer une plateforme Kafka.
Il est recommandé de faire le tp sur le linux or docker.

###  Etape 1 : utiliser docker
Récuperer l'image docker de kafka
```
docker pull apache/kafka:3.9.0
```
Démarrer le container docker kafka
```
docker run -p 9092:9092 apache/kafka:3.9.0
```
Les binaires kafka sont dans le repertoire :

```
cd /opt/kafka/bin
```

Si vous utilisez windows vous avez la possibilité d'installer wsl en suivant les instructions sur ce lien
https://docs.microsoft.com/fr-fr/windows/wsl/install
### Etape 1: Télécharger les fichiers (Si vous utilisez docker vous pouvez ignorez cette étape)

Pour faciliter le TP, créer un répertoire de travail kafka. 
La variable $HOME est le répertoire courant de votre utilisateur. Vous pouvez créer le repertoire kafka où vous voulez.  
```
mkdir $HOME/kafka
cd $HOME/kafka
```
Télécharger Kafka : http://kafka.apache.org/downloads.html  (https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz)
```
wget https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz
```

Télécharger Java : http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html (optionnel si vous avez déjà java)  

```
java -version
sudo apt-get update
sudo apt install default-jre
```


Télécharger zookeeper : http://zookeeper.apache.org/releases.html  (optionnel)

Copier les fichiers téléchargés dans le repertoire kafka.

### Etape 2 : Décompresser et vérification java

Décompresser le fichier kafka télécharger à l'étape 1
```
tar zxvf kafka_2.13-3.7.0.tgz
```

### Etape 3 : Lancement de zookeeper et kafka

Pour lancer Kafka sur votre machine, vous devez avoir deux serveurs :

* Le Zookeeper (le gestionnaire de cluter kafka).  
* Le serveur Kafka.  


##### Lancement zookeeper

La version de Kafka que vous avez téléchargée contient déjà Zookeeper et son fichier de configuration.

Avant de lancer Zookeeper ouvrez le fichier de configuration ./config/zookeeper.properties avec votre éditeur de texte préferé.

```
cd $HOME/kafka/kafka_2.13-3.7.0
vi ./config/zookeeper.properties
```

Pour lancer Zookeeper il faut exécuter la commande suivante :
```
cd $HOME/kafka/kafka_2.13-3.7.0
./bin/zookeeper-server-start.sh ./config/zookeeper.properties
```

Zookeeper comme configuré dans le fichier zookeeper.properties se lance sur le port 2181.

##### Lancement serveur kafka

De même que zookeeper nous devons configurer le fichier le configuration kafka  config/server.properties

```
Ouvrir un autre terminal
cd $HOME/kafka/kafka_2.13-3.7.0
vi config/server.properties
```


Maintenant vous pouvez lancer le serveur kafka avec la commande suivante :

```
./bin/kafka-server-start.sh ./config/server.properties
```

Par défaut le broker kafka va tourner sur le port 9092

### Etape 4 : Topic, Producteur, Consomateur

Grâce aux étapes précedentes notre cluster (à ce stade composé d'un seul broker kafka et une instance zookeeper) kafka est prêt à être utilisé.  

##### Topic
Pour commencer à envoyer des messages, créons un topic.

```
Ouvrir un autre terminal
cd $HOME/kafka/kafka_2.13-3.7.0
Création de topic
./bin/kafka-topics.sh --create  --replication-factor 1 --partitions 1 --topic mon-tunnel-topic --bootstrap-server localhost:9092
Lister les topics
 ./bin/kafka-topics.sh --list --bootstrap-server localhost:9092

```

##### Producteur

```
./bin/kafka-console-producer.sh --topic mon-tunnel-topic --bootstrap-server localhost:9092
```
##### Consomateur   (dans un autre terminal)

```
./bin/kafka-console-consumer.sh --topic mon-tunnel-topic --bootstrap-server localhost:9092
```
Rédemarrez le consumer en ajoutant l'option --from-beginning

```
./bin/kafka-console-consumer.sh --topic mon-tunnel-topic --from-beginning --bootstrap-server localhost:9092
```

Que remarquez-vous ?

### Etape 5 : Groupe de messages, partitions

##### Groupe de messages
Dans Kafka, un groupe représente un ensemble de consommateurs partageant les mêmes offsets, et donc aussi les mêmes partitions et les mêmes topics. Il ne peut pas y avoir deux consommateurs du même groupe sur une même partition. Si le groupe contient plus de consommateurs que de partitions, certains consommateurs ne seront pas utilisés.

Un groupe est identifié par un groupId, qui est une simple chaîne de caractères. Il est déclaré au niveau des consommateurs et il est géré au niveau brokers.

Lancer deux consomateurs dans 2 terminaux différents.
```
./bin/kafka-console-consumer.sh --topic mon-tunnel-topic --bootstrap-server localhost:9092 --consumer-property group.id="message-group-1"
```
Que remarquez-vous ?

##### Partitions

Une partition est une manière de distribuer les données d'un même topic. On précise le nombre de partition par l'option --partitions lors de la création d'un topic kafka-topics.sh --create

```
Créer un topic avec le nombre de partition à 3
./bin/kafka-topics.sh --create  --replication-factor 1 --partitions 3 --topic mon-tunnel-topic-2 --bootstrap-server localhost:9092

```

Un topic peut être composé de plusieurs partitions. Chacune de ces partitions contient des messages différents. Lorsqu'un producer émet un message, c'est à lui de décider à quelle partition il l'ajoute. Les producteurs disposent de plusieurs possibilités pour choisir :

* Aléatoirement : pour chaque message, une partition est choisie au hasard. C'est ce qui est fait par notre kafka-console-producer.
* Round robin : le producer itére sur les partitions les unes après les autres pour distribuer un nombre de message égal sur chaque partition.
* Hashage : le producer peut choisir une partition en fonction du contenu du message.

Lancer un nouveau producer avec le topic créé précedement

```
./bin/kafka-console-producer.sh --topic mon-tunnel-topic-2 --bootstrap-server localhost:9092
```

Lancer deux consommateurs avec le topic créé précedement
```
./bin/kafka-console-consumer.sh --topic mon-tunnel-topic-2 --bootstrap-server localhost:9092 --consumer-property group.id="message-group-2"
```
Que remarquez-vous ?

### Etape 6 : Ajouter des brokers kafka

##### Plusieurs brokers
Notre cluster kafka actuel ne contient qu'un seul broker. Nous sommes loin de mettre en place une plateforme tolérente aux pannes. Pour rémedier à ce problème, nous allons ajouter de nouveaux brokers.  
Pour cela dupliquer le fichier de configuration de kafka. 
Modifier les paramètres broker.id et décommenter la ligne listeners en modifiant le port.
```
cp config/server.properties config/server.1.properties
```
broker.id=1  
listeners=PLAINTEXT://:9093  

##### Nouveau topic

Créer un nouveau topic en précisant les adresses de vos brokers kafka.  
Mettez le facteur --replication-factor à 2  
Mettez le nombre de partitions à 4 --partitions 4  
Decrivez le topic en utilisant le script kafka-topics.sh    
Que remarquez-vous ?  
```
./bin/kafka-topics.sh --create  --replication-factor 2 --partitions 4 --topic mon-tunnel-topic-replica --bootstrap-server localhost:9092,localhost:9093

./bin/kafka-topics.sh --describe --topic mon-tunnel-topic-replica --bootstrap-server localhost:9092,localhost:9093 
```

