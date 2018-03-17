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
