# kafka-playground

Scratch files for learning about Kafka and the Confluent Platform.

## about

The docker-compose.yml file includes [Kafka](https://kafka.apache.org/) (port 9092),
[Confluent Schema Registry](https://github.com/confluentinc/schema-registry) (port 8081),
[Confluent REST Proxy](https://github.com/confluentinc/kafka-rest) (port 8082),
[Kafka Connect](https://kafka.apache.org/documentation/#connect) (port 8083),
and [ksqlDB](https://github.com/confluentinc/ksql) (port 8088).

There's only one instance of everything, so this is only for tinkering.
In production, you'd probably want to use a managed hosting provider like
[Confluent](https://www.confluent.io/) or [RedPanda](https://www.redpanda.com/),
or run it with [Confluent for Kubernetes](https://github.com/confluentinc/confluent-kubernetes-examples)
or [Strimzi](https://strimzi.io/) in Kubernetes.

## setup

1. Install [Docker](https://docs.docker.com/get-started/get-docker/).
2. `docker compose up`
3. Poke it with a stick.

## learning resources

I'm referencing these resources for learning more about Kafka:

- Stephane Maarek's Conduktor course https://learn.conduktor.io/kafka/
- Chad Crowell's Kafka Deep Dive course https://app.pluralsight.com/ilx/apache-kafka-deep-dive/table-of-content
- Will Boyd's CCDAK course https://app.pluralsight.com/ilx/confluent-certified-developer-for-apache-kafka-(ccdak)/table-of-content

## Interesting things

### making requests to the REST Proxy

The [Confluent REST Proxy](https://docs.confluent.io/platform/current/kafka-rest/index.html)
is a REST API for interacting with Kafka. Some example requests can be found in
[admin-requests.http](admin-requests.http). The server is listening on port 8082.

### checking Kafka connectivity

An easy way to check that Kafka is working is with the `kcat` command line tool.

```shell
brew install kafkacat
```

Then you can make a request for metadata using `-L`:

```shell
kcat -b localhost:19092 -L
Metadata for all topics (from broker -1: localhost:19092/bootstrap):
 3 brokers:
  broker 1 at broker1:9092
  broker 2 at broker2:9092 (controller)
  broker 3 at broker3:9092
 8 topics:
  topic "docker-connect-status" with 5 partitions:
    partition 0, leader 3, replicas: 3, isrs: 3
    ...    
```

### Using binaries inside broker container

```shell
docker exec -ti broker1 bash

$ kafka-<TAB><TAB>
kafka-acls                        kafka-log-dirs
kafka-broker-api-versions         kafka-metadata-quorum
kafka-client-metrics              kafka-metadata-shell
kafka-cluster                     kafka-mirror-maker
kafka-configs                     kafka-preferred-replica-election
kafka-console-consumer            kafka-producer-perf-test
kafka-console-producer            kafka-reassign-partitions
kafka-consumer-groups             kafka-replica-verification
kafka-consumer-perf-test          kafka-run-class
kafka-delegation-tokens           kafka-server-start
kafka-delete-records              kafka-server-stop
kafka-dump-log                    kafka-storage
kafka-e2e-latency                 kafka-streams-application-reset
kafka-features                    kafka-topics
kafka-get-offsets                 kafka-transactions
kafka-jmx                         kafka-verifiable-consumer
kafka-leader-election             kafka-verifiable-producer

$ kafka-topics --bootstrap-server localhost:9092 --create \
    --topic test --partitions 3 --replication-factor 3
Created topic test.

$ kafka-topics --bootstrap-server localhost:9092 --topic test --describe
Topic: test     TopicId: mnuRC-6PS8mxvRQK24CBbA PartitionCount: 3       ReplicationFactor: 1  Configs:
        Topic: test     Partition: 0    Leader: 1       Replicas: 1     Isr: 1  Elr:    LastKnownElr:
        Topic: test     Partition: 1    Leader: 1       Replicas: 1     Isr: 1  Elr:    LastKnownElr:
        Topic: test     Partition: 2    Leader: 1       Replicas: 1     Isr: 1  Elr:    LastKnownElr:
        
$ kafka-console-producer --bootstrap-server localhost:9092 --topic test
>message1
>message2
>message3
>^C

$ kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning
message1
message2
message3
^CProcessed a total of 3 messages

$ exit
```
