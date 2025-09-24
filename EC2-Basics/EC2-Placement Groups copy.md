# 🌐 Elastic Network Interfaces (ENI) – Learning Notes

This note captures my understanding of **Elastic Network Interfaces (ENIs)** in AWS.  
They are one of the most fundamental building blocks of networking in a VPC and act like **virtual network cards** for EC2 instances.

---

## 🧩 What is an ENI?
- ENI = **Elastic Network Interface**.  
- A logical component in a **VPC**.  
- Provides **network connectivity** to EC2 instances and can also be used outside EC2.  
- Bound to a **specific Availability Zone (AZ)** (cannot move across AZs).  

---

## ⚙️ ENI Attributes
Each ENI can have:
- **Primary private IPv4** (required).  
- **Secondary private IPv4 addresses** (optional, multiple).  
- **Elastic IPs** (one per private IPv4).  
- **Public IPv4s**.  
- **One or more Security Groups** attached.  
- **MAC Address**.  

---

## 🖥️ ENI and EC2
- When you launch an EC2 instance:  
  - The **primary ENI** is automatically attached as `eth0`.  
  - This provides the instance’s main **private IP**.  
- You can **attach additional ENIs** (e.g., `eth1`) to the same EC2 instance for extra IPs or network isolation.  

---

## 🔄 ENI Mobility & Failover
- ENIs can be **created independently** from EC2 instances.  
- They can be **attached/detached on the fly**.  
- Use Case: **Failover scenarios**  
  - Example: If Instance A fails, you can move an ENI (with its private IP) to Instance B.  
  - This allows seamless failover without changing DNS or reconfiguring applications.  

---

## 📊 Key Takeaways
- ENIs = **virtual network cards** inside your VPC.  
- They provide **flexibility**, **scalability**, and **failover** capability.  
- Bound to a **specific AZ** (cannot move across AZs).  
- Useful for high availability designs where IP addresses must stay consistent across EC2 instances.  

---

✅ **In short:**  
ENIs are like “plug-and-play” network cards for EC2 — they carry IP addresses, security groups, and network connectivity, and can move between instances for **resilience**.