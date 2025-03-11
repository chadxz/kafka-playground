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
