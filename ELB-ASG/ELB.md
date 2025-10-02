# ⚖️ AWS Elastic Load Balancers (ELB) – Learning Guide

This guide covers **Elastic Load Balancers (ELBs)** in AWS, their purpose, types, and best practices.  
It’s based on real-world examples and designed for easy recall later.

---

## 🌐 What is a Load Balancer?
A **load balancer** is a server (or set of servers) that:
- Forwards incoming traffic to multiple backend **EC2 instances**.
- Distributes load evenly so no single instance gets overloaded.
- Provides **one endpoint** for users, hiding backend complexity.

🔹 Example:  
- 3 EC2 instances behind 1 Elastic Load Balancer (ELB).  
- 3 users connect → each request gets routed to a different EC2.  
- Users don’t know which instance they hit; they only see the ELB endpoint.

---

## ✅ Why Use a Load Balancer?
- **Single point of access** → one DNS name for clients.
- **Fault tolerance** → unhealthy instances removed via **health checks**.
- **SSL termination** → offload HTTPS/TLS to the ELB.
- **Session stickiness** → enforce session persistence with cookies.
- **High availability** → spread traffic across multiple AZs.
- **Separation of concerns** → public traffic vs. private traffic.
- **Fully managed** → AWS handles upgrades, maintenance, scaling.

---

## 🩺 Health Checks
Load balancers use **health checks** to ensure only healthy instances get traffic.

- **Example config**:  
  - Protocol: `HTTP`  
  - Port: `4567`  
  - Endpoint: `/health`  

If the check does not return **HTTP 200**, the instance is marked **unhealthy** and traffic is stopped.

---

## 🔑 Types of AWS Load Balancers
AWS offers **four kinds** of managed load balancers:

1. **Classic Load Balancer (CLB, 2009)**  
   - Protocols: HTTP, HTTPS, TCP, SSL.  
   - Legacy (V1) → **deprecated** but still available.  
   - ⚠️ Avoid for new apps.

2. **Application Load Balancer (ALB, 2016)**  
   - Protocols: HTTP, HTTPS, WebSockets.  
   - Best for **Layer 7 routing** (e.g., path-based, host-based routing).  

3. **Network Load Balancer (NLB, 2017)**  
   - Protocols: TCP, TLS, UDP.  
   - Best for **high-performance, low-latency, millions of requests/sec**.  

4. **Gateway Load Balancer (GWLB, 2020)**  
   - Operates at **Layer 3 (IP layer)**.  
   - Best for **network appliances** (firewalls, intrusion detection, packet inspection).  

---

## 🔒 Security Group Best Practices
- **Users → Load Balancer**  
  - Allow inbound HTTP/HTTPS (ports `80` and `443`).  
  - Source: `0.0.0.0/0` (anywhere).  

- **Load Balancer → EC2 Instances**  
  - Allow traffic only from the **Load Balancer’s security group**.  
  - Prevents direct access to EC2 instances from the internet.  

---

## 🔄 Internal vs External ELBs
- **Internal ELB** → used for private apps within a VPC.  
- **External ELB** → used for public-facing apps (e.g., websites).  

---

## 🔗 Integrations
ELBs integrate seamlessly with many AWS services:
- **EC2 Auto Scaling Groups**
- **Amazon ECS (containers)**
- **AWS Certificate Manager (SSL certs)**
- **Amazon CloudWatch (monitoring)**
- **Route 53 (DNS)**
- **AWS WAF (security)**
- **Global Accelerator**  

---

## 📝 Key Takeaways
- Always use **managed ELBs** instead of rolling your own.
- Use **ALB** for web apps, **NLB** for high-performance traffic, **GWLB** for network appliances.
- Configure **health checks** to ensure traffic only flows to healthy instances.
- Secure EC2 instances by only allowing traffic from the ELB security group.
- Choose between **internal** or **external** ELBs depending on your workload.

---

💡 **Pro Tip:**  
In AWS exams and real-world architecture, **ELBs + Auto Scaling Groups across multiple AZs = fault-tolerant, highly available apps.**
