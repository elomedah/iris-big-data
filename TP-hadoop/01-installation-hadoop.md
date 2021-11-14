# TP HADOOP

### Installation

#### Pré requis  
Pour installer hadoop nous avons besoin :   
1. java
```
sudo apt-get update
sudo apt-get install openjdk8-jdk -y
java -version 
```
2. ssh 
```
sudo apt install openssh-server openssh-client -y 
```
Vérifier si le service ssh est up
```
service ssh status
```

Si le service est down, redemarrez
```
sudo service ssh start
sudo service ssh --full-restart
```

3. Configurer une clé ssh sans mot de passe
```
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```

#### Hadoop

Téléchargement du binaire hadoop

```
wget https://downloads.apache.org/hadoop/common/stable/hadoop-3.3.1.tar.gz
tar -xzvf hadoop-3.3.1.tar.gz
```

Modifier les variables 

### Variable du shell : bash

Ajouter les commandes hadoop dans le path .....

```
wget https://raw.githubusercontent.com/elomedah/iris-big-data/master/TP-hadoop/bash-hadoop-var.sh
cat bash-hadoop-var.sh >>.bashrc
source .bashrc
```

### Configuration hadoop-env.sh

hadoop-env.sh sert à configurer YARN, HDFS, MapReduce et les paramètres des projets Hadoop.

```
wget https://raw.githubusercontent.com/elomedah/iris-big-data/master/TP-hadoop/hadoop-env-var.properties
cat hadoop-env-var.properties >>$HADOOP_HOME/etc/hadoop/hadoop-env.sh

```

### Configuration core-site.xml

Ce fichier sert à configurer l'url du NameNode et le dossier temporaire dans lequel se feront les calculs MapReduce

```
wget https://raw.githubusercontent.com/elomedah/iris-big-data/master/TP-hadoop/core-site-local.xml
mv core-site-local.xml $HADOOP_HOME/etc/hadoop/core-site.xml
mkdir /home/iris/tmpdata
```

### Configuration  hdfs-site.xml

Permet de définir les dossiers correspondant au NameNode, au DataNode ainsi que le facteur de réplication de HDFS en faisant

```
wget https://raw.githubusercontent.com/elomedah/iris-big-data/master/TP-hadoop/hdfs-site-local.xml
mv hdfs-site-local.xml $HADOOP_HOME/etc/hadoop/hdfs-site.xml
mkdir /home/iris/namenode
mkdir /home/iris/datanode
```

### Configuration  mapred-site.xml 
Il sert à configurer le fonctionnement de mapreduce

```
wget https://raw.githubusercontent.com/elomedah/iris-big-data/master/TP-hadoop/mapred-site-local.xml
mv mapred-site-local.xml $HADOOP_HOME/etc/hadoop/mapred-site.xml
```
### Configuration yarn-site.xml

Il sert à configurer le gestionnaire de ressources

```
wget https://raw.githubusercontent.com/elomedah/iris-big-data/master/TP-hadoop/yarn-site-local.xml
mv yarn-site-local.xml $HADOOP_HOME/etc/hadoop/yarn-site.xml
```

### Formattage du NameNode
```
hdfs namenode -format
```

### Démarrage du cluster
Démarrez les NameNode et DataNode avec la commande
```
$HADOOP_HOME/sbin/start-dfs.sh

```

Pour démarrer YARN, la commande est
```
$HADOOP_HOME/sbin/start-yarn.sh

```
### Vérification

L'interface du namenode http://localhost:9870    
L'interface du datanode  http://localhost:9864    
L'interface yarn (ressource manager) : http://localhost:8088   
L'interface yarn (node manager) : http://localhost:8042   
