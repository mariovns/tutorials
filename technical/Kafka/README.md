# Kafka

Kafka® is used for building real-time data pipelines and streaming apps. It is horizontally scalable, fault-tolerant, wicked fast, and runs in production in thousands of companies.

## why kafka

### efficiency
* central repository : all nodes need to read/write to a single point
* fast writing : inbuilt faster writing
* audit log : who did, when did, what
* bootstrapping a new app : new apps data can be readily shared among older apps

### implication of moving to Kafka
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
Producers create an Event when it produces data for Kafka cluster.
Logs are continuos entries of these changes/events to a particular category.
Log becomes true source of data, any changes is reflected into Log instantly.

### Topic
Feed of changes.
