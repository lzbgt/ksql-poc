https://docs.ksqldb.io/en/latest/how-to-guides/use-connector-management/

https://stackoverflow.com/questions/31746182/docker-compose-wait-for-container-x-before-starting-y


https://github.com/bitnami/bitnami-docker-kafka/issues/68

```
version: "2.1" # Note you should use 2.1 or up

services:
  zookeeper:
    image: "bitnami/zookeeper:3"
    ports:
      - "2181:2181"
    volumes:
      - "zookeeper_data:/bitnami"
    environment:
      - ALLOW_ANONYMOUS_LOGIN=yes
  kafka:
    image: "bitnami/kafka:2"
    ports:
      - "9092:9092"
    volumes:
      - "kafka_data:/bitnami"
    environment:
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
      - ALLOW_PLAINTEXT_LISTENER=yes
    depends_on:
      - zookeeper
    healthcheck:
      test:
        ["CMD", "kafka-topics.sh", "--list", "--zookeeper", "zookeeper:2181"]
      interval: 30s
      timeout: 10s
      retries: 4

volumes:
  zookeeper_data:
    driver: local
  kafka_data:
    driver: local
```