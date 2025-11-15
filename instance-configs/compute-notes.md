# Compute Tier Notes

This document summarizes the design decisions, configuration details, and reasoning behind the compute layer of the OCI Multi-Tier Architecture Project.

---

## 1. Architecture Overview

The compute tier consists of two components:

### **1. Jump Host (Public Subnet)**
A secure entry point into the VCN since OCI Bastion is disabled in this student tenancy.

- Subnet: `public-subnet (10.0.1.0/24)`
- Purpose: SSH access into private-tier resources
- NSG: `nsg-public` (allows SSH only from admin IP)
- Public IP: Yes (required for external SSH)

### **2. Application Server (Private Subnet)**
The backend application server deployed in a fully isolated network.

- Subnet: `private-subnet (10.0.2.0/24)`
- Public IP: None (zero exposure to the internet)
- NSG: `nsg-private` (allows HTTP/SSH only from internal sources)
- Purpose: Web backend for the Load Balancer

This layout enforces proper tier separation and minimizes the attack surface.

---

## 2. Why a Jump Host Instead of OCI Bastion?

OCI Bastion Service is restricted in this student tenancy (quota limitations).  
To maintain secure access, a **Jump Host pattern** was implemented.

### **Benefits of Jump Host Approach**
- Maintains strict security boundaries
- Industry-standard alternative to managed bastion services
- Ensures private instances remain inaccessible from the public internet
- Works seamlessly within student tenancy restrictions

### **Access Flow**
Laptop → SSH (public IP) → Jump Host → SSH (private IP) → app-server-1



---

## 3. NGINX Deployment on Private Instance

NGINX is used as the backend web service for load balancing.

### **Commands Executed**
```bash
sudo dnf install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
echo "<h1>Welcome from app-server-1 (OCI Private Subnet)</h1>" | sudo tee /usr/share/nginx/html/index.html

Reasons for Choosing NGINX

Lightweight and widely used in production

Perfect for demonstrating backend-tier deployment

Works seamlessly behind Load Balancers

Easy to verify via custom test pages

The successful setup confirms:

Package installation via NAT works

Service runs correctly in private subnet

Instance is ready to be added to Load Balancer backend set

4. Security Design
Jump Host Security

SSH allowed only from admin external IP

Only port 22 exposed publicly

Controlled and isolated from private-tier resources

Private Instance Security

No public IP assigned

SSH allowed only from Jump Host subnet (10.0.1.0/24)

HTTP allowed only from Load Balancer tier

Outbound internet access via NAT Gateway

This ensures strong tier isolation and least-privilege access.

5. Validation Checklist

All compute-tier validations completed:

SSH from laptop → jump host: ✔ Successful

SSH from jump host → private instance: ✔ Successful

NGINX active and running: ✔ Verified

Custom index page deployed: ✔ Confirmed

NSG rules functioning correctly: ✔ Confirmed

Subnet routing and NAT access validated: ✔ Working

The compute layer is fully operational.

6. Next Steps

The next phase focuses on the public-facing layer:

Deploy Public Load Balancer in public-subnet

Create Backend Set (HTTP)

Register app-server-1 via private IP

Configure Load Balancer health checks

Add Listener on port 80

End-to-end public testing

Once complete, the full multi-tier architecture will be live.