# 🔐 AWS Learning Guide: Encrypting EBS Volumes

This guide explains how **EBS encryption** works, its benefits, and the steps to encrypt **unencrypted EBS volumes**.

---

## 🔹 What Happens When You Create an Encrypted EBS Volume?
- ✅ **Data at Rest** → Encrypted inside the volume.  
- ✅ **Data in Transit** → Encrypted between EC2 and EBS.  
- ✅ **Snapshots** → Automatically encrypted.  
- ✅ **New Volumes from Snapshots** → Also encrypted.  

✨ All encryption/decryption is **transparent** (handled by AWS).  
✨ Uses **KMS (AES-256)** keys.  
✨ Minimal performance impact (almost zero latency).  

---

## 🔹 How to Encrypt an Existing (Unencrypted) EBS Volume
Since you **cannot directly encrypt an existing unencrypted volume**, you need to go through snapshots:

1. **Create Snapshot** of the unencrypted EBS volume.  
2. **Copy the Snapshot** → enable encryption + select a KMS key.  
3. **Create New EBS Volume** from the encrypted snapshot.  
4. **Attach the Encrypted Volume** to your EC2 instance.  

---

## 🔹 Walkthrough Example
1. Create a **new EBS volume** → uncheck *encryption*.  
   - This volume = **Not Encrypted**.  
2. Create a **snapshot** of this volume → still **Not Encrypted**.  
3. Copy the snapshot → during **copy**, enable *encryption* and choose a **KMS key**.  
   - Result = **Encrypted Snapshot**.  
4. From the encrypted snapshot, create a new **EBS volume** → this one is **Encrypted**.  
5. Attach the encrypted volume to your EC2 instance. 🎉  

---

## 🔹 Shortcut Method
Instead of copying snapshots manually:
- From the **unencrypted snapshot**, directly choose **Create Volume**.  
- On the fly, select **Enable Encryption** + KMS key.  
- This creates an **encrypted EBS volume** right away.  

---

## 🔹 Clean-up
- Don’t forget to **delete old snapshots and volumes** when done.  
- Helps avoid unnecessary storage costs.  

---

## 🧠 Key Takeaways
- All **new EBS volumes can (and should) be encrypted** by default.  
- **Snapshots inherit encryption** status.  
- To encrypt an existing unencrypted volume:
  - Snapshot → Copy (with encryption) → Create new volume.  
- Shortcut: Enable encryption **directly while creating a volume from snapshot**.  

---

📌 *Learning this gave me a clear step-by-step process for handling encrypted vs unencrypted EBS volumes. The snapshot → copy → new volume workflow is a must-know for AWS exams and real-world usage.*