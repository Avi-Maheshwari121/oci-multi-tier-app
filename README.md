# OCI Multi-Tier Architecture Project

This repository documents a complete multi-tier cloud architecture deployed on Oracle Cloud Infrastructure (OCI).  
The goal of this project is to demonstrate practical Oracle Cloud Architect-level skills involving networking, compute, databases, load balancing, and security.

---

## 📌 Project Overview

This project includes:
- Virtual Cloud Network (VCN) design
- Public and private subnets
- Internet Gateway & NAT Gateway setup
- Compute instance (VM.Standard.A1.Flex)
- Load Balancer (HTTP)
- Autonomous Database (ATP)
- Network Security Groups / Security Lists
- Monitoring & Alarms
- Architecture diagrams
- Console screenshots for validation

---

## 📂 Repository Structure

architecture-diagrams/ → All architecture PNG diagrams
screenshots/ → All OCI console screenshots
instance-config/ → Server setup scripts & configs
terraform/ → Infrastructure as Code (optional)
README.md → Documentation


---

## 🚀 Current Progress

This section will be updated as we build the project:

## 🌐 Networking Architecture (Completed)

The networking layer for this OCI multi-tier architecture has been fully configured using best practices for tier isolation, security, and controlled connectivity.

### **VCN & Subnet Design**
- **VCN Name:** `project-vcn`
- **CIDR:** `10.0.0.0/16`
- **Public Subnet:** `10.0.1.0/24` (Load Balancer / Bastion / Public Tier)
- **Private Subnet:** `10.0.2.0/24` (Application Compute Instance)

This separates external-facing and internal workloads following standard cloud architectural patterns.

---

### **Gateways**
- **Internet Gateway:** Enables inbound/outbound access for public subnet.
- **NAT Gateway:** Allows private instances secure outbound access (updates/packages).
- **Service Gateway:** Provides private access to OCI services (Autonomous DB, Object Storage).

---

### **Route Tables**
- **Public Route Table:**  
  - `0.0.0.0/0 → Internet Gateway`
- **Private Route Table:**  
  - `0.0.0.0/0 → NAT Gateway`

This ensures private-tier resources remain unreachable from the public internet.

---

### **Security Configuration**
#### **Security Lists (Subnet-Level)**
- Minimal configuration to avoid conflicts:
  - **Ingress:** None
  - **Egress:** Allow all (`0.0.0.0/0`)

#### **Network Security Groups (Resource-Level)**
- **nsg-public**
  - Allow HTTP (80) from anywhere
  - Allow HTTPS (443) from anywhere
  - Allow SSH (22) only from admin IP
- **nsg-private**
  - Allow HTTP only from the public tier / load balancer
  - Allow internal DB port traffic when needed
  - Allow outbound internet via NAT Gateway

NSGs enforce least-privilege and clean tier isolation.

---

### **Status**
- ✔️ VCN created  
- ✔️ Subnets configured  
- ✔️ Gateways deployed  
- ✔️ Route tables applied  
- ✔️ Security Lists configured  
- ✔️ Network Security Groups created  
- ⏳ NSGs will be attached to compute and load balancer during deployment  

All networking screenshots are available under `/screenshots/`.

---




---

## 🔗 About This Project

This project is part of my Oracle Cloud Infrastructure Architect learning path and demonstrates real hands-on implementation of cloud architecture components.
