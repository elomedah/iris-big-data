### Mongodb sharding (distribution) configuration

Le sharding consiste à distribuer des données sur plusieurs machines. MongoDB utilise le sharding pour gérer les déploiements impliquant des volumes de données importants et des opérations à hautes performances.

La mise à l'échelle horizontale, également appelée scale-out, fait référence à l'ajout de machines pour partager l'ensemble de données et la charge. La mise à l'échelle horizontale permet une mise à l'échelle presque illimitée pour gérer les big data et les charges de travail intenses.

Documentation officielle : https://www.mongodb.com/resources/products/capabilities/sharding#:~:text=MongoDB%20employs%20sharding%20to%20handle,big%20data%20and%20intense%20workloads

## Sharded cluster architecture

Un cluster distribué MongoDB est un ensemble de : 
- Shard.
- Mongos instance.
- Config server replica set.

### Shard 

Un shard est un ensemble de replica set qui contient un sous-ensemble des données du cluster.
Une fois le partitionnement activé, ce qui se produit lorsqu'une seule instance MongoDB n'est pas en mesure de gérer le grand ensemble de données,
MongoDB divise les collections souhaitées en plusieurs shards pour obtenir une mise à l'échelle horizontale.
Chaque shard possède un shard principal et un ou plusieurs shards secondaires. Le shard principal est responsable des écritures et les réplique sur les shards secondaires.

### Mongos instance

L'instance mongos agit comme un routeur de requêtes (query router) pour les applications clientes, gérant à la fois les opérations de lecture et d'écriture.
Elle distribue les requêtes client aux shards concernés et regroupe le résultat des shards dans une réponse client cohérente.
Les clients se connectent à un mongos, et non à des shards individuels.

### Config server replica set

Un Config server replica set se compose de plusieurs membres de replica set MongoDB. Ils constituent la source officielle des métadonnées de partitionnement.
Les métadonnées de partitionnement reflètent l'état et l'organisation des données partitionnées. Les métadonnées contiennent la liste des collections partitionnées, les informations de routage, etc.


![image](https://github.com/user-attachments/assets/1fef01cd-0276-4a50-b27f-a50496f03710)
