# 🌊 Amazon Kinesis Data Streams (KDS): Real-Time Data Collection (Learning Guide)

This guide provides an overview of **Amazon Kinesis Data Streams (KDS)**, AWS's core service for collecting and storing **real-time streaming data**.

---

## 🚀 Core Concept: The Real-Time Data Pipeline

Kinesis Data Streams is designed to act as a highly scalable, durable buffer for continuous, high-volume data feeds. The key differentiator is the focus on **real-time** processing.

### **Data Flow Architecture**

1.  **Producers:** Send data into KDS as it's created.
    * **Sources:** Clickstreams, IoT device data (connected bicycles), server logs, and application metrics.
    * **Tools:** Custom **applications** (code you write) or the **Kinesis Agent** (for logs/metrics on servers).
2.  **Kinesis Data Streams (KDS):** Persistently stores the data stream.
3.  **Consumers:** Applications or services that read and process the data in real time.
    * **Examples:** Lambda functions, custom consumer applications, **Kinesis Data Firehose** (for delivery), and **Managed Service for Apache Flink** (for analytics).


### **Key Features**

* **Data Retention:** Data is retained on the stream for up to **365 days**, allowing consumers to **reprocess or replay** data.
* **Data Immutability:** Once data is sent into KDS, it **cannot be deleted**—you must wait for the retention period to expire.
* **Record Size:** Data records can be up to **1 megabyte (MB)** in size.
* **Security:** Features **HTTPS encryption** for data in flight and **KMS encryption** for data at rest.

---

## 📐 Ordering and Scaling with Shards

The scalability and ordering of KDS are managed through its fundamental unit of capacity: the **Shard**.

### **1. Data Ordering**

* Data is guaranteed to be **in order** if data points share the same **Partition ID**.
* The Partition ID is a key you assign to related data records, ensuring they are sent to the same shard and processed sequentially.

### **2. Shard Capacity (The Scaling Unit)**

A shard defines the throughput limits of the stream:

| Capacity Type | Limit per Shard |
| :--- | :--- |
| **Write (Inbound)** | **1 MB/second** or **1,000 records/second** |
| **Read (Outbound)** | **2 MB/second** |

> **Scaling Example:** To send 10,000 records/second (or 10 MB/second), you would need to provision **10 shards**.

### **3. Capacity Modes**

KDS offers two ways to manage shard capacity:

| Mode | Capacity Management | Scaling | Cost Model |
| :--- | :--- | :--- | :--- |
| **Provisioned Mode** | **Manual** – You explicitly choose the number of shards. | Requires continuous **monitoring** and manual scaling (increasing/decreasing shards) to match load. | Pay per **shard provisioned per hour**. |
| **On-Demand Mode** | **Automatic** – AWS manages the capacity for you. | Scales automatically based on the **observed throughput** over the last 30 days, with a default minimum of 4 MB/s (4,000 records/s). | Pay per **stream per hour** and per **amount of data** going in and out. |

---

## 🛠️ Optimization Libraries

To build efficient producer and consumer applications that fully utilize KDS's capacity and handle retries, batching, and load distribution, AWS provides specialized libraries:

* **Kinesis Producer Library (KPL):** Used to build **high-throughput, optimized producer applications**. It handles batching records and retries.
* **Kinesis Client Library (KCL):** Used to build **optimized consumer applications**. It simplifies reading from the stream, handles checkpointing, and load distribution across multiple consumer instances.