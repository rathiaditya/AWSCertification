# 📘 Learning Guide: Application Load Balancer (ALB) in AWS

This guide is based on the provided transcript and is designed as a **hands-on friendly learning resource**. It highlights the key concepts, examples, and comparisons for **Application Load Balancer (ALB)** in AWS.  

---

## 🚀 What is an Application Load Balancer (ALB)?
- **Layer 7 (HTTP/HTTPS) Load Balancer**  
  Works at the **application layer** of the OSI model, meaning it understands HTTP/HTTPS traffic.  
- Supports:
  - **HTTP/2** and **WebSockets**
  - **Automatic Redirects** (e.g., HTTP → HTTPS)
  - **Advanced Routing Rules** (path, host, headers, query strings)

---

## ⚡ Key Features of ALB
1. **Routing to Multiple Applications**
   - Can route traffic to different applications on:
     - The same EC2 instance  
     - Different EC2 instances  
     - Containers (ECS)  
     - Lambda functions  
     - On-premises servers (via private IPs)

2. **Dynamic Port Mapping (for ECS/containers)**  
   ALB integrates tightly with **Amazon ECS** and Docker containers, automatically mapping ports.

3. **Smart Routing Options**
   - **Path-based**: `/users` → User Service, `/search` → Search Service  
   - **Host-based**: `api.example.com` → API service, `app.example.com` → Web App  
   - **Header-based**: Route traffic based on HTTP headers  
   - **Query string–based**: `?platform=mobile` → Mobile service, `?platform=desktop` → Desktop service  

---

## 🧩 Target Groups
Target Groups define **where the traffic should go**.  
- A target group can contain:
  - **EC2 instances**
  - **Auto Scaling Groups**
  - **ECS tasks (containers)**
  - **Lambda functions** (for serverless apps)
  - **Private IPs** (on-premises servers)

- **Health Checks**  
  Each target group has health checks to ensure only healthy instances receive traffic.

---

## 🏗️ Example: Microservices with ALB
Imagine you run two microservices:
- **User Service** → Handles `/users`  
- **Search Service** → Handles `/search`

Instead of creating two Classic Load Balancers, you can use **one ALB** with **two target groups**:
- `/users` → Target Group 1 → EC2 Instances for User Service  
- `/search` → Target Group 2 → EC2 Instances for Search Service  

✅ Benefit: One ALB can route intelligently to multiple apps.  

---

## 🔗 Integration with On-Prem Servers
- ALB can route to **private IPs** in your own datacenter.  
- Example:
  - Mobile traffic (`?platform=mobile`) → AWS EC2 target group  
  - Desktop traffic (`?platform=desktop`) → On-premises target group  

---

## 🛠️ Request Headers and Client IP
- ALB **terminates the connection** from the client and forwards traffic to your servers.  
- The **real client IP** is passed via headers:
  - `X-Forwarded-For` → Client IP  
  - `X-Forwarded-Port` → Original port  
  - `X-Forwarded-Proto` → Protocol used (HTTP/HTTPS)  

👉 Your application must read these headers to log the correct client IP.

---

## 🔄 ALB vs Classic Load Balancer (CLB)
| Feature | ALB | CLB |
|---------|-----|-----|
| Protocol Layer | Layer 7 (HTTP/HTTPS) | Layer 4 & 7 |
| Path-based Routing | ✅ Yes | ❌ No |
| Host-based Routing | ✅ Yes | ❌ No |
| Multiple Apps per LB | ✅ Yes | ❌ Requires multiple CLBs |
| Containers (ECS) | ✅ Native support | ❌ No |
| WebSockets/HTTP2 | ✅ Supported | ❌ Limited |

---

## 📌 Key Takeaways
- Use **ALB** when:
  - You have **microservices** or **containerized apps**.  
  - You need **advanced routing**.  
  - You want **serverless integration** (Lambda).  

- Use **CLB** only for very simple apps, or when you need TCP/SSL passthrough at Layer 4.  

---

## 🧪 Hands-On Checklist
1. Create an **Application Load Balancer** in AWS.  
2. Define **two target groups**:
   - One with EC2 instances for `/users`.  
   - Another with EC2 instances for `/search`.  
3. Add **routing rules** in ALB:
   - `/users` → Target Group 1  
   - `/search` → Target Group 2  
4. Test with a browser or `curl` command.  
5. Inspect headers (`X-Forwarded-For`) on your EC2 logs.  

---

## 📖 Extra Notes (not in transcript but important)
- **Pricing**: You pay for hours the ALB is running + processed LCU (Load Balancer Capacity Units).  
- **Cross-Zone Load Balancing**: By default, ALB distributes traffic across AZs.  
- **Security Groups**: ALB itself needs a security group allowing inbound HTTP/HTTPS.  

---

## 🎯 Summary
The **Application Load Balancer (ALB)** is AWS’s smart, modern load balancer built for **microservices, containers, and serverless**. It allows:
- Intelligent routing  
- Support for multiple target groups  
- Integration with ECS, Lambda, and on-prem servers  

Compared to Classic Load Balancer, ALB is **more powerful, cost-efficient, and flexible** for modern apps.

---
