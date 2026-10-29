# AWS Identity and Access Management (IAM) Hands-On Lab

This repository provides structured, step-by-step documentation detailing practical implementations of **AWS Identity and Access Management (IAM)**, **Amazon EC2**, and **Amazon S3** configuration workflows.

## 🚀 Features
* Step-by-step user provisioning and credential management.
* Group-based permission structures using AWS-managed policies.
* Deployment of custom, least-privilege JSON policies.
* Secure cross-service authorization using IAM Roles on running EC2 instances.

## 📂 Project Structure
* `README.md` - Core comprehensive guide for setting up users, groups, custom policies, and roles.

## 🛠️ Deployment Steps

### 1. Lab 1: Group-Based Permissions Escalation
1. Navigate to **IAM ➔ Users** and create `user1` with a custom password. Leave standard permissions blank.
2. Log into the AWS Console using `user1` credentials inside an **Incognito window**.
3. Change region to **Mumbai** and try accessing EC2 to verify you receive an **"Access Denied"** error.
4. Back in the Admin console, go to **User Groups ➔ Create Group** named `HDFC_server_team`.
5. Attach the `AmazonEC2FullAccess` policy and add `user1` to this group. Refresh the user session to verify full EC2 control.

### 2. Lab 2: Deploying Custom S3 Policies
1. Launch an Amazon Linux `t3.micro` instance named `user1-test-ec2` with a 30GB volume.
2. From the Admin console, go to **IAM ➔ Policies ➔ Create Policy** for the **S3** service. 
3. Explicitly allow `List`, `Read`, `Write`, and `Tagging` permissions, then save it as `Besant-S3-policy-hdfc`.
4. Create `user2` and add them to a new group named `HDFC-storage-team` with your custom policy attached.
5. Log in as `user2` and create a private S3 bucket named `besant-7pm-demo-s3622` with **Block all public access** enabled.

### 3. Lab 3: Connecting EC2 to S3 via IAM Roles
1. Use **EC2 Instance Connect** to open a browser terminal for `user1-test-ec2` and switch to root (`sudo -i`).
2. Run `aws s3 ls` to confirm the instance throws an authentication error.
3. In the Admin console, go to **IAM ➔ Roles ➔ Create Role**, choosing **EC2** as the trusted service.
4. Attach `AmazonS3FullAccess` and `AmazonEC2FullAccess` policies, and name the role `besant7pm-ec2-to-s3-connect`.
5. Go to the EC2 Dashboard, select your instance, and click **Actions ➔ Security ➔ Modify IAM Role** to attach your new role.

## 🎯 Verification
Return to your active EC2 command-line session and re-run `aws s3 ls`. The command will now successfully list your S3 buckets without needing hardcoded credentials.

