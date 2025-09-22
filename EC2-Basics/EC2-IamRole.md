# 🚀 Learning IAM Roles with EC2

**IAM roles for EC2 instances**.  
It highlights why we should never store credentials manually on an instance and how to securely grant permissions using IAM roles.

---

## 🔑 Key Learnings

1. **Connecting to EC2**
   - Accessed my EC2 instance using **EC2 Instance Connect** (or SSH/PuTTY would also work).
   - Verified connectivity with basic Linux commands (e.g., `ping google.com`).

2. **The Wrong Way: `aws configure`**
   - Running `aws iam list-users` without credentials fails.
   - While we *could* set up credentials using `aws configure`, it is **extremely insecure**:
     - Credentials could be stolen by anyone with instance access.
     - Never hardcode or store IAM keys on EC2.

3. **The Right Way: IAM Roles**
   - Created an IAM role (`DemoRoleForEC2`) with the **IAMReadOnlyAccess** policy.
   - Attached the role to the EC2 instance via:
     - **Instance → Actions → Security → Modify IAM Role**.
   - Verified that commands now worked:
     ```bash
     aws iam list-users
     ```
     ✅ Successfully retrieved IAM users without needing `aws configure`.

4. **Testing Permissions**
   - Detached the IAM policy → got **Access Denied** as expected.
   - Re-attached IAMReadOnlyAccess → regained access.
   - Learned that IAM policy changes may take a few moments to propagate.

---

## 📝 Takeaways

- ✅ **Always use IAM roles** for EC2 (never store access keys).  
- ✅ Roles provide **temporary, secure credentials** managed by AWS.  
- ✅ IAM role attachment/detachment directly controls EC2 permissions in real time.  

---

## 📚 Reflection

This exercise reinforced a critical AWS best practice:  
> **IAM roles are the secure, scalable way to give EC2 instances access to AWS services.**  

No keys to rotate, no secrets to leak — just safe, temporary credentials managed by AWS itself. 🎯
