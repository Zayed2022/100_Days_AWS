# Task Name
Create an AWS EC2 Key Pair (Foundational Migration Task)
The Nautilus DevOps team is starting an incremental migration to the AWS Cloud. As a foundational security step, you must provide a way to securely access future cloud resources. Your objective is to create an Amazon EC2 Key Pair that will be used for SSH authentication.

**Details**
**Target Region**: us-east-1 (US East - N. Virginia)

**Key Pair Name**: datacenter-kp

**Key Pair Type**: RSA

**Private Key Format**: .pem (Standard for OpenSSH)

Environment: Perform this task using the provided AWS Management Console or the aws-client terminal.

---

## Method 1: Using the AWS Management Console

1. **Log in**: Use the credentials provided (Username: `kk_labs_user_922642`) to sign in to the AWS Console.
2. **Select Region**: Ensure you are in the **us-east-1 (N. Virginia)** region by checking the dropdown in the top right corner.
3. **Navigate to EC2**: Type **EC2** in the search bar and select the EC2 service.
4. **Find Key Pairs**: In the left-hand navigation pane, scroll down to the **Network & Security** section and click on **Key Pairs**.
5. **Create Key Pair**:
* Click the **Create key pair** button.
* **Name**: Enter `datacenter-kp`.
* **Key pair type**: Select **RSA**.
* **Private key file format**: Choose `.pem` (for OpenSSH) or `.ppk` (for PuTTY) depending on your local machine, though `.pem` is standard for DevOps environments.


6. **Save**: Click **Create key pair**. The browser will automatically download the private key file. Keep this safe!

---

## Method 2: Using the AWS CLI (Terminal)

If you prefer using the `aws-client` host provided in the KodeKloud lab, follow these commands:

1. **Configure Credentials**:
Run `showcreds` to get your keys, then configure the CLI:
```bash
aws configure

```


* **AWS Access Key ID**: (Paste from showcreds)
* **AWS Secret Access Key**: (Paste from showcreds)
* **Default region name**: `us-east-1`
* **Default output format**: `json`


2. **Create the Key Pair**:
Execute the following command to create the key and save the private key to a local file:
```bash
aws ec2 create-key-pair \
    --key-name datacenter-kp \
    --key-type rsa \
    --query 'KeyMaterial' \
    --output text > datacenter-kp.pem

```


3. **Verify**:
Check if the key pair exists in AWS:
```bash
aws ec2 describe-key-pairs --key-names datacenter-kp

```



---

### Key Requirements Checklist

| Requirement | Value |
| --- | --- |
| **Region** | us-east-1 |
| **Name** | `datacenter-kp` |
| **Type** | `RSA` |
