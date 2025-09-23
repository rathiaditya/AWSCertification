# Deep Dive into AWS EC2 Spot Instances 🚀

This document captures my learning experience on **AWS EC2 Spot Instances** and their pricing model, termination behavior, and advanced strategies like **Spot Fleets**. Spot instances are an amazing way to save costs (up to 90% vs On-Demand), but they come with trade-offs in reliability.

---

## 📌 Key Concepts

### What Are Spot Instances?
- Discounted EC2 instances where AWS sells spare capacity.
- Can save up to **90%** compared to On-Demand.
- Pricing fluctuates based on supply & demand.

### How Pricing Works
- Define a **maximum spot price** (your budget).
- Instance runs as long as the **current spot price ≤ your max price**.
- If the price rises above your max, AWS reclaims the instance.
- You get a **2-minute warning** to stop or terminate gracefully.

---

## 🛑 Termination Strategies
1. **Stop**  
   - Instance is stopped, data persists.  
   - If the spot price falls later, you can restart.  

2. **Terminate**  
   - Instance is fully terminated.  
   - Start fresh when resuming work.  

3. **Spot Block**  
   - Reserve a spot instance for **1–6 hours** without interruption.  
   - Rarely, AWS may still reclaim it.  

---

## 🧑‍💻 When to Use Spot Instances
✅ Batch jobs  
✅ Data analysis  
✅ Fault-tolerant workloads  

❌ Not suitable for **critical apps** (e.g., production databases).  

---

## 📉 Pricing Visualization
- Prices vary across **Availability Zones (AZs)**.
- Stable overall, but can fluctuate within ranges.
- Choosing the right **max spot price** balances cost vs. stability.

---

## 🔄 Spot Requests
- **One-time request** → fulfilled once, then disappears.  
- **Persistent request** → keeps trying to maintain desired capacity.  

⚠️ To fully terminate:
1. **Cancel the spot request** first.  
2. **Terminate instances** after.  
   - Otherwise, AWS may relaunch them automatically.  

---

## 🚀 Spot Fleets
- A **fleet = mix of Spot + On-Demand instances**.
- Tries to meet capacity with best cost options.
- Uses **multiple launch pools**:
  - Different instance types
  - Different AZs
  - Different OS  

### Allocation Strategies:
1. **Lowest Price** → Cheapest option, great for short workloads.  
2. **Diversified** → Spread across pools for reliability.  
3. **Capacity Optimized** → Choose pools with highest available capacity.  
4. **Price Capacity Optimized** → Balance cost + capacity (best for most workloads).  

---

## 🎯 Key Takeaways
- Spot Instances = **huge cost savings** with some trade-offs.  
- Use them for **resilient, fault-tolerant workloads**.  
- Always **cancel the spot request before terminating** instances.  
- For flexibility and max savings, **Spot Fleets** are the smarter option.  

---

📖 This deep dive helped me clearly understand not just the savings potential, but also **operational pitfalls** (like request cancellation order) and advanced strategies for managing spot capacity effectively.
