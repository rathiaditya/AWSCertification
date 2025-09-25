# 📚 AWS Learning Guide: EC2 Instance Store

This guide summarizes my learning about **EC2 Instance Store**, a special type of storage that provides extremely high disk performance by attaching physical hardware disks directly to the underlying EC2 host.

---

## 🔹 What is an EC2 Instance Store?
- A **hardware disk** physically attached to the underlying EC2 server.  
- Provides **very high I/O performance** compared to EBS.  
- Storage is **ephemeral** → data is lost if the instance stops, terminates, or the underlying hardware fails.  

👉 Think of it as **"temporary turbo-charged storage"** inside the EC2 host.

---

## 🔹 Why use an Instance Store?
- **Ultra-high performance**:  
  - Millions of IOPS possible (e.g., `i3` instance families).  
  - Much higher than EBS GP2/GP3 (which max out at ~32,000 IOPS).  
- **Low latency & high throughput**: ideal for workloads that need blazing-fast disk access.

---

## 🔹 Limitations
- **Data loss on stop/terminate**:  
  - Not durable storage → data disappears once the instance stops or is terminated.  
- **Tied to the physical server**:  
  - If the hardware fails, the data is lost.  
- **Not shared**: unlike EBS, cannot be attached to another instance.

---

## 🔹 Best Use Cases
- **Temporary storage needs**:
  - Caches
  - Buffers
  - Scratch data
  - Temporary files or logs  
- **High-performance workloads** where persistence is not required.  

👉 For **durable long-term storage**, use **EBS** instead.

---

## 🔹 Key Performance Insight
- Example:  
  - Instance Store (`i3` family):  
    - Random Read IOPS: **3.3 million**  
    - Random Write IOPS: **1.4 million**  
  - EBS GP2/GP3: ~**32,000 IOPS**  
- Instance Store = **massive performance boost**, but **ephemeral**.  

---

## 🧠 Key Takeaways
- EC2 Instance Store = **hardware-based, high-performance, temporary storage**.  
- Ideal for **fast but non-persistent workloads** (cache, temp data).  
- For durability and persistence → always rely on **EBS or S3**.  
- **Backups are your responsibility** if you use Instance Store.  

---

📌 *Learning about EC2 Instance Store gave me a deeper understanding of when to trade off persistence for speed. It’s a powerful option for caching, scratch data, or high I/O tasks, but never a replacement for EBS when durability is required.*
