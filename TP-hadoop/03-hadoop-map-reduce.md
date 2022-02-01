
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
Create file mapper.py
```
#!/usr/bin/python3
import sys

# input comes from STDIN (standard input)
for line in sys.stdin:
    # remove leading and trailing whitespace
    line = line.strip()
    # split the line into words
    words = line.split()
    # increase counters
    for word in words:
        print("{}\t{}".format(word, "1"))
                                          
```


#### Reducer
Create file reducer.py
```
#!/usr/bin/python3
import sys

current_word = None
current_count = 0
word = None

# input comes from STDIN
for line in sys.stdin:
    # remove leading and trailing whitespace
    line = line.strip()

    # parse the input we got from mapper.py
    word, count = line.split('\t', 1)

    # convert count (currently a string) to int
    try:
        count = int(count)
    except ValueError:
        # count was not a number, so silently
        # ignore/discard this line
        continue

    # this IF-switch only works because Hadoop sorts map output
    # by key (here: word) before it is passed to the reducer
    if current_word == word:
        current_count += count
    else:
        if current_word:
            # write result to STDOUT
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


