# 🧠 SQS vs. SNS vs. Kinesis: The Messaging and Streaming Showdown (Learning Guide)

Understanding the fundamental differences between **SQS, SNS, and Kinesis** is crucial for designing event-driven and decoupled architectures on AWS. This guide summarizes their core models, guarantees, and typical use cases.

---

## ⚖️ Core Differences: A Side-by-Side Comparison

| Feature | Amazon SQS (Simple Queue Service) | Amazon SNS (Simple Notification Service) | Amazon Kinesis Data Streams (KDS) |
| :--- | :--- | :--- | :--- |
| **Architectural Model** | **Queue** (Point-to-Point, 1:1 or 1:Many Workers) | **Pub/Sub** (Publish-Subscribe, 1:Many Subscribers) | **Streaming Data Platform** (Log/Stream, 1:Many Consumers) |
| **Data Flow** | **Pull-Based:** Consumers **pull** and delete data. | **Push-Based:** SNS **pushes** data to subscribers. | **Pull-Based** (Standard) or **Push-Based** (Enhanced Fan-Out). |
| **Data Persistence** | **Yes.** Messages are stored until processed/deleted or expire (1-14 days). | **No.** Data is **not persistent**; if not delivered, it may be lost. | **Yes.** Data is stored and replayable (1-365 days). |
| **Consumption** | **One-Time:** Each message is processed and **deleted** by *one* consumer group. | **Broadcast:** Every subscriber receives a **copy** of the message. | **Replayable:** Multiple consumers read the **same data** from different offsets. |
| **Throughput & Scaling** | **Fully Managed;** scales automatically to hundreds of thousands of messages. **No provisioning needed** (Standard Queue). | **Fully Managed;** scales automatically to hundreds of thousands of topics/messages. **No provisioning needed**. | **Provisioned** capacity via **Shards** or **On-Demand** mode. Throughput must be managed. |
| **Ordering Guarantee** | **Only with FIFO queues** (by Message Group ID). | **No** (Standard Topic). | **Yes**, guaranteed **at the Shard level**. |
| **Key Use Case** | Decoupling applications, **task queues**, buffering database writes. | **Broadcasting** events/notifications, Fan-Out to multiple endpoints. | **Real-time Big Data** analytics, ETL, log/clickstream collection, data replay. |

---

## 📦 Amazon SQS: The Task Queue

SQS is your go-to for **delegating work** and **decoupling** asynchronous tasks.

* **Consumer Model:** Workers **pull** messages from the queue.
* **Message State:** Messages are **deleted** after successful processing.
* **Features:**
    * **Message Delay:** Messages can be configured to appear to consumers after a set delay (e.g., 30 seconds).
    * **FIFO:** Provides strict ordering and exactly-once processing guarantees, but has throughput limits.

---

## 📢 Amazon SNS: The Notification Hub

SNS excels at **broadcasting events** to many different subscribers simultaneously. 

* **Delivery Model:** **Pub/Sub**; one push to the topic sends a copy to all subscribers.
* **Fan-Out Pattern:** Most commonly combined with **SQS queues** to provide message persistence and durability for the receiving services.
* **Scaling:** Supports up to **12.5 million+ subscribers** per topic.
* **SNS FIFO:** Can be combined with **SQS FIFO** queues to enforce order across the broadcast.

---

## 📊 Amazon Kinesis: The Real-Time Stream

Kinesis Data Streams is built for **high-throughput, real-time data ingestion and analytics**, where data persistence and replayability are mandatory.

* **Data Persistence:** Data is durable and retained for **1 to 365 days**, allowing consumers to re-read or replay the stream.
* **Ordering:** Ordering is guaranteed **within a shard** (based on Partition ID).
* **Consumption Modes:**
    * **Standard:** Consumers **pull** data (2 MB/s per shard).
    * **Enhanced Fan-Out (EFO):** Kinesis **pushes** data, providing dedicated throughput of 2 MB/s **per shard per consumer**. This enables highly parallel reading.
* **Capacity:** You must either **provision Shards** manually or use **On-Demand** mode for automatic scaling.
