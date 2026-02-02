# 📘 Day 22: Configuring Secure SSH Access to an EC2 Instance

---

## 📝 Task Details 

The Nautilus DevOps team needs to set up a new EC2 instance that can be accessed securely from their landing host (**aws-client**).

### Requirements:

* Create an EC2 instance named **datacenter-ec2**
* Instance type must be **t2.micro**
* Create a new SSH key named **id_rsa** on **aws-client** under:

```
/root/.ssh/
```

* Add the public key to the **root user’s authorized_keys** on the EC2 instance
* Enable **passwordless SSH access** from aws-client

---

# 🧠 Concept (Short & Clear)

SSH keys allow secure login without passwords using:

🔑 Private key → client
🔓 Public key → server

Stored in:

```
/root/.ssh/authorized_keys
```

---

# 🚀 STEP 1: Launch EC2 Instance (AWS Console)

### Go to:

EC2 → Launch Instance

### Configure:

| Setting        | Value               |
| -------------- | ------------------- |
| Name           | datacenter-ec2      |
| AMI            | Amazon Linux 2023   |
| Instance type  | t2.micro            |
| Security Group | Allow SSH (port 22) |
| Key pair       | None required       |

Click **Launch**

---

# 🔐 STEP 2: Generate SSH Key on aws-client

```bash
cd /root/.ssh
ssh-keygen
```

Press ENTER for all prompts.

Verify:

```bash
ls
```

You should see:

```
id_rsa
id_rsa.pub
```

---

# 📄 STEP 3: Copy Public Key

```bash
cat id_rsa.pub
```

Copy the output.

---

# 🖥️ STEP 4: Login to EC2 Instance

```bash
ssh root@<EC2_PUBLIC_IP>
```

(or login as ec2-user then sudo -i)

---

# 📂 STEP 5: Add Key to authorized_keys

```bash
sudo -i
cd /root/.ssh
vim authorized_keys
```

Paste public key → save.

---

# 🔁 STEP 6: Test Passwordless SSH

From aws-client:

```bash
ssh root@<EC2_PUBLIC_IP>
```

✅ Logs in without password

---

# ✅ Verification Checklist

✔ EC2 running
✔ SSH allowed in SG
✔ id_rsa created on aws-client
✔ public key in authorized_keys
✔ SSH connects directly

---

# 🎯 Common Mistakes

❌ Using instance ID instead of IP
❌ Forgetting port 22 rule
❌ Adding private key instead of public key
❌ Wrong permissions

---

# 📌 Quick Revision Flow

```
Launch EC2
↓
ssh-keygen on aws-client
↓
copy id_rsa.pub
↓
paste into /root/.ssh/authorized_keys on EC2
↓
SSH works
```

---


