# 2 - Application Load Balancer & Multi-AZ EC2 

## Overview
I expaned my project by deploying multiple EC2 instances behind an Application Load Balancer (ALB) to handle incoming traffic efficiently, and also explored routing traffic using a Route53 DNS alias.

The goal was to understand load balancing, health checks, and secure access patterns in AWS.

## Steps 

### Launch EC2 Instances
My First step was to launch two EC2 Instances in different AZs. 

While setting up the instances for the ALB, I noticed that both of my existing subnets (public and private) were in the same AZ (eu-central-1a). As I wanted to launch the Instances in different AZs to follow best practices, I created an additional public subnet in a new AZ (eu-central-1b) and associated it with the public route table to allow Internet Access. 

To quickly test load balancing, I used a simple user-data script to install a web server.  

```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from ALB Instance 1</h1>" > /var/www/html/index.html
```

Eventually, I was able to launch the Instance in the created subnet in the new AZ (eu-central-1b)

![alt text](../Images/ALB_Instances.png)

Finally, try to reach the instances to see if it works!

![alt text](../Images/ALB_Instance_1.png)

![alt text](../Images/ALB_Instance_2.png)

What is the Purpose of different AZs?

- High Availability: If one AZ fails, resources in another AZ can continue running.
- Fault Tolerance: Reduces the risk of downtime due to hardware or network issues.


### Set up the ALB 

Next, I created an Application Load Balancer (ALB) across the two public subnets to distribute traffic evenly between the EC2 instances.
- Listener: HTTP (port 80)
- Security Group: ALB-http (allows HTTP traffic from anywhere)
- Target Group:
	- Target type: Instances
	- Protocol: HTTP, Port: 80
	- Health check path: /
	- Registered both EC2 instances

![alt text](../Images/Create_Targets.png)

![alt text](../Images/Resource_Map.png)

This ensures that the ALB only routes traffic to healthy instances and provides high availability across multiple AZs.


### Adjust Security Groups

Adjust the security groups to follow AWS best practices for an ALB setup:

- ALB Security Group: Allow HTTP (port 80) from anywhere, so the load balancer can receive traffic from the Internet.

- EC2 Security Groups: Allow HTTP (port 80) only from the ALB Security Group. This ensures that the EC2 instances are not directly accessible from the Internet.

- SSH Access: Allowed only from my own IP address for both instances.

This adjustment secures the EC2 instances and maintains traffic through the ALB.

EC2 Instances SG's:

![alt text](../Images/SG_Adjustment.png)


### Testing the ALB 

To verify that the Application Load Balancer works correctly:
- I Open the ALB DNS name in a browser.
- Refreshed the page multiple times to ensure traffic alternates between both EC2 instances.
- Confirm that health checks in the Target Group show both instances as healthy.

![alt text](../Images/Result.png)

![alt text](../Images/Health_Check.png)


### Bonus: Add Route53 DNS Name

- Created a public hosted zone in Route53: `naadir-alb-project-2026.com`
- Added an Alias record pointing to the ALB: `ALB-1-13514643.eu-central-1.elb.amazonaws.com`
- Note: The domain is not yet registered, so this DNS name will not resolve in the browser yet. The ALB can still be tested via its default AWS DNS name.

![alt text](<../Images/Create Record.png>)


### Conclusion and Learnings

I successfully set up multiple EC2 instances across different Availability Zones and put them behind an Application Load Balancer. I learned how to:
- Deploy EC2 instances in separate AZs for high availability and fault tolerance.
- Configure a simple web server using user-data scripts.
- Set up an ALB with listeners, target groups, and health checks to route traffic only to healthy instances.
- Adjust security groups to ensure instances are not directly exposed to the Internet, while allowing the ALB to communicate with them.
- Test the ALB to verify traffic distribution between instances.
- Configure a Route53 hosted zone and alias record to point to the ALB.