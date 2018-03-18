# ipt Tech-Bier Stream Processing mit Apache Kafka
20. März 2018, Hotel Crowne Plaza Zürich



![Image of Kafka](https://www.codecentric.de/files/2015/12/logo.png)


# Exercise setup

1. **Start the VM**

2. **Log on**

# Exercise 1

**Run the [landoop/fast-data-dev](https://github.com/Landoop/fast-data-dev) docker image**

```
  docker run -d --rm --net=host --name kafka-dev-env -e RUNTESTS=0 landoop/fast-data-dev:latest
```
_Note: The container will now start running in the background_

1. Wait ~1 minute, so Kafka can start up properly
1. Now open firefox
1. Open the bookmark "Kafka UI" in the bookmarks toolbar

**Enter the container bash in the terminal**

```
docker run --rm -it --net=host landoop/fast-data-dev bash
```

_Now inside the container..._

**Create a New Topic called "topic1"**
```
kafka-topics --zookeeper localhost:2181 --create --topic topic1 --replication-factor 1 --partitions 3
```
_Info: By only typing `kafka-topics` you'll see a list of all the arguments_

**List all topics**
```
kafka-topics --zookeeper localhost:2181 --list
```
You'll now see your created topic `topic1`.

You can also view your topics by using the Kafka UI

1. Go to the Kafka UI tab in firefox 
1. Enter _TOPICS_
1. Your topics will be listed on the left
1. Click `topic1`

_Info: There are some other topics, which were created by the landoop image_

**Produce to the topic using console producer**
```
kafka-console-producer --broker-list localhost:9092 --topic topic1
```
Data will now produced to `topic1`

Now type things/messages in and press enter. Each line will then be pushed to Kafka with the message you just typed.

If you now go to your `topic1` in the Kafka UI, you'll notice that data is beeing assigned to random partitions. That is because we are not using a key, so the key is set to null. Also keep in mind, without providing a key there's no ordering!

_Info: Console producer can be exited by pressing CTRL + D (end of file in Linux)_

**Consume from the topic using console consumer**

Open a new terminal

`Terminal: File -> Open Terminal`

Again enter the container bash in this new terminal

```
docker run --rm -it --net=host landoop/fast-data-dev bash
```

Finally, your data of `topic1` will be shown by typing

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic1
```

You can choose either if you want to print data from now or from the beginning, so all data of a topic is beeing printend.

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic1 --from-beginning
```

You can now switch over to the producer terminal, produce some data, switch to the consumer terminal and see that data is going to be processed. Also you can check the data of the topics by using the Kafka UI.

_Info: Console consumer can be stopped by pressing CTRL + C_


**Browse the Kafka UI** 

... take a look at some other topics and play a little bit around 

# Exercise 2

If we provide a key, Kafka will take care that data with the same key is beeing stored in the same partition.

**Use the same topic**

`topic1`

**Specify key**

```
kafka-console-producer --broker-list localhost:9092 --topic topic1 --property "parse.key=true" --property "key.separator=:"
```

**Again produce some data**

f.e.:

```
key1:data1
key1:data2
key1:data3
key2:data4
key3:data5
key3:data6
```

**Read key and value using console consumer**


```
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic1 --from-beginning --property print.key=true
```

You can also print the timestamp by typing

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic1 --from-beginning --property print.key=true --property print.timestamp=true
```

**Go to the Kafka UI**

On `topic1` you'll now see, that the data we just provided have a key. To the right of each data you'll see _Offset:_ and _Partition:_. You'll now notice that data with the same key is beeing stored on the same partition with incrementing offset.

**Now by yourself**

1. Create a new topic with 2 partitions
2. Write more data and see what happens to order

Kafka Java Producer: https://github.com/alainschmutz/KafkaTechBier_JavaProducer
Kafka Streams: https://github.com/fabiohecht/techbierkafkastreamsexercise
