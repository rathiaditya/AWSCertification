# 📂 AWS Learning Guide: Amazon Elastic File System (EFS)

Amazon **EFS (Elastic File System)** is a **managed Network File System (NFS)** that allows multiple EC2 instances to **mount the same file system across multiple Availability Zones (AZs)**.  

---

## 🔹 Key Features
- **Managed NFS**: No infrastructure management, AWS handles scaling & availability.  
- **Multi-AZ Access**: Can be mounted by EC2 instances across different AZs.  
- **Scalable**: Automatically grows up to **petabyte scale**.  
- **Highly Available & Durable**: Ideal for production workloads.  
- **Pay-per-use**: No need to pre-provision capacity, charged per GB used.  
- **Linux Only**: Works with Linux-based AMIs, not Windows.  
- **Security**: Access controlled with **security groups**.  
- **Encryption**: Supports encryption at rest (KMS) and in transit.  

---

## 🔹 Use Cases
- 📑 **Content management systems**  
- 🌐 **Web serving**  
- 🤝 **Data sharing** across instances  
- 📝 **WordPress hosting**  
- 🎥 **Media processing / Big Data**  

---

## 🔹 Performance Modes
Choose at **creation time**:
1. **General Purpose (default)**  
   - Low latency, best for web servers, CMS, latency-sensitive workloads.  

2. **Max I/O**  
   - Higher latency but **higher throughput & parallelism**.  
   - Best for **big data, analytics, or media processing**.  

---

## 🔹 Throughput Modes
1. **Bursting** (default)  
   - Throughput grows with storage size.  
   - Example: 50 MB/s baseline per TB, bursts up to 100 MB/s.  

2. **Provisioned**  
   - Set throughput **independently** from storage size.  
   - Example: 1 GB/s throughput for 1 TB storage.  

3. **Elastic**  
   - Scales up/down automatically with workload.  
   - Example: Up to **3 GB/s reads** and **1 GB/s writes**.  
   - Best for **unpredictable workloads**.  

---

## 🔹 Storage Classes
EFS offers **storage tiers** with lifecycle management to reduce costs:

- **Standard** → Frequently accessed files.  
- **EFS-IA (Infrequent Access)** → Cheaper storage, small retrieval cost.  
- **Archive Tier** → Rarely accessed files, cheapest storage.  

📌 You can use **lifecycle policies** to automatically move files between tiers:  
- Example: Move files not accessed for **60 days** → `EFS-IA`.

---

## 🔹 Availability & Deployment Options
- **Standard (Multi-AZ)**  
  - Replicated across multiple AZs.  
  - Recommended for **production workloads**.  

- **One Zone**  
  - Cheaper, stored in a single AZ.  
  - Still durable & compatible with **IA storage tier**.  
  - Best for **dev/test workloads**.  

💡 Using **IA and Archive tiers with lifecycle policies** can save up to **90% in storage costs**.  

---

## 🧠 Key Takeaways
- EFS = **managed NFS** that can scale to petabytes.  
- **Expensive (~3x GP2 EBS)**, but extremely flexible.  
- **Performance tuning options**: General Purpose vs Max I/O.  
- **Throughput options**: Bursting, Provisioned, Elastic.  
- **Storage optimization**: Standard, EFS-IA, Archive with lifecycle policies.  
- **Deployment**: Multi-AZ for production, One Zone for dev/test.  

---

📌 *Learning this helped me understand when to use EFS vs EBS: EBS is per-instance, while EFS is a shared file system across instances/AZs.*  
