# 📂 AWS Learning Guide: EBS vs EFS vs Instance Store

This guide helps understand the key differences between **Amazon EBS**, **Amazon EFS**, and **EC2 Instance Store**.  

---

## 🔹 Amazon EBS (Elastic Block Store)
- **Attachment**: One EC2 instance at a time.  
  - Exception: `io1` / `io2` volumes with **Multi-Attach** (up to 16 instances).  
- **AZ Locked**: Volumes exist in a single Availability Zone.  
  - To migrate across AZs → take a **snapshot**, then restore in another AZ.  
- **Performance**:  
  - `gp2`: IOPS scales with disk size.  
  - `gp3` & `io1/io2`: IOPS can be set **independently** of disk size.  
- **Backups**:  
  - Snapshots consume **I/O**.  
  - Avoid running backups during heavy traffic (may affect performance).  
- **Lifecycle with EC2**:  
  - Default: **Root EBS volume is deleted** if the EC2 instance is terminated.  
  - Option to disable termination protection for data persistence.  

✅ **Use Cases**:  
- Boot volumes (OS disks).  
- Databases requiring block-level storage.  
- Persistent single-instance storage.  

---

## 🔹 Amazon EFS (Elastic File System)
- **Type**: Managed **network file system** (NFS).  
- **Attachment**: Can be shared across **hundreds of EC2 instances**, across multiple AZs.  
- **Architecture**: One EFS file system → multiple **mount targets** in different AZs.  
- **Compatibility**: Linux-only (POSIX-compliant).  
- **Cost**: More expensive than EBS (~3x GP2), but scales automatically.  
- **Optimization**:  
  - Supports **storage tiers** (Standard, Infrequent Access, Archive) to reduce cost.  

✅ **Use Cases**:  
- Shared storage across applications (e.g., **WordPress**).  
- Content management & web servers.  
- Data sharing across AZs.  

---

## 🔹 EC2 Instance Store
- **Type**: Ephemeral storage physically attached to the EC2 host.  
- **Performance**: Extremely high I/O performance (direct hardware access).  
- **Persistence**: Data is lost if:  
  - Instance is stopped or terminated.  
  - Underlying hardware fails.  
- **Responsibility**: User must manage backups & replication manually.  

✅ **Use Cases**:  
- Temporary data (cache, buffer, scratch space).  
- High-performance workloads where durability is not critical.  

---

## 🔹 Quick Comparison

| Feature              | EBS                           | EFS                                | Instance Store |
|----------------------|-------------------------------|-------------------------------------|----------------|
| **Attachment**       | 1 EC2 (multi-attach for io1/io2) | Many EC2s across AZs                 | 1 EC2 only     |
| **AZ Scope**         | Locked to single AZ           | Multi-AZ (via mount targets)        | Host-specific  |
| **Persistence**      | Data persists unless deleted  | Data persists, pay-per-use          | Data lost on stop/terminate |
| **Performance**      | Scalable IOPS (gp3/io1/io2)   | High throughput, scalable, shared   | Very high (hardware-level) |
| **Cost**             | Moderate                      | High (~3x GP2), optimized with tiers | Free (included in EC2 cost) |
| **Use Cases**        | Boot volumes, DBs, single-instance apps | Shared file systems, WordPress, data sharing | Cache, buffer, temporary data |

---

## 🧠 Key Takeaways
- **EBS** → Block storage for **one instance** (except io1/io2 Multi-Attach).  
- **EFS** → Network file system for **many instances across AZs**.  
- **Instance Store** → Temporary, ultra-fast, but not durable.  

📌 *Think: EBS = per-instance persistence, EFS = shared network storage, Instance Store = fast but ephemeral.*
