# 🛡️ Secure AWS Infrastructure Monitoring and IAM Governance Lab

## 📖 Project Overview

This project demonstrates the design and implementation of a secure AWS environment using private networking, IAM governance, centralized logging, and audit monitoring.

The objective was to simulate real-world cloud security and operations practices by deploying workloads inside a private subnet while maintaining secure administrative access and visibility into both system-level and AWS account-level activities.

The project combines AWS networking, identity and access management, logging, and auditing services to create a production-style monitoring environment.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/aedf248c-7034-4fb6-a120-9428edb77220" />


---

## 🎯 Objectives

✅ Design a secure AWS network architecture

✅ Implement public and private subnet separation

✅ Configure secure access to private EC2 instances using a Bastion Host

✅ Apply IAM roles, users, groups, and least-privilege permissions

✅ Centralize operating system logs using Amazon CloudWatch Logs

✅ Monitor AWS API activities using AWS CloudTrail

✅ Enable log collection from private EC2 instances without internet access using VPC Endpoints

✅ Understand the difference between infrastructure-level and operating system-level monitoring

---

## 🏗️ Architecture Components

### 🌐 Networking

* Amazon VPC
* Public Subnet
* Private Subnet
* Internet Gateway
* NAT Gateway
* Route Tables
* Security Groups

<img width="2375" height="402" alt="image" src="https://github.com/user-attachments/assets/906569c4-b949-4351-8c43-c0c6987d2102" />

<img width="2489" height="618" alt="image" src="https://github.com/user-attachments/assets/98d53ffc-a314-4c33-bcde-2a5aad574a62" />


### 💻 Compute

* Bastion Host EC2 Instance
* Private EC2 Instance

* Accessing private ec2 instance from bastion host:
<img width="2876" height="1094" alt="image" src="https://github.com/user-attachments/assets/88fb88a7-9789-443e-9a00-83de2080e96a" />


### 🔐 Identity & Access Management

* IAM Users
<img width="2404" height="850" alt="image" src="https://github.com/user-attachments/assets/27cec26a-b016-4b93-bf50-6ea02b1b4de8" />

* IAM Groups
<img width="2414" height="571" alt="image" src="https://github.com/user-attachments/assets/20894dd0-818f-4740-9927-e11256b5da0e" />

* IAM Policies

for Admins:
<img width="2392" height="459" alt="image" src="https://github.com/user-attachments/assets/049e21b0-64b4-418c-bde8-47e722cd419d" />

for Developers:
<img width="2427" height="967" alt="image" src="https://github.com/user-attachments/assets/0d7f6997-3580-435e-94cb-e8e6a88058f8" />

for Interns:
<img width="2433" height="918" alt="image" src="https://github.com/user-attachments/assets/0da15cd5-67b6-4d80-90f0-a619016dda6d" />

for Security Team:
<img width="2421" height="905" alt="image" src="https://github.com/user-attachments/assets/d73d2a1a-a12d-4b72-b0e2-35b1c069f807" />

* IAM Roles

Roles Attached to EC2:
<img width="2355" height="421" alt="image" src="https://github.com/user-attachments/assets/d1546f5f-66f3-4dc5-9c8c-b0125ec9bd88" />




### 📊 Monitoring & Logging

* Amazon CloudWatch Logs
* CloudWatch Agent
* AWS CloudTrail

*log group (cloudtrail-security-logs):

<img width="2435" height="1143" alt="image" src="https://github.com/user-attachments/assets/625553f2-3a42-47b6-a559-61298226166f" />

*log group (private-ec2-logs):

<img width="2499" height="1150" alt="image" src="https://github.com/user-attachments/assets/36e501b0-2dca-448f-8f77-111cec4722a8" />



### 🔗 Private Connectivity

* STS VPC Endpoint
* CloudWatch Logs VPC Endpoint

*Sending a message to log stream of private-ec2-logs using sts vpc endpoint from a private ec2 instance without using a NAT Gateway:
<img width="2878" height="265" alt="image" src="https://github.com/user-attachments/assets/cf2f401e-4751-4c80-ad70-73aff298fb77" />
*log stream:
<img width="1686" height="172" alt="image" src="https://github.com/user-attachments/assets/9f9a3615-77e2-48ae-8f38-01465821aea1" />






