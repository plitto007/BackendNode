# KAFKA
## Listen for a topic
```
kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic [topic-name] --from-beginning
```

## List all topic
```
kafka-topics.sh --list --zookeeper localhost:2181
```