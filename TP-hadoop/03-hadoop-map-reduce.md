
# Map Reduce

L'objectif de ce TP est d'executer un job map reduce sur votre cluster local Hadoop.

### Pré requis

* Faire le TP 1 Hadoop (Installer hadoop)


### Hadoop streaming

Le streaming Hadoop est un utilitaire fourni avec la distribution Hadoop. Cet utilitaire vous permet de créer et d'exécuter des tâches Map/Reduce avec n'importe quel exécutable ou script en tant que mappeur et/ou réducteur.   
https://hadoop.apache.org/docs/stable/hadoop-streaming/HadoopStreaming.html    

### Word count example avec Hadoop streaming

Tout job dans Hadoop doit comporter deux phases :
*  mappeur 
*  réducteur   

Nous allons écrire le mappeur et le réducteur en script python pour l'exécuter sous Hadoop

#### Mapper
Créer le fichier mapper.py
```
#!/usr/bin/python3
import sys

# Les données en provenance de l'entrée standard (standard input)
for line in sys.stdin:
    # Effectuer un nettoyage 
    line = line.strip()
    # Récuperer les mots en splittant
    words = line.split()
    # Afficher chaque mot avec le compteur 1
    for word in words:
        print("{}\t{}".format(word, "1"))
                                          
```


#### Reducer
Créer le fichier reducer.py
```
#!/usr/bin/python3
import sys

current_word = None
current_count = 0
word = None

# Les données en provenance de l'entrée standard (standard input)
for line in sys.stdin:
    # Effectuer un nettoyage 
    line = line.strip()

    # parser les données afficher à l'aide du mapper.py
    word, count = line.split('\t', 1)

    # Pour pouvoir incrémenter nous allons convertir en entier les strings count
    try:
        count = int(count)
    except ValueError:
        # Gérer les exceptions. Dans notre cas on ignore simplement la ligne
        continue

    # Les données sont par défaut trier par hadoop (shuffling)
    if current_word == word:
        current_count += count
    else:
        if current_word:
            print(current_word, current_count)
        current_count = count
        current_word = word
print(current_word, current_count)	

```

#### Installer python3 pour tester le script

```
sudo apt-get update
sudo apt-get install python3.6
```

#### Créer un fichier local 

Créer le fichier texte en mettant le contenu file.txt
```
tout est tout
tout bon
tout est mauvais
```

Exemple de commande :

```
echo "tout est tout
tout bon
tout est mauvais" > file.txt
```

#### Tester les scripts avec l'exemple du cours

```
cat file.txt | python3 mapper.py | sort | python3 reducer.py
```


#### Copier un fichier local sur hdfs

Créer un repertoire input-01 sur hdfs

Copier le fichier local file.txt dans le repertoire input-01


#### Exécuter le job map reduce
```
 hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.3.1.jar \
 -files mapper.py,reducer.py   \
 -mapper mapper.py  \
 -reducer reducer.py \
 -input /input-01 \
 -output /output-01
```

Après exécution le resultat sera dans le repertoire output-01.   
Vérifier que le résultat est le même que précedemment.

#### Exécution en mode batch vs hadoop Perf

Nous avons utilisé hadoop Map reduce pour faire un traitement que nous pouvions faire simplement sur linux.
Pour voir l'impact d'utilisation d'hadoop nous allons exécuter le programme sur un texte volumineux.


