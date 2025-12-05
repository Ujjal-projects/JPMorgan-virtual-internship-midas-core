# Midas
Project repo for the JPMC Advanced Software Engineering Forage program

During this internship project, the goal was to integrate Apache Kafka into the Midas Core backend to enable asynchronous communication between frontend and backend services. Kafka acts as a message queue, helping the system achieve better decoupling, scalability, and reliability when handling financial transactions.

✅ Task 1 – Kafka Dependency Setup

What I added:

Dependencies (pom.xml)
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka-test</artifactId>
    <scope>test</scope>
</dependency>

✅ Task 2 – Kafka Listener Integration
1️⃣ Created the TransactionListener class
package com.jpmc.midascore.component;

import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.stereotype.Component;
import com.jpmc.midascore.foundation.Transaction;

@Component
public class TransactionListener {

    @KafkaListener(topics = "${kafka.topic.name}", groupId = "midas-core")
    public void listen(Transaction transaction) {
        System.out.println("Received transaction: " + transaction);
    }
}

2️⃣ Modified application.yml
spring:
  kafka:
    consumer:
      group-id: midas-core
      auto-offset-reset: earliest
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
      properties:
        spring.json.trusted.packages: "*"

    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer

kafka:
  topic:
    name: midas-transactions

📤 Output from TaskTwoTests
Received transaction: Transaction {senderId=6, recipientId=7, amount=122.86}
Received transaction: Transaction {senderId=5, recipientId=2, amount=42.87}
Received transaction: Transaction {senderId=7, recipientId=4, amount=161.79}
Received transaction: Transaction {senderId=8, recipientId=7, amount=22.22}
