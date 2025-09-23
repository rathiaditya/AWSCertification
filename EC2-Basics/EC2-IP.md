# 🌐 AWS Networking Basics: Public vs Private IPs & Elastic IPs

This note captures my learning experience around **networking concepts in AWS**, focusing on **public IPs, private IPs, and Elastic IPs**. These are fundamental to understanding how EC2 instances and VPCs communicate.

---

## 📌 IPv4 vs IPv6
- **IPv4**: Most commonly used. Format: `x.x.x.x` (four numbers between 0–255).  
  - ~3.7 billion public addresses exist (almost exhausted).  
- **IPv6**: Longer alphanumeric format, designed for **IoT** and large-scale connectivity.  
  - Supported by AWS but less commonly used today.  

👉 In AWS learning contexts, **IPv4 is the default focus**.

---

## 🔑 Public IPs
- Uniquely identifies a machine **on the internet**.  
- Must be **globally unique** (no two machines can share it).  
- Used to connect from outside (e.g., SSH into an EC2).  
- Can be geolocated via IP lookup.  
- **Drawback**: Public IPs for EC2 change when an instance is stopped and restarted.  

---

## 🔒 Private IPs
- Identifies a machine **only within a private network (VPC)**.  
- Must be **unique within the private network**, but **different networks can reuse the same private IPs**.  
- Cannot be accessed from the internet directly.  
- Communication outside the private network happens via:
  - **NAT Gateway/Device**  
  - **Internet Gateway** (acts as a proxy).  

---

## 📌 Elastic IPs (EIP)
- A **fixed public IPv4** that remains the same even if the instance stops/starts.  
- Owned by your AWS account until released.  
- Can only be attached to **one instance at a time**.  
- Useful for:
  - Quickly remapping IPs between instances (failover scenarios).  
- Limitations:
  - Default quota = **5 per AWS account**.  
  - Using Elastic IPs is generally considered **poor architecture**.  
  - Better alternatives:  
    - **DNS via Route 53** (map a domain to dynamic IPs).  
    - **Load Balancer** (avoid public IP usage altogether).  

---

## 🧑‍💻 Hands-On Observations
- By default, an EC2 instance gets:
  - **Private IP** (for internal AWS network).  
  - **Public IP** (for external access).  
- For SSH access:
  - Use **Public IP** (unless connected via VPN into the VPC).  
- When an EC2 instance is stopped & restarted:
  - **Private IP remains the same.**  
  - **Public IP changes** (unless using Elastic IP).  

---

## 🎯 Key Takeaways
- Public IP = internet-facing, unique globally.  
- Private IP = local to VPC, can be reused across networks.  
- Elastic IP = fixed public IPv4, but avoid unless necessary.  
- Best practice: use **DNS (Route 53)** or **Load Balancers** instead of hardcoding public IPs.  
