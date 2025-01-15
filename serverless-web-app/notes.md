![image](https://github.com/user-attachments/assets/2507d63d-2a78-4424-91d8-e0f7d1731615)

testing private instance to determine natgateway config using session manager

1. VPC Setup
Created a VPC named Tutoring-Website-VPC with a CIDR block of 10.0.0.0/16.
Added subnets:
Public Subnet: 10.0.1.0/24.
Private Subnet: 10.0.2.0/24.
Configured networking components:
Internet Gateway (IGW): Attached to the VPC for internet access.
NAT Gateway (NATGW): Allows private instances to access the internet securely.
Configured route tables:
Public subnet routes traffic through the IGW.
Private subnet routes traffic through the NATGW.
2. IAM and Security
Created an IAM role with the AmazonSSMManagedInstanceCore policy for secure and keyless management of instances.
Attached the role to the EC2 instances for Session Manager access.
Verified secure and private access:
Successfully accessed a private instance using Session Manager.
Confirmed the private instance has internet connectivity via the NAT Gateway.
3. CloudTrail Configuration
Enabled CloudTrail for logging AWS API activity.
Configured an S3 bucket to store CloudTrail logs (cherry-cloudtrail-logs).
Ensured all management events (Read/Write) are logged for auditing.
4. Static Website Hosting
Created an S3 bucket named cherry-site-frontend for static website hosting.
Enabled Static Website Hosting with:
index.html as the entry point.
Uploaded index.html to the S3 bucket.
Configured bucket policies and public access settings:
Granted public GetObject permissions for website files.
Verified that index.html is accessible via the browser.
