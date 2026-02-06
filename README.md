# aws-vpc-elb-autoscaling
This project demonstrates the design and implementation of a highly available and scalable AWS infrastructure using a custom VPC, public subnets across multiple Availability Zones, Application Load Balancer, and Auto Scaling Group to host Linux-based web servers.

# Architecture

<p align="center">
  <img src="Architecture Load.png" width="700">
</p>

## 📋 Requirements
- Create a custom VPC  
- Create 2 public subnets in different Availability Zones  
- Launch Linux EC2 instances  
- Create Target Group  
- Create Application Load Balancer  
- Create Launch Template / Launch Configuration  
- Create Auto Scaling Group  

---

## 🌐 VPC

➢ Open AWS Console → Navigate to **VPC** → Create VPC → Select **VPC only**  
→ Name: `project-vpc`  
→ IPv4 CIDR block: `10.0.0.0/16`  
→ Create VPC  

➢ Navigate to **Internet Gateways** → Create Internet Gateway  
→ Name: `project-igw`  
→ Attach Internet Gateway to `project-vpc`

---


## 🗂 Subnets

➢ Navigate to **Subnets** → Create Subnet  
→ VPC: `project-vpc`  

**Public Subnet 1**  
→ Name: `public-subnet-1`  
→ Availability Zone: `ap-south-1a`  
→ CIDR: `10.0.1.0/24`  

**Public Subnet 2**  
→ Name: `public-subnet-2`  
→ Availability Zone: `ap-south-1b`  
→ CIDR: `10.0.2.0/24`

---

## 🛣 Route Table

➢ Navigate to **Route Tables** → Create Route Table  
→ Name: `public-rt`  
→ VPC: `project-vpc`  

➢ Edit Routes  
→ Destination: `0.0.0.0/0`  
→ Target: Internet Gateway (`project-igw`)  

➢ Associate both public subnets with `public-rt`

---

## 🔐 Security Group

➢ Navigate to **EC2 → Security Groups** → Create Security Group  
→ Name: `web-sg`  
→ VPC: `project-vpc`

**Inbound Rules**
- HTTP → Port 80 → Source: `0.0.0.0/0`
- SSH → Port 22 → Source: `My IP`

---

## 🖥 EC2 Instances

➢ Navigate to **EC2** → Launch Instance  
→ Name: `WebServer`  
→ AMI: Amazon Linux 2  
→ Instance Type: `t3.micro`  
→ Key Pair: Select existing key  
→ Network: `project-vpc`  
→ Subnet: Public Subnet  
→ Security Group: `web-sg`  
→ Launch instance  

➢ Repeat the same steps to launch a second instance in another public subnet (different AZ)

### User Data Script
```bash
#!/bin/bash
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Server from $(hostname)</h1>" > /var/www/html/index.html
