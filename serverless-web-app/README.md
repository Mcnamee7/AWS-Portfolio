VPC Setup for Tutoring Website

This document outlines the steps taken to create a Virtual Private Cloud (VPC) on AWS for hosting a fully serverless tutoring website. The setup adheres to AWS best practices and prepares the foundation for scalable and secure hosting.

Project Overview

The tutoring website requires a secure and scalable network architecture to host serverless components, including:

Frontend: Static website hosted on S3 with CloudFront.

Backend: API Gateway and Lambda for business logic.

Database: DynamoDB or other serverless database solutions.

The VPC setup includes public and private subnets to manage traffic flow and ensure security.

VPC Configuration

1. VPC Creation

Name: Tutoring-Website-VPC

CIDR Block: 10.0.0.0/16

Purpose:

Logical isolation of resources.

Enables secure access control and routing for serverless components.

2. Subnet Configuration

Public Subnet

Name: Public-Subnet

Availability Zone: eu-west-1a

CIDR Block: 10.0.1.0/24

Purpose:

Hosts components requiring internet access (e.g., Internet Gateway).

Private Subnet

Name: Private-Subnet

Availability Zone: eu-west-1b

CIDR Block: 10.0.2.0/24

Purpose:

Securely hosts resources without direct internet access (e.g., DynamoDB, Lambda).

Networking Components

3. Internet Gateway

Purpose:

Enables internet access for resources in the public subnet.

Steps to Create:

Attach the Internet Gateway to the Tutoring-Website-VPC.

4. NAT Gateway

Purpose:

Allows instances in the private subnet to access the internet securely (e.g., for Lambda updates or accessing APIs).

Steps to Create:

Deploy the NAT Gateway in the public subnet.

Associate it with the private subnet routing table.

Routing Configuration

5. Routing Tables

Public Subnet Routing Table

Route:

0.0.0.0/0 → Internet Gateway

Purpose:

Directs traffic from the public subnet to the internet.

Private Subnet Routing Table

Route:

0.0.0.0/0 → NAT Gateway

Purpose:

Allows private subnet resources to access the internet securely without direct exposure.

Security Measures

6. IAM Roles and Policies

Principle of Least Privilege:

Only assign necessary permissions to resources.

Roles Created:

EC2 role with S3 read-only access (example role for logging purposes).

7. Logging and Monitoring

CloudTrail:

Enabled for tracking API activity across the account.

VPC Flow Logs:

Configured to capture traffic details for troubleshooting and monitoring.

Next Steps

Complete the setup of Internet and NAT Gateways.

Verify routing tables for accurate traffic flow.

Transition to serverless components (e.g., S3, Lambda, API Gateway).

Document additional serverless configurations and integrate them into the architecture.

Best Practices Followed

Used non-overlapping CIDR blocks to avoid conflicts.

Ensured separation of public and private resources for security.

Enabled logging for better troubleshooting and visibility.

Resources

AWS VPC Documentation

AWS Serverless Architecture Best Practices


