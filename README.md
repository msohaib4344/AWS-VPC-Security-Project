Overview  

Hey, I've designed a network demonstrating high availability and maximum security. This setup is for MegaCorp, which is located in the US East region. 

![vpc (6)](https://github.com/user-attachments/assets/31ff143b-06f8-4350-a121-a8d9954ab469)


 

Step-by-Step Guide to Creating a VPC with Public and Private Subnets 

 

Step 1: Create a VPC 

Navigate to the AWS Management Console and go to the VPC Dashboard. 

Click Create VPC. 

Fill in the details:  

Name: Megacorp  

IPv4 CIDR Block: 10.0.0.0/16 

 ![image](https://github.com/user-attachments/assets/197c76ea-8c98-4da9-88b6-157603445b6d)


Click Create VPC. 

 
Why This Step? 

A Virtual Private Cloud (VPC) is an isolated network within AWS where you can deploy resources securely. 

Defining a CIDR block (10.0.0.0/16) provides a large address range, which allows you to create multiple subnets within your VPC. 

Why This Is a Great Step: 

You get complete control over your network architecture. 

Isolation enhances security and helps you manage resources efficiently. 

 

 

 

 

 

 

 

Step 2: Create the Subnets 

1. Create the Web Servers Subnet (Public Subnet) 

Go to Subnets and click Create Subnet. 

Fill in the details:  

Name: WEB_Servers 

VPC: Select megacoprp 

Availability Zone: us-east-1f 

IPv4 CIDR Block: 10.0.1.0/24 
<img width="493" alt="2024-12-11 13_04_24-VPC _ us-east-1 - Brave" src="https://github.com/user-attachments/assets/1e92688e-684b-4192-a272-a47f7e2d5ca8" />


Click Create Subnet. 

 

 

Why This Step? 

This subnet is designated for web servers that need to be accessible from the internet. 

A public subnet allows resources to receive public IP addresses and communicate with the internet. 

Why This Is a Great Step: 

Placing web servers in a public subnet ensures they can handle client requests directly. 

Separating web servers from the database increases security by keeping different roles isolated. 

2. Create the Public Subnet 

Go to Subnets and click Create Subnet. 

Fill in the details:  

Name: App_server 

VPC: Select Megacorp 

Availability Zone: us-east-1b 

IPv4 CIDR Block: 10.0.2.0/24 


 

Click Create Subnet. 

Why This Step? 

This subnet provides an additional public-facing segment for future use, such as load balancers or additional web resources. 

Why This Is a Great Step: 

Ensures flexibility and scalability for future deployments. 

Distributing resources across different availability zones enhances high availability. 

 

 

 

 

 

 

 

3. Create the Database Subnet (Private Subnet) 

Go to Subnets and click Create Subnet. 

Fill in the details:  

Name: Private_DB_Server 

VPC: Select megacorp-vpc 

Availability Zone: us-east-1b 

IPv4 CIDR Block: 10.0.3.0/24 

 

 
<img width="488" alt="2024-12-11 20_16_19-VPC _ us-east-1 - Brave" src="https://github.com/user-attachments/assets/9b06e59d-611e-406b-aacb-20d527cfb786" />

 

Click Create Subnet. 

Why This Step? 

This subnet is for database servers that should not be accessible from the internet for security reasons. 

Private subnets restrict public internet access, enhancing database security. 

Why This Is a Great Step: 

Protects sensitive data by isolating databases from the public network. 

Follows the principle of least privilege by limiting exposure to the internet. 

 

Step 3: Configure Internet Gateway for Public Subnets 

Go to VPC Dashboard > Internet Gateways. 

Click Create Internet Gateway.  

Name: megacorp-igw 

 

Attach the Internet Gateway to your VPC:  

Select MyIGW and click Actions > Attach to VPC. 

Choose megacorp-vpc. 

 

 

Why This Step? 

An Internet Gateway (IGW) allows traffic to flow between the public subnets and the internet. 

Why This Is a Great Step: 

Enables public-facing resources (like web servers) to serve client requests over the internet. 

Essential for creating a fully functional public subnet. 

 
<img width="960" alt="2024-12-11 22_23_08-" src="https://github.com/user-attachments/assets/9c4ba375-939e-4ace-af9c-0e8f68fde2f4" />

Step 4: Create a Route Table for Public Subnets 

Go to VPC Dashboard > Route Tables. 

Click Create Route Table.  

Name: megacorp-rt 

VPC: megacop-vpc  

Add a route to the internet:  

Select the route table, go to the Routes tab, and click Edit Routes. 

Add the following route:  

Destination: 0.0.0.0/0 

Target: MyIGW (Internet Gateway) 

 <img width="608" alt="2024-12-11 22_23_31-VPC _ us-east-1 - Brave" src="https://github.com/user-attachments/assets/49958691-5959-4d19-a478-4994e83daef3" />

<img width="960" alt="2024-12-11 22_27_02-" src="https://github.com/user-attachments/assets/e99d67d7-3782-4d47-9d02-9f39647a4f32" />

 

 

 

Click Save Routes. 

 

Why This Step? 

A route table directs traffic from your public subnet to the internet. 

Why This Is a Great Step: 

Ensures web servers and public resources can communicate with the internet. 

Provides clarity and control over network traffic. 

 <img width="605" alt="2024-12-11 22_31_34-VPC _ us-east-1 - Brave" src="https://github.com/user-attachments/assets/250659f6-24bc-4b88-9539-9b273afaf0f4" />


Step 5: Associate Public Subnets with the Public Route Table 

Select Public_Route_Table. 

Go to the Subnet Associations tab and click Edit Subnet Associations. 

Select:  

WEB_Servers 

DB_Server 

Click Save. 

<img width="950" alt="2024-12-11 22_33_55-VPC _ us-east-1 - Brave" src="https://github.com/user-attachments/assets/d38f1f08-7887-4dcf-8ede-de0fd55c3d36" />


 <img width="950" alt="2024-12-11 22_33_55-VPC _ us-east-1 - Brave" src="https://github.com/user-attachments/assets/488d7cdd-05ab-4570-ae11-d8535ab2255d" />


<img width="960" alt="2024-12-11 22_34_58-Greenshot" src="https://github.com/user-attachments/assets/93be8db1-0e64-44cf-9121-edd4ac9d9eaa" />

 

 

 <img width="960" alt="2024-12-11 22_35_15-" src="https://github.com/user-attachments/assets/e5c55d43-ff3a-4e06-b0fa-fe26ce4c30dc" />


 

Why This Step? 

Subnets must be explicitly associated with a route table for the routes to take effect. 

Why This Is a Great Step: 

Ensures that web servers in public subnets can route traffic to the internet. 

Facilitates organized and predictable network behavior. 

 

Step 6: Configure the Private Subnet 

Since the private subnet (Private_DB_Server) does not need direct internet access, it doesn't require an Internet Gateway. If you need outbound internet access (e.g., for updates), follow these steps: 

Create a NAT Gateway in the public subnet. 

 <img width="580" alt="2024-12-11 22_52_59-VPC _ us-east-1 - Brave" src="https://github.com/user-attachments/assets/12c15fc3-c7a4-479d-8909-494b3b47a03e" />


 

 

 

 

 

Add a route in the private subnet’s route table to direct outbound traffic to the NAT Gateway. 

 
<img width="657" alt="2024-12-11 22_46_49-VPC _ us-east-1 - Brave" src="https://github.com/user-attachments/assets/2fae2933-c4b0-4870-815e-7aee21e21d34" />

 <img width="959" alt="2024-12-11 22_53_37-" src="https://github.com/user-attachments/assets/e454209a-f1c8-440b-b892-1e5365c0eeff" />
<img width="954" alt="2024-12-11 23_03_49-VPC _ us-east-1 - Brave" src="https://github.com/user-attachments/assets/bb0a5e60-d4ec-49a5-adff-e11b4660ae88" />


 

Why This Step? 

A NAT Gateway allows instances in private subnets to access the internet while keeping them secure. 

Why This Is a Great Step: 

Provides controlled internet access for database servers without exposing them directly. 

Balances security and functionality. 

 

Step 7: Launch Resources 

Web Servers: Launch instances in the WEB_Servers subnet with public IP addresses. 

Database Instances: Launch instances in the Private_DB_Server subnet without public IP addresses. 

Why This Step? 

Deploying resources according to their roles ensures adherence to security best practices. 

Why This Is a Great Step: 

Isolating web and database layers enhances security and simplifies management. 

Ensures a scalable and secure infrastructure for your application. 

 

Summary of CIDR Blocks and Availability Zones 

Subnet Name 

CIDR Block 

Availability Zone 

WEB_Servers 

10.0.1.0/24 

us-east-1f 

Public_Subnet 

10.0.2.0/24 

us-east-1b 

Private_DB_Server 

10.0.3.0/24 

us-east-1c 

 

This guide ensures a secure, scalable, and highly available architecture for deploying web applications and databases. Each step aligns with best practices for cloud security and network design. 

 
