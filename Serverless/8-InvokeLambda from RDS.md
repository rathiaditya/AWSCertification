## 💾 Database Integration Patterns with Lambda

This guide differentiates between two critical ways AWS Lambda interacts with **Amazon RDS** (Relational Database Service), focusing on the distinction between reacting to **Data Events** (inside the database) versus **Service Events** (about the database instance).

---

## 1. Data Events: RDS Directly Invokes Lambda

This pattern is used when you need to run code *immediately* after a change happens to the **data** inside a table (e.g., a new row is inserted).

* **Supported Engines:** **RDS for PostgreSQL** and **Aurora MySQL/PostgreSQL** (requires specific setup/extensions within the database itself).
* **Mechanism:** You configure a trigger, stored procedure, or extension **within the database itself** (not via the AWS Console) to call the Lambda API endpoint.
    * Example: A user inserts a row into the `Registration` table $\rightarrow$ the database trigger executes $\rightarrow$ the database invokes the Lambda function $\rightarrow$ Lambda sends a welcome email.
* **Key Requirements:**
    1.  **IAM Permissions:** The **RDS instance** must have an associated **IAM role** that grants it permission to use the `lambda:InvokeFunction` action.
    2.  **Network Connectivity:** The RDS instance must have network access to the Lambda function's API endpoint (which may require using a **NAT Gateway**, **VPC Endpoint**, or ensuring the Lambda function is accessible).

> **Analogy:** This is like the database itself running a script when a specific change happens to the spreadsheet data.

---

## 2. Service Events: RDS Event Notifications

This pattern is used to react to events related to the **health, configuration, and lifecycle** of the database instance itself, **not** changes to the data within the tables.

* **Source:** The AWS RDS service sends notifications about the database instance.
* **Event Types (Examples):**
    * **Configuration:** Database parameter group changes.
    * **Availability:** Instance created, started, stopped, rebooted, or failed over.
    * **Maintenance:** Database instance maintenance is scheduled or completed.
* **Delivery:** These events are delivered to two main services:
    1.  **Amazon SNS:** Notifications are sent to an SNS Topic, which can then notify subscribers (emails, SQS, or a Lambda function).
    2.  **Amazon EventBridge:** Events are sent to EventBridge, which allows for more complex filtering and routing to various targets, including a Lambda function.
* **Timing:** These are **near real-time events** (delivery is typically within five minutes).
* **Crucial Distinction:** **RDS Event Notifications DO NOT provide information about data events** (INSERT, UPDATE, DELETE) happening within your tables.

| Event Type | Purpose (What it tracks) | Trigger Mechanism | Access to Data? |
| :--- | :--- | :--- | :--- |
| **Data Events** | Changes to table **data** (e.g., new user registration). | **Database Trigger/Extension** configured *inside* the DB. | **YES**, the function receives the data payload. |
| **Service Events** | Changes to the **instance** lifecycle/metadata (e.g., failover, snapshot taken). | **AWS RDS Service** via SNS/EventBridge. | **NO**, only service metadata about the instance itself. |