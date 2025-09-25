# 📚 AWS Learning Guide: EBS Snapshots

This guide captures key concepts I learned about **Amazon EBS Snapshots** and their features, which are crucial for understanding storage backups, recovery, and cost optimization in AWS.

---

## 🔹 What is an EBS Snapshot?
- An **EBS Snapshot** is a **backup** of your EBS volume at a specific point in time.
- You **don’t need to detach** your EBS volume from the EC2 instance to take a snapshot (though it’s often recommended for consistency).
- Snapshots allow you to:
  - Copy data **across different Availability Zones (AZs)**.
  - Copy data **across AWS Regions**.

👉 Example:  
If you have an **EC2 instance in `us-east-1a`** and another in **`us-east-1b`**, you can:
1. Take a snapshot of the volume in `us-east-1a`.  
2. Restore it in `us-east-1b`.  
This is how volumes can be effectively moved between AZs.

---

## 🔹 Key Features of EBS Snapshots

### 1. **Snapshot Archive**
- Move snapshots into an **archive tier**.
- Saves **up to 75% of storage costs**.
- Downside: **Restoration takes 24–72 hours** → not immediate.

---

### 2. **Recycle Bin**
- Deleted snapshots don’t vanish immediately.
- They go into a **Recycle Bin** instead, where you can **recover accidentally deleted snapshots**.
- Retention period can be set: **1 day → 1 year**.

---

### 3. **Fast Snapshot Restore (FSR)**
- Ensures snapshots are **fully initialized** so there is **no latency on first use**.
- Especially useful for **large snapshots** or situations where you need to launch volumes instantly.
- ⚠️ **Expensive** feature → use only when speed is critical.

---

## ✅ Why This Is Important
- Helps in **disaster recovery** across regions and AZs.
- Reduces **storage costs** with archive tiering.
- Protects against **accidental deletions**.
- Provides **instant readiness** for production workloads with Fast Snapshot Restore.

---

## 🧠 Key Takeaways
- **EBS Snapshots = backups** for EBS Volumes.  
- They provide **mobility across AZs/Regions**.  
- Choose between **cost savings (Archive)** vs. **speed (FSR)** depending on workload.  
- Use **Recycle Bin** to avoid permanent data loss.  

---

📌 *This was a valuable learning experience and a reminder of how AWS gives us multiple options to balance **cost**, **reliability**, and **performance** with storage backups.*
