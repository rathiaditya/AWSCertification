
# ⚡ AWS Network Load Balancer (NLB) – Learning Guide

This concise README summarizes the key concepts, architecture, and practical notes for the **AWS Network Load Balancer (NLB)**.  
Save this file in your GitHub repo as `README.md` for quick reference.

## What is an NLB?
- **Layer 4 (Transport Layer)** load balancer for **TCP** and **UDP** traffic.
- Designed for **extreme performance**, **ultra-low latency**, and **static IP** support (one IP per AZ).
- Ideal when you need raw network-level balancing, fixed IPs (EIPs), or hybrid/on-prem routing.

## Key Features
- Handles **TCP** and **UDP** traffic.
- **Millions of requests/sec** with microsecond-level latency.
- **Static IP (Elastic IP)** support per Availability Zone.
- Target groups can contain **EC2 instances, private IPs,** or **ALBs**.
- Supports **health checks** (TCP, HTTP, HTTPS).
- Optional **cross-zone load balancing**.
- Preserves **source client IP** for backends.

## How it works (brief)
1. Client sends TCP/UDP to the NLB endpoint (static IP per AZ).
2. NLB forwards connections to targets in a target group.
3. Health checks ensure only healthy targets receive traffic.

## Target Group Types
- **EC2 instances** — direct backend servers.
- **Private IP addresses** — useful for on-prem or hybrid setups.
- **Application Load Balancers (ALB)** — NLB in front of ALB gives you static IPs + Layer 7 routing.

## Use Cases
- High-performance trading platforms, gaming, streaming (UDP/TCP).
- Systems that require **fixed IP addresses** for firewall whitelisting.
- Hybrid architectures connecting AWS and on-prem backends.
- Fronting an ALB when both static IPs and HTTP routing are needed.

## Health Checks
Supported protocols: **TCP**, **HTTP**, **HTTPS**.  
Configure at the target-group level. If a target fails, NLB stops sending traffic to it.

## Combining NLB & ALB
- NLB provides static IPs & high throughput.
- ALB provides Layer 7 routing (path/host/header-based) and HTTP features.
- Pattern: Client → NLB (static IP) → ALB → Application Targets.

## Comparison: NLB vs ALB (short)
- **NLB**: Layer 4, TCP/UDP, static IPs, ultra-low latency, preserves source IP.
- **ALB**: Layer 7, HTTP/S, path/host routing, WebSockets/HTTP2, more app-aware.

## Hands-on Checklist
1. Create NLB: choose protocol (TCP/UDP) and AZs.
2. Assign Elastic IPs if static IPs are required.
3. Create target groups and register EC2s or private IPs.
4. Configure health checks (TCP/HTTP/HTTPS).
5. (Optional) Chain NLB → ALB for Layer 7 capabilities.
6. Test with `curl`, `nc`, or load-testing tools.

## Practical Tips
- Use NLB when you need to preserve client IP at the backend (no X-Forwarded-For needed).
- Choose NLB for performance-sensitive services.
- Combine with ALB to get the benefits of both layers.
- Monitor with CloudWatch and enable access logs if you need auditing.

## Quick Exam/Interview Memory Aids
- **TCP/UDP & static IPs** → think **NLB**.
- **Path-based routing / microservices** → think **ALB**.
- **Legacy/simple** → **CLB** (legacy; avoid for new designs).

---

**Short summary:** NLB is AWS’s Layer 4 solution for ultra-fast, low-latency, and static-IP-capable load balancing. Use it for network-heavy workloads or when static IPs are required; pair it with ALB for full-featured HTTP routing.
