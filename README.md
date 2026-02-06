# aws-vpc-elb-autoscaling
This project demonstrates the design and implementation of a highly available and scalable AWS infrastructure using a custom VPC, public subnets across multiple Availability Zones, Application Load Balancer, and Auto Scaling Group to host Linux-based web servers.

# Architecture

<p align="center">
  <img src="Architecture Load.png" width="700">
</p>

Requirements:
•	Create a custom VPC
•	Create 2 public subnets in different Availability Zones
•	Launch Linux EC2 instances
•	Create Target Group
•	Create Application Load Balancer
•	Create Launch Template / Launch Configuration
•	Create Auto Scaling Group


VPC
	Open AWS Console → Navigate to VPC → Create VPC → Select VPC only 
→ Name: project-vpc → IPv4 CIDR block: 10.0.0.0/16 → Create VPC
