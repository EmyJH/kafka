# linux 
```linux
wget http://apache.mindstudios.com/kafka/1.0.1/kafka_2.11-1.0.1.tgz
tar -xf kafka_2.11-1.0.1.tgz
cd /mnt/c/Users/Fitec/Downloads/Kafka/kafkabin/kafka_2.11-1.0.1

bin/zookeeper-server-start.sh config/zookeeper.properties ok

bin/kafka-server-start.sh config/server.properties

bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

kafka-topics.sh --list --zookeeper localhost:2181

bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning --consumer.config config\consumer_1.properties
```

# windows
log.dirs=c:/kafka/kafka-logs dans config server à la place de /tmp/kafka-logs
dataDir=c:/kafka/kafka-dataDir dans zookeeper /tmp/zookeeper

Shell zookeeper:
```windows
C:\kafka_2.11-1.0.1>C:\kafka_2.11-1.0.1\bin\windows\zookeeper-shell.bat
```
Zookeeper:
```windows
bin\windows\zookeeper-server-start.bat config\zookeeper.properties
```

Serveur / broker:
```
bin\windows\kafka-server-start.bat config\server.properties &
```

Create topic:
```
bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```

Vérifier la présence de topics:
```
bin\windows\kafka-topics.bat --list --zookeeper localhost:2181
```
Description du topic:
```
bin\windows\kafka-topics.bat --describe --zookeeper 54.229.212.201 --topic julietest2
```
Créer un producteur:
```
bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic test
```

Créer un consommateur:
```
bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning
```

# AWS
pour ajouter broker need créer un nouveau fichier server properties
id-broker=2

```
bin/kafka-topics.sh --create --zookeeper 54.229.212.201 --replication-factor 2 --partitions 5 --topic julietest2
Created topic "julietest"
```

```
bin/kafka-topics.sh --list --zookeeper 54.229.212.201
ATLAS_ENTITIES
ATLAS_HOOK
__consumer_offsets
julietest
julietest2
test2
```

```
bin/kafka-console-producer.sh --broker-list ip-172-31-23-171.eu-west-1.compute.internal:6667 --topic julietest2
bin/kafka-console-producer.sh --broker-list 54.171.152.221:6667 --topic julietest2
```

```
bin/kafka-console-consumer.sh --bootstrap-server 54.229.212.201:6667 --topic julietest2 --from-beginning
```

```
bin/kafka-topics.sh --describe --zookeeper 54.229.212.201 --topic julietest2
```

# connect
start du zookeeper et du broker
```
bin\windows\zookeeper-server-start.bat config\zookeeper.properties
bin\windows\kafka-server-start.bat config\server.properties
```

dans connect-file-source.properties:
file=c:/Users/Fitec/Documents/kafka/tes1.txt

dans connect-file-sink.properties:
file=c:/Users/Fitec/Documents/kafka/test.sink.txt

dans standalone properties :
offset.storage.file.filename=c:/kafka/connect.offsets à la place de tmp/connect.offsets
```
bin\windows\connect-standalone.bat config\connect-standalone.properties config\connect-file-source.properties config\connect-file-sink.properties
```
verification de la creation du topic connect
```
bin\windows\kafka-topics.bat --list --zookeeper localhost:2181
__consumer_offsets
connect-test
test
```
lecture du topic
```
bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic connect-test --from-beginning
{"schema":{"type":"string","optional":false},"payload":"coucou"}
{"schema":{"type":"string","optional":false},"payload":"test 1"}
```
## ajout de ligne dans le fichier
on écrit une nouvelle ligne
```
C:\Users\Fitec\Documents\kafka>echo Another line>> tes1.txt
```
on lit le fichier sink
```
C:\Users\Fitec\Documents\kafka>more test.sink.txt
coucou
test 1
Another line
```


