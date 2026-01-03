# VPC and Subnets – CloudFormation Documentation

## Overview

This step provisions the **network foundation** for the project using AWS CloudFormation. It creates a Virtual Private Cloud (VPC) with **two public subnets** and **two private subnets** across two Availability Zones (AZs), including basic internet routing for public resources.

This network layer is designed to support:

* **EC2 instances** in public subnets
* **RDS MySQL databases** in private subnets
* Future integration with **security groups** and **monitoring services**

> Note: NAT Gateway is **not included** in this step. EC2 in public subnets can communicate with RDS in private subnets directly using their private IPs.

---

## Resources Created

### 1. VPC

* CIDR block defined via parameter (`VpcCidr`)
* DNS support and DNS hostnames enabled (required for RDS endpoints)
* Tagged with project name and environment

### 2. Subnets

#### Public Subnets

* **PublicSubnet1** → AZ1
* **PublicSubnet2** → AZ2
* Automatically assigns public IPs to instances
* Intended for internet-facing resources (EC2)

#### Private Subnets

* **PrivateSubnet1** → AZ1
* **PrivateSubnet2** → AZ2
* No automatic public IPs
* Intended for internal resources (RDS)

### 3. Internet Gateway

* Attached to the VPC
* Enables outbound and inbound internet access for public subnets

### 4. Public Route Table

* Routes `0.0.0.0/0` traffic to the Internet Gateway
* Associated with both public subnets

---

## Parameters Used

* `VpcCidr` – CIDR range for the VPC
* `PublicSubnet1Cidr` / `PublicSubnet2Cidr` – CIDR ranges for public subnets
* `PrivateSubnet1Cidr` / `PrivateSubnet2Cidr` – CIDR ranges for private subnets
* `AvailabilityZone1` / `AvailabilityZone2` – AZs where subnets are created
* `EnvironmentName` – Environment identifier (dev, prod, staging)
* `ProjectName` – Used for consistent resource tagging

---

## Outputs

The following values are exported for use by other CloudFormation stacks:

* **VPC ID**
* **Public Subnet IDs** (1 & 2)
* **Private Subnet IDs** (1 & 2)

These exports enable clean cross-stack references for EC2, RDS, and security stacks.

---

## Importance of This Step

* Establishes **network isolation and security boundaries**
* Enables a **multi-AZ architecture** for high availability
* Supports **multi-stack architecture** using CloudFormation exports
* Provides a foundation for EC2, RDS, and monitoring deployments
* Follows AWS best practices for public/private subnet separation
* NAT Gateway is optional and can be added later if private subnets need internet access

---

## Next Step

Create **security groups** and configure **EC2 and RDS deployments**, using this VPC and subnets as the network foundation