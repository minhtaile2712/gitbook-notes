# Kafka

Deployed: bare-metal hardware, virtual machines, containers in on-premises or in the cloud.

Kafka: distributed system consisting of servers, clients; communicating over TCP.

Servers:

* Some form the storage layer, called `brokers`
* Other run `Kafka Connect` to continuously import and export data as event streams.

Clients: read, write, process streams of events.

Event:

* Is a record or message
* Has `key`, `value`, `timestamp`, `metadata` (optional)
* Organized & storaged in **topics**.
* with the same key are written to the same partition.

Producers: publish (write) events to Kafka.

Consumers: subscribe to (read & process) events.

Topics are partitioned: spread over a number of "buckets" located on different `brokers`.

