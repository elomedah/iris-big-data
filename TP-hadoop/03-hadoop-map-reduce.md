
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

Nous avons écrit des codes pour le mappeur et le réducteur en script python pour l'exécuter sous Hadoop

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
```


#### Copier un fichier local sur hdfs



#### Exécuter le job map reduce
```
 hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.3.1.jar \
 -files mapper.py,reducer.py   \
 -mapper mapper.py  \
 -reducer reducer.py \
 -input /input-01 \
 -output /output-01
```


