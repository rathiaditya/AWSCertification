# Learning AWS EC2 🚀

## Summary
This session introduced **Amazon EC2 (Elastic Compute Cloud)** and walked through the fundamentals of creating our **first website on AWS** using EC2.  

### Key Learnings
- **What is EC2?**  
  - AWS service for renting virtual machines (instances).  
  - Core part of Infrastructure as a Service (IaaS).  
  - Provides flexibility to choose compute power, memory, storage, OS, and networking.  

- **EC2 Components**  
  - **Instances** → Virtual machines you rent.  
  - **EBS Volumes** → Network-attached storage.  
  - **Instance Store** → Hardware-attached storage.  
  - **Elastic Load Balancer (ELB)** → Distributes traffic.  
  - **Auto Scaling Group (ASG)** → Scales instances based on demand.  
  - **Security Groups** → Firewall rules for controlling access.  

- **Operating System Options**  
  - Linux (most popular), Windows, macOS.  

- **Customization Options**  
  - CPU cores, RAM, storage (EBS/EFS/Instance Store), networking speed, and IP setup.  

- **Bootstrapping with User Data**  
  - User data scripts run **once at launch** (as root).  
  - Automates tasks like:  
    - Installing updates/software.  
    - Downloading files.  
    - Running setup commands.  
  - Powerful way to pre-configure servers automatically.  

### Takeaway
EC2 shows the **real power of the cloud**: the ability to rent compute resources on-demand, customize them as needed, and bootstrap them with automation — all within minutes.  

---
