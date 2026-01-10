# 1 - Custom VPC Architecture

## Overview
In this project, I built a custom AWS Virtual Private Cloud (VPC) from scratch.

The goal was to understand the basics of AWS networking, including subnets, routing, and secure Internet access.

The setup includes a public and a private subnet, an Internet Gateway, a NAT Gateway, and EC2 instances secured with security groups. 

## Steps 

### 1. Create a VPC 
In this step, I created a custom Virtual Private Cloud (VPC).
Although AWS provides a default VPC to make it beginner-friendly, I intentionally created a custom VPC to gain full control over the network infrastructure and configuration.

When creating a VPC, different options must be defined:

- Name Tag: First of all give the VPC a name tag, which helps for identifying the VPC and understanding the purpose of it. 

- IPv4 CIDR block: Defining an IPv4 CIDR block is mandatory, because it specifies the private IP address range available inside the VPC.

A CIDR block consists of two components:

    1. Base IP: Represents an IP contained in the range (XX.XX.XX.XX)
    2. Subnet Mask: Defines how many bits can change in the IP

I chose 10.0.0.0/16 which gave my VPC 65'536 different IP-Addresses

![alt text](../Images/VPC_Settings.png)

-> Finally, click “Create VPC” 

### 2. Create Subnets
Next, I created one public and one private subnet within my custom VPC, named "vpc-networking-lab". 
Subnets are a crucial part of a functional VPC because they define where resources are placed and allow separation between public and private network traffic.

For the private subnet, I configured the following:
- Name: private-subnet-01
- Availability Zone: Europe (Frankfurt) / eu-central-1a 
- IPv4 Subnet CIDR: 10.0.0.0/24 -> 256 IP-addresses

And for the Public Subnet:
- Name: public-subnet-01
- Availability Zone: Europe (Frankfurt) / eu-central-1a
- IPv4 CIDR: 10.0.1.0/24 

## 3. Configure Internet Access
To enable communication with the Internet, attach an Internet Gateway (IGW) to the VPC. 

![alt text](../Images/Create_IGW.png)
![alt text](../Images/Attach_IGW.png)

Next, create a NAT Gateway to allow instances in the private subnet to access the Internet while keeping them isolated from inbound traffic.

![alt text](../Images/NAT_Gateway.png)

### 4. Set up Route Tables
To control how traffic flows in the VPC, I created separate route tables:
1. A Public Route Table
2. A Private Route Table  

![alt text](../Images/Route_Tables.png)

Public Route Table:
- Route: 0.0.0.0/0 → Target: Internet Gateway (IGW)
- This allows all outbound traffic from the public subnet to reach the Internet.
- Subnet Association: public-subnet-01

![alt text](../Images/Edit_Public_Route_Table.png)

Private Route Table:
- Route: 0.0.0.0/0 → Target: NAT Gateway
- Purpose: Enables instances in the private subnet to access the Internet (e.g. for updates) while remaining isolated from inbound traffic.
- Subnet Association: private-subnet-01

![alt text](../Images/Edit_Private_Route_Table.png)

### Create EC2 Instances 
I deployed two EC2 instances:

- Public EC2: Placed in the public subnet with a public IP to allow direct Internet access.

![alt text](../Images/Public_Network_Settings.png)

- Security Group: Only allows SSH and HTTPS access from your own IP address for security. This ensures no one else can directly SSH into the instance.

![alt text](../Images/Public_SG.png)

- Private EC2: Placed in the private subnet without a public IP, accessing the Internet only via the NAT Gateway.

![alt text](../Images/Private_Network_Settings.png)

- Security Group: Only allows SSH traffic from my IP to keep the private instance isolated from the Internet. 

![alt text](../Images/Private_SG.png)

## Result
A successfully built, fully functional and secure AWS network architecture.

- A custom VPC with isolated public and private subnets
- Controlled Internet access using an Internet Gateway and NAT Gateway
- Routing through route tables
- Secure EC2 deployments with security group rules
