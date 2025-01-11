For this short exercise I created a VPC inside AWS. 

The purpose of this exercise was to familiarize myself with creating a VPC and understand the functionality and importance of this feature.

**What is a VPC?**
A VPC is a way to secure resources inside the AWS cloud by nesting them inside of a network. 
**Why do we need them?**
1. To control network flow in and out of our resources
2. To secure and/or isolate our resources.

Creating your own VPC inside AWS is best practice for several reasons
1. Defining custom IP ranges
2. Segmenting our network into public or private sections.
3. Custom creation of security rules.


In this short exercise we created a VPC illustrated here: 

![image](https://github.com/user-attachments/assets/ef4ac8ac-d311-49d7-8554-4040f2b5e28a)

I assigned the CIDR block of 10.0.0.0/16 which is a standard default CIDR due to the large range of IP addresses it can provide.

I then created a two subnets, one private 

![image](https://github.com/user-attachments/assets/790dbbdf-6588-421a-8483-63c0edd392df)

and one public.

![image](https://github.com/user-attachments/assets/8600b30a-46b3-4da3-9474-944a435e931a)


However, as you can see the CIDR for this public subnet is set to 10.0.0.0/24. This is a **mistake** because there is now an overlap between the VPC ip ranges and the public subnet. This can cause potential complications such as routing conflicts which could result in security breaches or even breaking applications. 

![image](https://github.com/user-attachments/assets/d7cf9339-6770-4b4f-9e2d-a656592be72c)

Changing the CIDR to 10.0.1.0/24 ensures that the VPC, public subnet and private subnet all share distinct ip ranges and is ultimately best practice.


