**AWS EC2 and S3 Integration Project**

Overview

This project demonstrates the setup and configuration of an AWS environment that enables an EC2 instance to securely interact with an S3 bucket. Key steps include creating a custom VPC, configuring networking components, attaching IAM roles, and performing S3 operations like uploading, downloading, and syncing files.

**Project Objectives**

1. *Create and configure a secure and scalable AWS environment using best practices.*

2. *Launch an EC2 instance within a custom VPC and establish SSH access.*

3. *Use IAM roles for secure, temporary access to S3 without storing credentials.*

4. *Perform S3 operations, including file upload, download, and synchronization.*

5. *Troubleshoot and resolve challenges related to IAM roles, permissions, and networking.
Architecture Diagram*


Steps Completed
1. **VPC and Networking Setup**
* Created a custom VPC with a CIDR block of 10.0.0.0/16.
* Added a public subnet with CIDR block 10.0.1.0/24 and attached it to a route table.
* Configured the route table with a route (0.0.0.0/0) pointing to an Internet Gateway for public internet access.
![image](https://github.com/user-attachments/assets/c04b918c-93f9-4c0e-b6e4-d39f8e9ba27e)

2. **Launching an EC2 Instance**
* Launched an Amazon Linux 2023 instance in the public subnet.
* Enabled Auto-Assign Public IP to ensure connectivity.
* Attached a security group with the following rules:
    * Inbound Rule: SSH (Port 22) from the CloudShell public IP.
    * Outbound Rule: Default for internet access.
* Connected to the instance using SSH from AWS CloudShell.
* Verified internet connectivity via ping google.com.
![image](https://github.com/user-attachments/assets/537e7a49-e089-43cb-9894-f86a6b9d5382)


3. **Attaching an IAM Role**
Created and attached an IAM role (EC2-S3-ReadOnlyRole) to the EC2 instance.
Initially attached the AmazonS3ReadOnlyAccess managed policy.
Updated the policy to a custom JSON policy to allow additional actions:


```{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::testbucket997766",
                "arn:aws:s3:::testbucket997766/*"
            ]
        }
    ]
}
```

Verified permissions by performing S3 operations.

4. **S3 Bucket Configuration**
Created an S3 bucket named testbucket997766.
Configured the bucket with default settings.
Verified bucket accessibility from the EC2 instance.
[Screenshot to include: S3 bucket overview showing its name and region]

5. S3 Operations

Created a test file on the EC2 instance:
```
echo "test" > testfile.txt
```
Uploaded the file to S3:
```
aws s3 cp testfile.txt s3://testbucket997766/
```
Verified the file in the S3 bucket:
```
aws s3 ls s3://testbucket997766/
```

Created a folder on the EC2 instance:
```
mkdir test-folder
echo "test1" > test-folder/file1.txt
echo "test2" > test-folder/file2.txt
```
Synced the folder to S3:
```
aws s3 sync test-folder/ s3://testbucket997766/
```
Verified all files were uploaded:
```
aws s3 ls s3://testbucket997766/
```

![image](https://github.com/user-attachments/assets/5a1aacc5-73d6-4868-8fe5-58b15a02f083)

Downloaded a file from S3 to the instance:
```
aws s3 cp s3://testbucket997766/file1.txt downloaded-file1.txt
```
Verified the file contents:
```
cat downloaded-file1.txt
```
**Challenges and Troubleshooting**

**AccessDenied Errors:**

Encountered AccessDenied for s3:PutObject due to insufficient IAM permissions.
  * Resolved by attaching a custom policy with the required permissions.
    
**Empty Folder Issue:**

The folder was initially empty, causing aws s3 sync to appear successful but upload nothing.
  * Resolved by populating the folder with test files.

**Dynamic CloudShell IP:**

SSH access failed after the CloudShell IP changed.
  * Resolved by updating the security group to allow the new IP.

**Networking Misconfigurations:**
Verified and corrected VPC, subnet, and route table configurations to ensure public internet access.

**Key Learnings**

**IAM Roles:**
How to securely attach roles to EC2 instances for temporary access to S3.

**Networking:**
Importance of correctly configuring subnets, route tables, and Internet Gateways.

**S3 Operations:**
Practical usage of aws s3 commands for file uploads, downloads, and synchronization.

**Troubleshooting:**
Real-world problem-solving with permissions, networking, and command usage.

Next Steps
* Implement advanced S3 features like versioning, lifecycle policies, and encryption.
* Automate the setup process using AWS CloudFormation or Terraform.
* Expand this project to include more services like Lambda, DynamoDB, or CloudFront.
