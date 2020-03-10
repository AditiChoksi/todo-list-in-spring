# Todo List example application


To-do list using spring

## Todo list architecture

The Todo List application is built using

* Java
* JPA
* Eventuate Tram
* Spring Boot
* MySQL
* ElasticSearch
* Apache Kafka

The application consists of two services:

* `Todo Service` - implements the REST endpoints for creating, updating and deleting todos.
The service persists the Todo JPA entity in MySQL.
Using `Eventuate Tram`, it publishes Todo domain events that are consumed by the `Todo View Service`.

* `Todo View Service` - implements a REST endpoint for querying the todos.
It maintains a CQRS view of the todos in ElasticSearch.

The `Todo Service` publishes events using Eventuate Tram.
Eventuate Tram inserts events into the `MESSAGE` table as part of the ACID transaction that updates the TODO table.
The Eventuate Tram CDC service tracks inserts into the `MESSAGE` table using the MySQL binlog and publishes messages to Apache Kafka.
The `Todo View Service` subscribes to the events and updates ElasticSearch.


# How it works

The Todo application uses the [Eventuate Tram framework](https://github.com/eventuate-tram/eventuate-tram-core) to publish and consume domain events.