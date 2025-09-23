# 🚀 Learning AWS – EC2 Purchasing Options  

This note captures my learning about the **different EC2 Instance purchasing options** in AWS.  
It explains when to use each option, their benefits, and how pricing models differ.  

---

## 📌 EC2 Purchasing Models  

### 1. **On-Demand Instances**  
- Pay-as-you-go (per second for Linux/Windows, per hour for others).  
- ✅ Best for: Short-term, unpredictable workloads.  
- ❌ Highest cost, no commitment.  

---

### 2. **Reserved Instances (RI)**  
- Commit to **1 or 3 years**.  
- Up to **72% discount** compared to On-Demand.  
- Fixed attributes (instance type, region, tenancy, OS).  
- Payment: All upfront, partial upfront, or no upfront.  
- **Convertible RIs** → allow changing instance type, OS, family, etc. (up to 66% discount).  
- ✅ Best for: Steady workloads (e.g., databases).  

---

### 3. **Savings Plans**  
- Modern alternative to RIs.  
- Commit to **spending ($/hour)** for 1 or 3 years.  
- Flexible across instance sizes, OS, and tenancy within a family.  
- Up to **70% savings**.  
- ✅ Best for: Long-term usage with some flexibility.  

---

### 4. **Spot Instances**  
- Up to **90% discount**.  
- Can be **terminated anytime** if AWS needs capacity back.  
- ✅ Best for: Batch jobs, data analysis, image/video processing.  
- ❌ Not for critical workloads or databases.  

---

### 5. **Dedicated Hosts**  
- Entire physical server reserved.  
- Full visibility into hardware (sockets, cores, etc.).  
- ✅ Best for: Compliance requirements, Bring-Your-Own-License (BYOL).  
- ❌ Most expensive option.  

---

### 6. **Dedicated Instances**  
- Run on hardware dedicated to you (not shared with other customers).  
- But **no control over instance placement**.  
- ✅ Best for: Security isolation without full host control.  

---

### 7. **Capacity Reservations**  
- Reserve capacity in a specific AZ **without discounts**.  
- Charged at **On-Demand rate**, even if unused.  
- ✅ Best for: Ensuring availability in specific AZs when needed.  

---

## 🏨 Analogy: Resort Hotel 🏨  

- **On-Demand** → Pay full price whenever you stay.  
- **Reserved** → Book long-term stay at a discount.  
- **Savings Plan** → Commit to spend a fixed amount monthly, flexible rooms.  
- **Spot** → Last-minute discounted rooms, but you can get kicked out anytime.  
- **Dedicated Host** → You book the entire hotel building.  
- **Capacity Reservation** → Book a room just in case, pay even if you don’t use it.  

---

## ✅ Key Takeaways  
- Use **On-Demand** for unpredictable short-term workloads.  
- Use **Reserved Instances or Savings Plans** for long-term stable workloads.  
- Use **Spot Instances** for flexible, fault-tolerant tasks.  
- Use **Dedicated Hosts** for compliance or licensing requirements.  
- Use **Capacity Reservations** when you must guarantee availability in an AZ.  

---