---

## 🏛️ Architecture Flow

```text
Internet User
      │
      ▼
🌐 Bastion Host (Public Subnet)
      │ SSH
      ▼
🔒 Private EC2 (Private Subnet)
      │
      ▼
📊 CloudWatch Agent
      │
      ▼
☁️ CloudWatch Logs

AWS Console Actions
      │
      ▼
📜 CloudTrail
      │
      ▼
📂 CloudTrail Log Group
```

---

## ⚙️ Implementation Steps

### 🏗️ 1. VPC Deployment

Created a custom VPC with:

* Public Subnet
* Private Subnet
* Internet Gateway
* Route Tables

### 🔑 2. Bastion Host Setup

Configured a public EC2 instance to provide secure SSH access to the private EC2 instance.

### 🔒 3. Private EC2 Deployment

Launched a private EC2 instance without a public IP address.

### 👥 4. IAM Configuration

Configured:

* IAM Users
* IAM Groups
* IAM Policies
* IAM Roles

Implemented least-privilege access controls for testing authorization scenarios.

### 📊 5. CloudWatch Agent Installation

Installed and configured CloudWatch Agent on the private EC2 instance.

Configured log collection from:

```bash
/var/log/cloud-init-output.log
```

### 📜 6. CloudTrail Configuration

Enabled CloudTrail log delivery to CloudWatch Logs to monitor AWS API activities.

### 🔗 7. VPC Endpoint Configuration

Created:

* STS Interface Endpoint
* CloudWatch Logs Interface Endpoint

Verified that CloudWatch logging continued to function after NAT Gateway removal.

---

## 🧪 Key Experiments Performed

### 🔒 Experiment 1: Private EC2 Access

Verified secure access to private EC2 through Bastion Host.

### 👤 Experiment 2: IAM Authorization Testing

Tested user permissions using IAM users and groups.

Observed allowed and denied actions.

### 📜 Experiment 3: CloudTrail Monitoring

Performed actions such as:

* Start EC2
* Stop EC2
* Modify Security Groups
* Attach IAM Policies

Observed corresponding CloudTrail events.

### 📊 Experiment 4: CloudWatch Log Collection

Generated operating system log entries and verified delivery to CloudWatch Logs.

### 🚫🌐 Experiment 5: NAT Gateway Removal

Verified successful log delivery using VPC Endpoints without internet access.

This demonstrated private AWS service communication using AWS PrivateLink.

---

## 🛡️ Security Concepts Demonstrated

* Principle of Least Privilege
* Network Segmentation
* Bastion Host Architecture
* IAM Role-Based Access Control
* Centralized Logging
* Audit Logging
* Private AWS Service Connectivity
* Secure Management of Private Resources

---

## 🚧 Challenges Encountered

### ❌ Incorrect Log Source

Initially configured CloudWatch Agent to monitor a log file that did not exist on Amazon Linux 2023.

### 🔑 IAM Credential Issues

Resolved CloudWatch Agent authentication issues using an EC2 IAM Role.

### ⚙️ Configuration Troubleshooting

Identified and corrected active CloudWatch Agent configuration files.

### 🔗 Private Connectivity Validation

Successfully validated CloudWatch log delivery through VPC Endpoints after removing NAT Gateway dependency.

---

## 🎉 Results

✅ Successfully implemented a secure AWS environment

✅ Centralized operating system logs using CloudWatch Logs

✅ Audited AWS API activities using CloudTrail

✅ Demonstrated IAM governance and access control

✅ Validated private communication with AWS services through VPC Endpoints

✅ Built a production-style monitoring and security lab environment

---

## 🚀 Future Enhancements

* CloudWatch Alarms
* Amazon SNS Notifications
* AWS GuardDuty
* AWS Security Hub
* EventBridge Automation
* S3 Log Archival
* SIEM Integration

---

## 🏆 Skills Demonstrated

**AWS • IAM • EC2 • VPC • Bastion Host • NAT Gateway • CloudWatch • CloudTrail • AWS PrivateLink • VPC Endpoints • Linux • Security Monitoring • Troubleshooting • Cloud Security**

