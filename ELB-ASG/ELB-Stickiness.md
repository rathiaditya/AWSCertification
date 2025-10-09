# 🍪 AWS Elastic Load Balancer (ELB) — Stickiness (Sticky Sessions) Explained

## 📘 Overview

When using Elastic Load Balancers (ALB, NLB, or CLB), traffic is **normally distributed** evenly across multiple backend instances.  
However, some applications require that **a specific client always connects to the same backend instance** for the duration of their session.

That’s where **stickiness** (also known as **sticky sessions**) comes in.  

> 🧠 **Definition:** Stickiness allows a client to consistently connect to the same backend EC2 instance behind a load balancer, maintaining session continuity.

---

## 🎯 Why Use Stickiness?

Without stickiness, each new request from a user may be sent to a **different backend instance**.  
This can cause problems if user-specific data (like login sessions or shopping carts) is stored **locally** on the instance rather than in a shared database or cache.

### ✅ Example Use Cases
- User logins (session cookies)
- Shopping carts
- Multi-step forms
- Any web app that stores session data on instance memory

---

## 🧩 How Stickiness Works

Let’s visualize it:


     ┌──────────────────────────┐
     │  Application Load Balancer │
     └─────────────┬─────────────┘
                   │
┌─────────────┬─────────────┬─────────────┐
│ EC2 #1 │ EC2 #2 │ EC2 #3 │
└─────────────┴─────────────┴─────────────┘
▲ ▲ ▲
│ │ │
Client 1 → always → EC2 #1
Client 2 → always → EC2 #2
Client 3 → always → EC2 #3



When stickiness is **enabled**, the load balancer uses a **cookie** to remember which backend instance each client is “stuck” to.

---

## 🍪 Stickiness Mechanism — The Cookie System

The load balancer uses **HTTP cookies** to maintain client affinity.

### 🔹 What Happens Behind the Scenes

1. Client sends the **first request** to the load balancer.
2. Load balancer routes it to one backend EC2 instance.
3. Load balancer or application **sets a cookie** in the client’s browser (with an expiry).
4. On subsequent requests, the client **sends the cookie back**.
5. Load balancer reads the cookie and forwards traffic to the **same backend instance**.

If the cookie expires, the load balancer may route the client to a different instance.

---

## 🧠 Cookie Types in AWS Load Balancing

There are **two main types of cookies** used for sticky sessions.

| Cookie Type | Who Creates It | Cookie Name | Expiry Control | Applicable ELB Types | Description |
|--------------|----------------|--------------|----------------|----------------------|--------------|
| **Application-based Cookie** | Your application | Custom name (e.g., `MYAPPSESSION`) | Controlled by your app | ALB only | Created by the backend app. Can include custom attributes. |
| **Duration-based Cookie** | AWS Load Balancer | `AWSALB` (ALB) / `AWSELB` (CLB) | Controlled by load balancer | ALB, NLB, CLB | Managed by ELB; expires after a set duration (1 sec – 7 days). |

---

## 🧱 Reserved Cookie Names (❗Important)

You **must not use** the following cookie names in your application — they’re reserved by AWS:

- `AWSALB`
- `AWSALBAPP`
- `AWSALBTG`
- `AWSELB`

If you override them, your stickiness configuration will not work as expected.

---

## ⚙️ Configuring Stickiness in AWS

### 🪜 Steps to Enable Stickiness (ALB Example)

1. Go to **EC2 Console → Target Groups**.  
2. Select your target group.  
3. Click **Actions → Edit Attributes**.  
4. Scroll to **Stickiness** section.  
5. Turn **ON** stickiness.  
6. Choose:
   - **Load balancer generated cookie**, or  
   - **Application-based cookie**
7. (Optional) Set a **duration** (1 second to 7 days).
8. Save changes.

✅ Done! Your load balancer will now maintain sticky sessions for clients.

---

## 🧪 Hands-on Example

Let’s test how it behaves in the browser:

1. Open your app behind the ALB.  
2. Enable **Developer Tools → Network tab** (Chrome/Firefox).  
3. Refresh the page multiple times.  
4. Observe:
   - All requests go to the **same backend instance**.
   - Under “Cookies,” a cookie like `AWSALB` appears with an expiry time.
5. Disable stickiness in the Target Group and repeat.
   - Now requests should **rotate** across backend instances.

🧩 This confirms stickiness is working.

---

## ⚖️ Pros & Cons

| Advantage | Drawback |
|------------|-----------|
| Maintains user sessions (avoids logouts) | Can cause **uneven load distribution** |
| Simplifies session management in app servers | Sticky users can overload a single instance |
| Easy to configure (no code changes) | Reduced scalability for stateless designs |

---

## 💡 Best Practices

- Use **stickiness only when absolutely necessary**.  
  (Modern apps should be **stateless** whenever possible.)
- If your app needs state persistence, prefer:
  - **Amazon ElastiCache (Redis)** for session storage.
  - **Amazon DynamoDB** or **RDS** for centralized state.
- Always **set reasonable cookie durations** to avoid stale sessions.
- Monitor backend load for **hot spots** caused by sticky clients.

---

## 🔐 Supported Load Balancers

| Load Balancer Type | Supports Stickiness? | Cookie Behavior |
|---------------------|----------------------|-----------------|
| **Application Load Balancer (ALB)** | ✅ Yes | Application-based or Duration-based |
| **Network Load Balancer (NLB)** | ✅ Yes | Duration-based only |
| **Classic Load Balancer (CLB)** | ✅ Yes | Duration-based only |
| **Gateway Load Balancer (GWLB)** | ❌ No | Works at Layer 3, no session tracking |

---

## 🧭 Key Terms Summary

| Term | Description |
|------|--------------|
| **Stickiness (Sticky Session)** | Keeps a client connected to the same backend instance |
| **Cookie** | Identifier that tracks client-server mapping |
| **Application Cookie** | Custom cookie defined by your app |
| **Duration Cookie** | AWS-managed cookie with time-based expiry |
| **Session Persistence** | Maintaining a user's state during multiple requests |

---

## 🧠 AWS Exam Tips

- Stickiness works by **inserting cookies**.
- **Two types:** Application-based (custom) and Duration-based (AWS-managed).
- **NLB supports stickiness via duration cookies only**.
- **GWLB does NOT support stickiness** (Layer 3).
- Overuse of stickiness can lead to **load imbalance**.
- Cookie duration: **1 second → 7 days**.
- ALB uses **cookie `AWSALB`**, CLB uses **`AWSELB`**.

---

## 🎮 Real-World Analogy

Imagine a restaurant (your backend servers) with a hostess (load balancer):

- Normally, each time you visit, the hostess sends you to a **different waiter**.  
- With stickiness enabled, she remembers you and always seats you with the **same waiter**, who remembers your order and preferences.

🍽️ That’s sticky sessions!

---

## 🧾 References

- [AWS Docs — Sticky Sessions for ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/sticky-sessions.html)
- [AWS Docs — Target Group Attributes](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html)
- [AWS Blog — Understanding ELB Stickiness](https://aws.amazon.com/blogs/aws/new-stickiness-for-your-load-balanced-applications/)

---

> 💬 _“If cookies make users happy, ELB stickiness makes their sessions happy.”_
