# AWS VPC Peering Lab

## Project Overview

This project demonstrates private communication between two Amazon Virtual Private Clouds (VPCs) using VPC Peering. Private EC2 instances communicate using private IPv4 addresses without requiring public IP addresses on the application instances.

## Objectives

- Create two VPCs with non-overlapping CIDR blocks.
- Configure public and private subnets and route tables.
- Establish a VPC peering connection.
- Configure routes for communication between VPCs.
- Use a bastion host to access a private EC2 instance.
- Test connectivity and demonstrate security-group-based traffic restriction.

## Architecture

- **VPC A:** `demo-vpc`
- **VPC A CIDR:** `10.0.0.0/16`
- **VPC B:** `EWB-VPC-B`
- **VPC B CIDR:** `10.1.0.0/16`
- **Bastion host:** `10.0.1.12`
- **App A private IP:** `10.0.2.254`
- **App B private IP:** `10.1.1.194`
- **Region:** Europe (Stockholm), `eu-north-1`

## AWS Services Used

- Amazon VPC
- VPC Peering
- Amazon EC2
- Route Tables
- Security Groups
- EC2 Instance Connect

## Network Configuration

The VPCs use separate CIDR ranges to avoid overlap. The private route tables include routes to the other VPC through the active VPC peering connection.

- VPC A private route: `10.1.0.0/16` → VPC peering connection
- VPC B private route: `10.0.0.0/16` → VPC peering connection

## Security Configuration

App B's security group allows SSH and ICMP traffic from the App A subnet (`10.0.2.0/24`) for the lab tests.

The bastion host provides an entry point to access App A. Private application instances communicate through their private IP addresses.

## Testing and Results

### Successful Connectivity

From App A, the following command was executed:

`ping -c 4 10.1.1.194`

Result: 4 packets transmitted, 4 received, 0% packet loss.

### Controlled-Failure Test

The ICMP inbound rule was temporarily removed from App B's security group. Ping requests then failed. After correcting and restoring the ICMP rule, connectivity was successful again.

This demonstrates how security-group rules control network traffic independently of the VPC peering routes.

## Key Learnings

- Configuring VPCs, subnets, and route tables.
- Establishing VPC peering between non-overlapping networks.
- Accessing private EC2 instances through a bastion host.
- Troubleshooting network connectivity and security-group rules.
- Validating private communication using ICMP.

## Evidence

Screenshots of the VPC peering status, route tables, security-group rules, and connectivity tests will be included in this repository.
