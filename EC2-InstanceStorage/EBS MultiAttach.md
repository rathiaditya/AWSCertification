# 📚 AWS Learning Guide: EBS Multi-Attach

This guide summarizes the concept of **EBS Multi-Attach**, how it works, and when to use it.  

---

## 🔹 What is EBS Multi-Attach?
- **Multi-Attach** allows a single **io1** or **io2** EBS volume to be attached to **multiple EC2 instances** at the same time.  
- All instances get **full read and write permissions**.  
- Only available within the **same Availability Zone (AZ)**.  

---

## 🔹 Key Features
- Supported only on: **Provisioned IOPS SSD (io1 & io2)** volumes.  
- Max: **16 EC2 instances** can attach to the same volume simultaneously.  
- Each attached instance can perform **concurrent read/write operations**.  
- Not supported across different AZs (limited to one AZ).  

---

## 🔹 Use Cases
- **High Availability Clusters**  
  Example: clustered Linux applications (e.g., Teradata).  
- **Shared Data Access**  
  Applications that must manage **concurrent write operations**.  

---

## 🔹 Limitations
- ❌ Works only within a single Availability Zone.  
- ❌ Requires a **cluster-aware file system** (standard ones like XFS/EXT4 won’t work safely).  
- ❌ Not available for gp2, gp3, st1, or sc1 volumes.  

---

## 🧠 Key Takeaways
- **Multi-Attach = io1/io2 only.**  
- **Up to 16 EC2 instances** can share the same volume.  
- Great for **HA clusters** but requires special file systems.  
- Still tied to **one AZ** — no cross-AZ support.  

---

📌 *Learning this helped me understand how AWS enables clustered applications to share storage. The “16 EC2 per volume” limit and “cluster-aware file system” requirement are key exam facts to remember!*
