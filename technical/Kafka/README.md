# Kafka

Kafka® is used for building real-time data pipelines and streaming apps. It is horizontally scalable, fault-tolerant, wicked fast, and runs in production in thousands of companies.

## Embracing Kafka

### Efficiency
* central repository : all nodes need to read/write to a single point
* fast writing : inbuilt faster writing
* audit log : who did, when did, what
* bootstrapping a new app : new apps data can be readily shared among older apps

### Implication of moving to Kafka
* retooling : a new service is required if we are shifting to kafka based system
* changing app-architecture : applications need to be modified to fit with the needs of the new Kafka layer
* whole-scale investment : people needs to be trained on the new tool
* maintainability : maintenance cost will come

## Concepts

* Producer : providing input to kafka cluster
* Consumer : receiving data from Kafka Clusters
* Connectors : allows us to integrate with RDBMS
* Stream Processors : handle the data as coming in
* Broker : logic separation of task, may provide multiple backup, can be on different m/c
* Topic : category of changes, handled by a Broker, replica can spread across different broker
* Log : 

### Log
Producers create an Event or change when it produces data for Kafka cluster.
Logs are continuos entries of these changes/events to a particular category.
Log becomes true source of data, any changes to any of its data-source/connector is reflected into Log instantly.

### Topic
* Feed of changes.
* partitioned
* immutable : each change will be saved progressively in each replica
different log have different topic, you can strategize different way of reading separate partition of topic.

### Broker
Data write happens in lead partition, partition 0
Logical separation of partitions of topics.
Each topic is replicated across different brokers as a different partition but is lead in only 1 of those broker. Usually we can keep no. of topics = no. of brokers

### Producers
* Publishes data to Topics
* uses Partitioner : which partition is current lead for the topic and which broker, whether data has been replicated and recieved successfully
* replication complete before read
* throughput configurations: 
	* message durability : you can send response only after replication is done or otherwise
	* ordering/retries : if there is any issue with any write, you want to retry for it to get corrected, how many retry, 
	* batching & compression : maximize this is the required
	* queuing limit : depends on Java Memory, so tune it accordingly
### Consumers
* read data from topics
* organized into groups 
* have all the partition divide among each consumers
* needs to be rebalanced as new consumer comes and go which is done by zookeeper
* broker coordinator (newer version) does same thing as zookeper.
* The Kafka cluster durably persists all published records—whether or not they have been consumed—using a configurable retention period
* The only metadata retained on a per-consumer basis is the offset or position of that consumer in the log
* configurations
	* group id
	* session timeout -default 30s
	* heartbeat : each consumer will have, in absence zookeeper will re-balance
	* auto-commit : default 5s

### Typical Operations

* Adding/ Removing Topics: bin/kafka-topics.sh --zookeeper <host:port> --create --topic <topic_name> --partition<count> --repication-factor <count> --config <x=y>
* Adding Partitions :  bin/kafka-topics.sh --zookeeper <host:port> --alter --topic <topic_name> --partition<count> OR --repication-factor <count> OR --config <x=y> OR --delete-config <x>
* Delete A Topic : delete.topic.enable=true bin/kafka-topics.sh --zookeeper <host:port> --delete --topic <topic_name>
* consumer position check : bin/kafka-run-class.sh kafka.tools.ConsumerOffsetChecker --zookeeper <host:port> --greoup <group_name>
* configure consumer group : bin/kafka-consumer-groups.sh --bootstrap-server <broker:port> --describe --group <group_name>

### Monitoring in Kafka

### Auditing
all messages have been handled properly.
#### Kafka Audit
multiple data-centers, mirror all the messages from 1 DC to 2nd DC, another DC will check if all had been mirrored successfully or not.
#### Chaperone
Used by Uber. All Regional Kafka Clusters have their own CHaperone Service which will read the audit info like who send the message, the time stamp, etc. These messages from all regional clusters will be forwarded to aggregate kafka cluster which will have its own Chaperone service which will then order the messages and help to keep messages in sync. then chaperone collector will come into picture.

#### Confluent Control Center
Prepared by Confluent
