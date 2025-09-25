# 📚 AWS Learning Guide: EBS Volume Types

This guide summarizes my learning about the **six types of Amazon EBS volumes**, their characteristics, and when to use each.  

---

## 🔹 What is an EBS Volume?
- **Elastic Block Store (EBS)** volumes are network-attached storage for EC2 instances.  
- They persist beyond instance termination and can serve as **boot volumes** or data volumes.  
- Key metrics: **Size, Throughput, and IOPS** (I/O operations per second).  

---

## 🔹 Categories of EBS Volumes
1. **General Purpose SSDs** (gp2, gp3)  
   - Cost-effective, balanced for price and performance.  
   - Can be used as **boot volumes**.  

2. **Provisioned IOPS SSDs** (io1, io2 / Block Express)  
   - High-performance, low-latency.  
   - Ideal for databases and mission-critical workloads.  
   - Can be used as **boot volumes**.  

3. **Throughput-Optimized HDD** (st1)  
   - Low-cost HDD designed for **big data, data warehousing, log processing**.  
   - Not usable as a boot volume.  

4. **Cold HDD** (sc1)  
   - Lowest-cost HDD, for **infrequently accessed archive data**.  
   - Not usable as a boot volume.  

---

## 🔹 General Purpose SSD (gp2 vs gp3)
- **gp2** (older generation):
  - IOPS and volume size are **linked**.  
  - Small volumes can burst up to **3,000 IOPS**.  
  - Max 16,000 IOPS when volume size ≥ 5,334 GB.  

- **gp3** (newer generation):  
  - **3000 baseline IOPS** and **125 MB/s throughput** by default.  
  - IOPS (up to 16,000) and throughput (up to 1,000 MB/s) can be set **independently**.  

👉 Use **gp3** for most general-purpose workloads.  

---

## 🔹 Provisioned IOPS SSD (io1 & io2)
- **io1**:
  - Size: 4 GB – 16 TB.  
  - Max: **64,000 IOPS** (Nitro EC2) or **32,000 IOPS** (non-Nitro).  
  - IOPS can be provisioned **independently of storage size**.  

- **io2 / io2 Block Express**:
  - Size: up to **64 TB**.  
  - Max: **256,000 IOPS** with 1,000:1 IOPS-to-GB ratio.  
  - Sub-millisecond latency.  
  - Supports **EBS Multi-Attach** for attaching the same volume to multiple EC2 instances.  

👉 Best for **databases, financial apps, and latency-sensitive workloads**.  

---

## 🔹 HDD Volumes
- **st1 (Throughput-Optimized HDD):**
  - Size: up to 16 TB.  
  - Max throughput: **500 MB/s**, Max IOPS: **500**.  
  - Great for **big data, streaming, log processing**.  
  - ❌ Cannot be a boot volume.  

- **sc1 (Cold HDD):**
  - Size: up to 16 TB.  
  - Max throughput: **250 MB/s**, Max IOPS: **250**.  
  - Best for **archival & rarely accessed data**.  
  - ❌ Cannot be a boot volume.  

---

## 🧠 Key Takeaways
- **gp2/gp3**: General workloads, boot volumes → choose **gp3** for flexibility.  
- **io1/io2**: Mission-critical, high IOPS workloads (databases).  
- **st1**: High-throughput, frequent access (big data/logs).  
- **sc1**: Rarely accessed, archival, cheapest option.  
- For **>32,000 IOPS**, you need **Nitro EC2 + io1/io2**.  

---

📌 *Learning these EBS volume types clarified which storage to pick for specific workloads. The main tradeoff is between cost, performance, and durability. gp3 is my default for general workloads, while io2 is the go-to for demanding enterprise applications like databases.*
