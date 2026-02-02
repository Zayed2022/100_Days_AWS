# 📘 Day 22: Configuring Secure SSH Access to an EC2 Instance

---

## 📝 Task 

The Nautilus DevOps team needs to set up a new EC2 instance that can be accessed securely from their landing host (**aws-client**). The instance should be of type **t2.micro** and named **datacenter-ec2**.

A new SSH key with name **id_rsa** should be created on the **aws-client** host under the **/root/.ssh/** folder, if it doesn't already exist.

This key should then be added to the **root user's authorised keys** on the EC2 instance, allowing **passwordless SSH access** from the aws-client host.

---

# 🧠 WHY this is required

### Problems with password login:

❌ Easily hacked
❌ Not automation friendly
❌ Not industry standard

### SSH keys provide:

✅ High security
✅ Automation access
✅ No password storage

Used in:

* Jump servers
* CI/CD pipelines
* Production servers

---

# 🕒 WHEN this is used in real world

✔ Connecting DevOps servers to cloud VMs
✔ Ansible automation
✔ Jenkins deployments
✔ Backup servers

---

# ⚙️ HOW SSH key auth works (simple)

```
Client (aws-client) holds private key
Server (EC2) stores public key

If keys match → access allowed
```

Files:

| File            | Purpose             |
| --------------- | ------------------- |
| id_rsa          | private key         |
| id_rsa.pub      | public key          |
| authorized_keys | allowed public keys |

---

# 🚀 STEP 1 — Launch EC2 Instance (Console)

EC2 → Launch Instance

| Setting        | Value          |
| -------------- | -------------- |
| Name           | datacenter-ec2 |
| AMI            | Amazon Linux   |
| Type           | t2.micro       |
| Security Group | Allow SSH (22) |
| Key pair       | None           |

Launch instance.

---

# 🔐 STEP 2 — Create SSH key on aws-client

```bash
cd /root/.ssh
ssh-keygen
```

Press ENTER for all prompts.

Creates:

```
id_rsa
id_rsa.pub
```

---

# 📄 STEP 3 — Copy public key

```bash
cat id_rsa.pub
```

Copy output.

---

# 🖥️ STEP 4 — Login to EC2 (initial)

```bash
ssh root@EC2_PUBLIC_IP
```

(or `ec2-user` then `sudo -i`)

---

# 📂 STEP 5 — Add key to authorized_keys

```bash
cd /root/.ssh
vim authorized_keys
```

Paste public key → save.

---

# 🔁 STEP 6 — Test passwordless SSH

From aws-client:

```bash
ssh root@EC2_PUBLIC_IP
```

✅ Should connect directly.

---

# ✅ Verification

✔ EC2 reachable
✔ SSH port open
✔ id_rsa exists
✔ authorized_keys updated
✔ No password prompt

---

# ❗ Common mistakes

❌ Using instance ID instead of IP
❌ Private key pasted instead of public
❌ Port 22 closed
❌ Wrong user

---

# 📌 Quick revision flow

```
Launch EC2
→ ssh-keygen
→ copy id_rsa.pub
→ paste into authorized_keys
→ ssh works
```

---

# 💬 Interview line (perfect)

“SSH key-based authentication allows secure, passwordless access by matching client private key with server stored public key.”

---


