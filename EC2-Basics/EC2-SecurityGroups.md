# AWS EC2 Security Groups 🔐

## Summary
This session focused on **Security Groups**, the fundamental firewalls for EC2 instances in AWS.  
They define how traffic flows **into (inbound)** and **out of (outbound)** an instance, forming a crucial layer of **network security** in the cloud.

---

## Key Learnings

### 🔑 Basics of Security Groups
- Act as **virtual firewalls** around EC2 instances.  
- Control **inbound (into EC2)** and **outbound (from EC2)** traffic.  
- **Only allow rules** → no explicit deny rules.  
- Can reference:
  - **IP addresses / ranges** (IPv4 & IPv6).  
  - **Other security groups** for flexible communication.  

---

### 🛡️ Behavior
- **Default**:  
  - Inbound traffic → **blocked**.  
  - Outbound traffic → **allowed**.  
- Security groups live **outside the instance**:
  - Blocked traffic never reaches EC2.  
- One instance can have **multiple security groups**.  
- A security group can apply to **multiple instances**.  
- Security groups are tied to **Region + VPC** → must recreate if switching.  

---

### 💡 Best Practices
- Maintain a **separate security group just for SSH** (port 22).  
- **Timeout error** → likely a security group issue (blocked traffic).  
- **Connection refused** → traffic reached EC2, but the app wasn’t listening/responding.  

---

### ⚙️ Advanced Feature – Referencing Security Groups
- Security groups can **reference each other** instead of relying on IPs.  
- Example:
  - Group A allows inbound from **Group B**.  
  - Any instance with Group B can connect to Group A instances.  
- Useful for **load balancers, multi-tier apps, or auto-scaling**.  

---

### 📌 Common Ports to Remember
- **22** → SSH (Linux login)  
- **21** → FTP (file transfer)  
- **22 (again)** → SFTP (secure file transfer over SSH)  
- **80** → HTTP (unsecured websites)  
- **443** → HTTPS (secured websites, standard today)  
- **3389** → RDP (Windows remote desktop)  

---

## Takeaway
Security groups are **simple yet powerful**:
- They regulate **who can talk to your EC2** and **what your EC2 can reach**.  
- They provide **granular control without dealing with IP addresses** directly (when referencing groups).  
- Understanding security groups is essential for **AWS networking, troubleshooting, and exam prep**.   
