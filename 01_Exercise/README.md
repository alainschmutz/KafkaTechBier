# Exercise setup

1. **Start the VM**

2. **Log on**

# Exercise 1

**Run the [landoop/fast-data-dev](https://github.com/Landoop/fast-data-dev) docker image**

```
docker run --rm -it -p 2181:2181 -p 3030:3030 -p 8081:8081 -p 8082:8082 -p 8083:8083 -p 9092:9092 -p 9581:9581 -p 9582:9582 -p 9583:9583 -p 9584:9584 -e ADV_HOST=127.0.0.1 -e RUNTESTS=0 landoop/fast-data-dev:latest
```
_Note: This terminal MUST NOT be closed. If you close it, the Kafka-instance will stop._

1. Wait ~1 minute, so Kafka can start up properly
1. Now open firefox
1. Open the bookmark "Kafka UI" in the bookmarks toolbar

**Open a new terminal, again make user to sudo and enter the password**

`Terminal: File -> Open Terminal`

```
sudo su
```

**Enter the container bash in this new terminal**

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

_Info: There are some other dummy topics, which were created by the COYOTE HEALTH CHECKS_

**Produce to the topic using console producer**
```
kafka-console-producer --broker-list localhost:9092 --topic topic1
```
Data will now produced to `topic1`

**Consume from the topic using console consumer**

Open a new terminal, again make user to sudo and enter the password

`Terminal: File -> Open Terminal`

```
sudo su
```

Again enter the container bash in this new terminal

```
docker run --rm -it --net=host landoop/fast-data-dev bash
```

Finally, your data of `topic1` will be shown by typing

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic topic1 --from-beginning
```

You can now switch over to the producer terminal, produce some data, switch to the consumer terminal and see that data is going to be processed


**Browse the Kafka UI** 

... and play a little bit around 

# Exercise 2
**Setup**

Run [landoop/fast-data-dev](https://github.com/Landoop/fast-data-dev) docker image. For example:

```
docker run --rm -it -p 2181:2181 -p 3030:3030 -p 8081:8081 -p 8082:8082 -p 8083:8083 -p 9092:9092 -p 9581:9581 -p 9582:9582 -p 9583:9583 -p 9584:9584 -e ADV_HOST=127.0.0.1 landoop/fast-data-dev:latest
```

Use same topic

Specify key and value to be written

Read key and value

New topic with 2 partitions

Write more data and see what happens to order


