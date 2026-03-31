# Secure Private Infrastructure Deployment with Bastion Access and IAM Role-Based Architecture

## Project Overview

This project demonstrates a secure AWS infrastructure where:

* Application servers run in private subnets
* No public IP is assigned to backend servers
* Access is allowed only via a Bastion Host
* IAM Roles are used instead of access keys
* Proper network isolation is enforced

---

## Architecture Diagram

<img width="853" height="338" alt="image" src="https://github.com/user-attachments/assets/506d3e48-d439-4064-bcad-9761e6fdc867" />

<img width="852" height="362" alt="image" src="https://github.com/user-attachments/assets/67a785d1-fd71-4935-be7c-c972ce48059a" />


The architecture includes:

* 1 Custom VPC
* 2 Public Subnets
* 2 Private Subnets
* Internet Gateway
* NAT Gateway (in Public Subnet)
* Bastion Host
* Private Application Server

### Traffic Flow

* User → Internet → Internet Gateway → Bastion Host → Private EC2
* Private EC2 → NAT Gateway → Internet

---

## Network Configuration

### Public Route Table

<img width="839" height="359" alt="image" src="https://github.com/user-attachments/assets/1e47c0e3-1c3d-4a69-88ce-5907b81b2a65" />


* `0.0.0.0/0 → Internet Gateway`

### Private Route Table


<img width="831" height="360" alt="image" src="https://github.com/user-attachments/assets/4aca6c9d-7777-4d1b-b694-024e1817a392" />


* `0.0.0.0/0 → NAT Gateway`

---

## Security Groups Configuration

### Bastion Security Group

<img width="836" height="364" alt="image" src="https://github.com/user-attachments/assets/168a4095-9680-4747-9a0c-34392ec15308" />


* SSH (Port 22) allowed only from my IP
* All outbound traffic allowed

### Private EC2 Security Group

<img width="857" height="377" alt="image" src="https://github.com/user-attachments/assets/78f03c58-891a-4917-ae22-42244bb4beb6" />


* SSH (Port 22) allowed only from Bastion Security Group
* HTTP (Port 80) allowed internally only

---

## Security Validation

### Direct SSH from Local Machine (Blocked)

* Private instance is not accessible directly from the internet

### Bastion to Private EC2 Access (Successful)

* Access to private instance is only possible via Bastion Host

---

## IAM Role Implementation

### IAM Role Attached to EC2

* Role: `EC2-S3-Role`
* Policy: `AmazonS3ReadOnlyAccess`

### S3 Access without Access Keys

* AWS CLI successfully lists S3 buckets using IAM Role
* No credentials are stored inside the server

---

## Instance Details

### Bastion Host

* Deployed in Public Subnet
* Used for administrative access

### Private Application Server

* Deployed in Private Subnet
* No public IP assigned

---

## Architecture Explanation

* Backend application servers are deployed inside private subnets without public IP addresses
* Administrative access is controlled through a Bastion Host in a public subnet
* Outbound internet traffic from private instances is routed via a NAT Gateway
* IAM Roles are used to securely grant AWS permissions without using access keys

---

## Security Design Decisions

* No public IP for backend servers
* Least privilege IAM policy
* SSH restricted to a specific IP
* No hardcoded credentials
* Separate route tables for network isolation
* NAT Gateway for controlled outbound access

---

## Lessons Learned

* Importance of subnet isolation
* Difference between NAT Gateway and Internet Gateway
* IAM Roles improve security
* Security Groups act as virtual firewalls
* Bastion Host improves access control
* Least privilege reduces attack surface

---

## Project Structure

```
Project/
 ├── README.md
 ├── Screenshots/
 │     ├── Architecture.png
 │     ├── Public-Route-Table.png
 │     ├── Private-Route-Table.png
 │     ├── Bastion-SG.png
 │     ├── Private-SG.png
 │     ├── Direct-SSH-Fail.png
 │     ├── Bastion-to-Private.png
 │     ├── IAM-Role.png
 │     ├── S3-Access.png
```

---

## Project Status

✔ Secure Infrastructure Implemented
✔ Bastion-based Access Control
✔ IAM Role-Based Authentication
✔ Production-Level Security Architecture

---

## About

This project showcases a production-level secure AWS architecture using best practices for networking, access control, and identity management.

---

## Contributor

* Riyaj Kalawant

---
