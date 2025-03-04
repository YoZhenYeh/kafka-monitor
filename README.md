# Kafka Monitor with Prometheus and Grafana
This project sets up a 3-node Kafka cluster using KRaft mode, complete with Prometheus and Grafana for monitoring, and performance testing scripts.

## Prerequisites
1. Docker and Docker Compose
2. jmx_prometheus_javaagent-0.3.1.jar (https://github.com/prometheus/jmx_exporter/tree/parent-0.3.1)

## File Structure
```
kafka-monitor/
├── docker-compose.yml
├── prometheus.yml
├── jmx-exporter/
│   └── jmx_prometheus_javaagent-0.3.1.jar
│   └── kafka-broker.yml
├── KafkaMonitorGrafana.json
└── README.md
```

## Getting Started

### Clone the Repository

```
git clone https://github.com/YoZhenYeh/kafka-monitor
cd kafka-monitor
```

### Start the Kafka Cluster

```
docker-compose up -d
```

### Create the Test Topic
```
docker exec -it kafka1 kafka-topics.sh \
  --create \
  --topic test-topic \
  --bootstrap-server kafka1:9094 \
  --partitions 3 \
  --replication-factor 3
```

### Test Producer Message 
Use Kafka's built-in performance testing tool to produce messages:

```
docker exec -it kafka1 kafka-producer-perf-test.sh \
  --topic test-topic \
  --num-records 100000 \
  --record-size 1000 \
  --throughput -1 \
  --producer-props bootstrap.servers=kafka1:9094
```

### Test Consumer Message
Use Kafka's built-in performance testing tool to consume messages:

```
docker exec -it kafka1 kafka-consumer-perf-test.sh \
  --bootstrap-server kafka1:9094 \
  --topic test-topic \
  --messages 100000 \
```

### Access Monitoring Dashboards
* Grafana: http://localhost:3000 (user: admin, password: admin)
* Prometheus: http://localhost:9090



