# Networking Setup Notes

This file documents the networking design and decisions for the OCI Multi-Tier Architecture Project.

## VCN & Subnets
- VCN: project-vcn (10.0.0.0/16)
- public-subnet: 10.0.1.0/24 (for LB and admin access)
- private-subnet: 10.0.2.0/24 (for compute/app servers)

## Gateways
- Internet Gateway for public-subnet
- NAT Gateway for private-subnet
- Service Gateway for Autonomous Database and Object Storage

## Security
- Security Lists kept minimal: egress allow-all
- NSGs enforce ingress rules:
  - nsg-public: HTTP/HTTPS + SSH
  - nsg-private: HTTP from LB, DB from app tier

## Rationale
- NSGs provide better granularity and logical grouping.
- Private subnet isolates compute resources from public internet.
- NAT ensures outbound updates while preventing inbound access.

This structure is ready for compute instance deployment in the private subnet.
