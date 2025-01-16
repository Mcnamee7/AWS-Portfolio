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



Project Progress: Tutoring Website Setup
Here’s a detailed breakdown of the actions we’ve taken so far, including more technical insights for each step:

1. Created the VPC
Objective: Establish a secure and scalable network for the tutoring website infrastructure.
Actions Taken:
Created a VPC with a CIDR block of 10.0.0.0/16, providing 65,536 IP addresses for resources.
Added:
Public Subnet: 10.0.1.0/24, for resources that need internet access.
Private Subnet: 10.0.2.0/24, for backend resources without direct internet exposure.
Configured an Internet Gateway to provide internet access to the public subnet.
Set up a NAT Gateway in the public subnet to allow private subnet resources to access the internet securely.
2. Provisioned EC2 Instances
Objective: Deploy compute resources for testing and accessing the private subnet.
Actions Taken:
Launched:
Public Instance: Assigned a public IP for internet access, used as a jump server.
Private Instance: No public IP, accessed securely through the public instance or AWS Session Manager.
Verified connectivity:
Confirmed the private instance had internet access via the NAT Gateway.
Implemented best practices:
Used AWS Systems Manager Session Manager to securely access the private instance without exposing SSH keys or opening port 22.
3. Set Up S3 Bucket for Frontend
Objective: Host the website’s frontend using S3 as a static website.
Actions Taken:
Created an S3 bucket named cherry-site-frontend.
Enabled Static Website Hosting:
Defined index.html as the default root object.
Uploaded index.html to the bucket.
Ensured that the content was accessible by CloudFront via bucket policies.
4. Configured Bucket Policy and Permissions
Objective: Secure the S3 bucket and ensure it’s accessible only through CloudFront.
Actions Taken:
Configured the following bucket policy:
json
Copy code
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::cherry-site-frontend/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::<account-id>:distribution/<distribution-id>"
        }
      }
    }
  ]
}
Enabled Block Public Access to prevent unauthorized public access.
Verified that only CloudFront (via Origin Access Control) could access objects in the bucket.
5. Set Up CloudFront Distribution
Objective: Distribute static website content globally with caching, HTTPS, and improved performance.
Actions Taken:
Created a CloudFront distribution:
Origin: Configured S3 bucket as the origin.
Origin Access Control (OAC): Enabled secure access to S3 without making objects public.
Set Default Root Object to index.html to serve the website’s main page.
Enabled HTTPS:
Used AWS Certificate Manager (ACM) to configure an SSL/TLS certificate for secure connections.
Configured Caching:
Optimized cache behavior to improve response times for users.
6. Resolved Configuration Issues
Objective: Fix access and configuration issues for seamless website functionality.
Issues and Resolutions:
403 Access Denied Errors:
Identified that the bucket policy did not correctly allow CloudFront access.
Updated the bucket policy to grant permissions for the CloudFront distribution via OAC.
Intermittent Errors:
Discovered that /index.html as the default root object caused conflicts.
Corrected the setting to index.html (without /) to fix the issue.
7. Verified Distribution and Consistency
Objective: Ensure the website works consistently via CloudFront.
Actions Taken:
Accessed the website using the CloudFront URL (e.g., https://<distribution-ID>.cloudfront.net).
Verified:
Root URL (/) redirects to index.html.
Secure HTTPS connections are enforced.
Objects load correctly via CloudFront.
