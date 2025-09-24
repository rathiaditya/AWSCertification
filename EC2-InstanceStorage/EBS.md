# 💾 AWS EC2 Storage – EBS Volumes

## 📌 What are EBS Volumes?
- **EBS = Elastic Block Store** → network drives for EC2.  
- Work like **network-attached USB sticks** that can persist data **even after EC2 termination**.  
- Can be **detached and re-attached** between instances (within the same AZ).  
- Bound to an **Availability Zone (AZ)** – must match the AZ of the EC2 instance.  

---

## ⚙️ Key Properties
- **Persistence** → Data remains after instance termination.  
- **One-to-One Mounting** → At CCP level, 1 EBS volume = attached to 1 EC2 at a time.  
- **Multiple Volumes** → An EC2 can have multiple EBS volumes.  
- **Provisioning** → Must predefine size (GB) and performance (IOPS).  
- **Snapshots** → Used to move/copy EBS volumes across AZs or regions.  
- **Unattached Volumes** → Can exist independently, ready to attach when needed.  

---

## 🗂️ Delete on Termination (Important!)
- **Root EBS Volume** → By default **deleted** when instance is terminated.  
- **Additional EBS Volumes** → By default **not deleted**.  
- **Customizable** → You can enable/disable this flag at creation time.  

✅ Example use case:  
Disable *Delete on Termination* for root volume to preserve important data even if the EC2 instance is terminated.  

---

## 🚀 Use Cases
- Persist application or database data across instance restarts.  
- Failover – detach volume from one EC2 and reattach to another.  
- Storage expansion – increase size/IOPS when workload grows.  
- Snapshots for backups or cross-AZ/region replication.  

---

## 🧠 Key Takeaway
EBS volumes give **persistent, network-attached storage** to EC2.  
Think of them as **detachable USB drives in the cloud** — durable, flexible, and bound to an AZ.
