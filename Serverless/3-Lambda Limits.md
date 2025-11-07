## 🚧 AWS Lambda: Essential Execution and Deployment Limits

Understanding the limits of AWS Lambda is crucial for the exam, as these constraints often determine whether Lambda is the appropriate service for a given workload. These limits are typically **per region**.

---

## ⚙️ Execution Limits

These limits define the performance and operational boundaries for a running Lambda function.

| Limit Category | Constraint | Key Takeaway |
| :--- | :--- | :--- |
| **Maximum Execution Time** | **900 seconds (15 minutes)** | Any job requiring longer than 15 minutes is **not** suitable for Lambda. |
| **Memory Allocation** | **128 MB to 10 GB** | Configurable in **1 MB increments**. Increasing RAM proportionally increases CPU and network performance. |
| **Temporary Disk Space (`/tmp`)** | **512Mb to 10 GB** | Use this directory for pulling or creating large files during execution (e.g., downloading a file from S3 to process). |
| **Concurrent Executions** | **1,000** (Soft limit, can be increased) | The default maximum number of functions that can run simultaneously across your entire account in a region. Use **Reserved Concurrency** to guarantee capacity for critical functions. |
| **Environment Variables** | **4 KB** | Limited space for configuration settings. Do **not** use for large files or complex data structures. |

---

## 📦 Deployment Limits

These limits govern the size of the code and dependencies you deploy to Lambda.

| Limit Category | Constraint | Key Takeaway |
| :--- | :--- | :--- |
| **Deployment Package Size (Zipped)** | **50 MB** | The maximum size of the `.zip` file you upload via the console or CLI. |
| **Deployment Package Size (Unzipped)** | **250 MB** | The total size of all files when the `.zip` package is extracted. |

> **Best Practice:** If your deployment package exceeds 250 MB (e.g., if you have large libraries or binaries), you should store the larger files in **Amazon S3** and download them into the **`/tmp` directory** during the function's execution.

---

## 🛑 Recognizing Unsuitable Lambda Workloads

The exam frequently presents scenarios designed to test your knowledge of these limits. If a problem description includes any of the following constraints, **Lambda is not the correct solution**:

| Scenario Constraint | Governing Lambda Limit | Recommended AWS Service |
| :--- | :--- | :--- |
| **"Requires 30 minutes of runtime..."** | Max Execution Time (15 minutes) | **Amazon EC2, AWS Fargate, or ECS** |
| **"Needs to process a 3 GB file..."** | `/tmp` is limited to 10 GB; deployment size is 250 MB. | **Amazon EC2 or AWS Fargate** (for full disk access). |
| **"Needs 30 GB of RAM..."** | Max Memory Allocation (10 GB) | **Amazon EC2** (select a large instance type). |

---